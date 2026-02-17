# AdventureWorks IT Runbook
### Internal IT Administration Reference
**Maintained by:** IT Infrastructure Team
**Environment:** adventureworks.local
**Last Updated:** February 2026

---

## A Note on This Document

This runbook covers the day-to-day tasks you'll actually run into managing the AdventureWorks environment. It's not meant to be exhaustive — it's meant to be useful. If something isn't in here, check with the team before making changes, especially anything touching Active Directory or file share permissions.

Everything in this document assumes you're logged into DC01 as a Domain Administrator unless otherwise noted.

---

## Quick Reference — IP Address Plan

| Device | Role | IP Address |
|--------|------|------------|
| DC01 | Domain Controller | 10.0.1.10 |
| FS01 | File Server / SQL Server | 10.0.1.11 |
| LINUX01 | Web / Samba Server | 10.0.1.20 |
| CLIENT-SALES | Sales Workstation | 10.0.20.10 |
| CLIENT-FIN | Finance Workstation | 10.0.30.10 |

**Domain:** adventureworks.local
**DNS:** DC01 (10.0.1.10)
**DHCP:** DC01 (10.0.1.10)

---

## Section 1 — User Account Management

### Creating a New Employee Account

When a new hire starts, you'll get a request from HR with the employee's name, department, and job title. Here's the process:

First, figure out which OU the user belongs to based on their department. The structure is:

- Manufacturing departments → `OU=Users,OU=Manufacturing`
- Sales, Marketing, Purchasing → `OU=Users,OU=Sales`
- Finance, HR, Executive → `OU=Users,OU=Corporate`
- IT staff → `OU=Users,OU=IT-Infrastructure`

Then create the account. The username format is first initial + last name (e.g., John Smith = jsmith):

```powershell
New-ADUser `
    -Name "John Smith" `
    -GivenName "John" `
    -Surname "Smith" `
    -SamAccountName "jsmith" `
    -UserPrincipalName "jsmith@adventureworks.local" `
    -Path "OU=Users,OU=Sales,DC=adventureworks,DC=local" `
    -Title "Sales Representative" `
    -Department "Sales" `
    -AccountPassword (ConvertTo-SecureString "Welcome2026!" -AsPlainText -Force) `
    -ChangePasswordAtLogon $true `
    -Enabled $true
```

After creating the account, add them to their department security group:

```powershell
Add-ADGroupMember -Identity "Sales-Users" -Members "jsmith"
```

**Don't forget** to also add them to the CompanyWide share group if your environment uses a dedicated group for that — currently all domain users have access, so no extra step needed there.

---

### Disabling a Departing Employee

When someone leaves the company, disable the account immediately — don't delete it. Deleting removes the SID, which can cause issues with file ownership and audit logs. HR will tell you when someone is leaving.

```powershell
# Disable the account
Disable-ADAccount -Identity "jsmith"

# Move to a Disabled Users OU (keeps things organized)
Get-ADUser -Identity "jsmith" | Move-ADObject -TargetPath "OU=Disabled,DC=adventureworks,DC=local"
```

If you haven't created a Disabled OU yet, do that first:

```powershell
New-ADOrganizationalUnit -Name "Disabled" -Path "DC=adventureworks,DC=local"
```

After 90 days, you can delete the account if no one has requested it be kept.

---

### Unlocking a Locked Account

After 5 failed login attempts, accounts lock automatically. This is the most common helpdesk call you'll get.

```powershell
# Check if locked
Get-ADUser -Identity "jsmith" -Properties LockedOut | Select-Object Name, LockedOut

# Unlock it
Unlock-ADAccount -Identity "jsmith"
```

---

### Resetting a Password

```powershell
Set-ADAccountPassword -Identity "jsmith" `
    -NewPassword (ConvertTo-SecureString "TempPass2026!" -AsPlainText -Force) `
    -Reset

# Force them to change it at next login
Set-ADUser -Identity "jsmith" -ChangePasswordAtLogon $true
```

---

### Finding a User's Group Memberships

Useful when troubleshooting access issues:

```powershell
Get-ADUser -Identity "jsmith" -Properties MemberOf | 
    Select-Object -ExpandProperty MemberOf
```

---

## Section 2 — File Share Management

### Checking Who Has Access to a Share

If a user says they can't access a folder, or you need to verify permissions are correct, run this on FS01:

```powershell
icacls "C:\Shares\Sales"
```

The output shows each group and their permission level. What you're looking for:
- **(OI)(CI)(F)** = Full Control, inherited by files and subfolders
- **(OI)(CI)(RX)** = Read and Execute
- If you see **BUILTIN\Users** with any access — that's a problem. Remove it.

---

### Granting a User Access to a Share

The right way to do this is through group membership, not individual permissions. Never add a single user directly to a folder's ACL — it becomes a nightmare to manage.

To give a user access to the Sales share, add them to Sales-Users:

```powershell
Add-ADGroupMember -Identity "Sales-Users" -Members "jsmith"
```

Group Policy refreshes every 90 minutes, but the user will need to log off and back on for the new token to take effect.

---

### Adding a New Shared Folder

If a department needs a new folder:

```powershell
# Create the folder on FS01
New-Item -Path "C:\Shares\Sales\Reports" -ItemType Directory

# Set permissions — replace Sales-Users with the appropriate group
icacls "C:\Shares\Sales\Reports" /grant "ADVENTUREWORKS\Sales-Users:(OI)(CI)(F)"
icacls "C:\Shares\Sales\Reports" /grant "ADVENTUREWORKS\IT-Users:(OI)(CI)(F)"
icacls "C:\Shares\Sales\Reports" /remove "BUILTIN\Users"
```

If it needs to be a new SMB share (not just a subfolder):

```powershell
New-SmbShare -Name "SalesReports" -Path "C:\Shares\Sales\Reports" -FullAccess "Everyone"
```

Keep Share Permissions set to Everyone — Full Control. Do all your actual security at the NTFS level.

---

## Section 3 — Domain Join & Workstations

### Joining a New Computer to the Domain

On the new workstation, open PowerShell as Administrator:

```powershell
Add-Computer -DomainName "adventureworks.local" `
    -Credential "ADVENTUREWORKS\Administrator" `
    -Restart
```

After it restarts, log into DC01 and move the computer account from the default Computers container to the correct OU:

```powershell
# Find it first
Get-ADComputer -Filter {Name -like "DESKTOP*"} | Select-Object Name, DistinguishedName

# Move it
Get-ADComputer -Identity "DESKTOP-XXXXXX" | 
    Move-ADObject -TargetPath "OU=Computers,OU=Sales,DC=adventureworks,DC=local"
```

Then force a GPO update on the machine:

```powershell
gpupdate /force
```

---

### Verifying Group Policy is Applying

If a user reports that a policy doesn't seem to be working — USB drive still works on a Sales machine, or screen isn't locking on Finance machines — check the GPO report:

```powershell
gpresult /H C:\GPReport.html /SCOPE COMPUTER /F
Start-Process "C:\GPReport.html"
```

Look under Applied GPOs. If the policy isn't listed, the computer account is probably in the wrong OU.

---

## Section 4 — DNS & DHCP

### Checking DHCP Scope Status

```powershell
Get-DhcpServerv4Scope | Select-Object Name, StartRange, EndRange, State
```

All 5 scopes should show as Active. If one is inactive, something went wrong — check the DHCP service is running:

```powershell
Get-Service DHCPServer
```

### Viewing Current DHCP Leases

Useful for finding out what IP a device got:

```powershell
Get-DhcpServerv4Lease -ScopeId 10.0.20.0 | Select-Object HostName, IPAddress, ClientId
```

Change the ScopeId to whichever VLAN you're looking at.

---

### Adding a DNS Record

If you add a new server or device and want it resolvable by name:

```powershell
# A Record
Add-DnsServerResourceRecordA `
    -Name "newserver" `
    -IPv4Address "10.0.1.25" `
    -ZoneName "adventureworks.local"

# Alias (CNAME)
Add-DnsServerResourceRecordCName `
    -Name "alias" `
    -HostNameAlias "newserver.adventureworks.local" `
    -ZoneName "adventureworks.local"
```

Verify it works:

```powershell
nslookup newserver.adventureworks.local
```

---

## Section 5 — General Troubleshooting

### User Can't Log In

Work through this in order:

1. Is the account enabled?
```powershell
Get-ADUser -Identity "jsmith" -Properties Enabled, LockedOut | Select-Object Name, Enabled, LockedOut
```

2. Is the computer joined to the domain and can it reach DC01?
```powershell
ping 10.0.1.10
```

3. Is DNS working on the client machine?
```powershell
nslookup adventureworks.local
```

4. Is the user's account in the correct OU? Check in ADUC or:
```powershell
Get-ADUser -Identity "jsmith" | Select-Object DistinguishedName
```

---

### User Can't Access a File Share

1. Confirm the user is in the correct security group
2. Have them log off and back on (group membership changes require a new token)
3. Check NTFS permissions on the share folder (FS01)
4. Verify the firewall isn't blocking port 445 between their VLAN and FS01

Test port connectivity from the client:
```powershell
Test-NetConnection -ComputerName 10.0.1.11 -Port 445
```

---

### Services to Check When Things Go Wrong

These are the services that matter most. If something's broken, check these first:

**On DC01:**
```powershell
Get-Service ADWS, DNS, DHCPServer, Netlogon, W32Time | Select-Object Name, Status
```

**On FS01:**
```powershell
Get-Service LanmanServer, MSSQLSERVER | Select-Object Name, Status
```

All should show Running. If anything is stopped, start it:
```powershell
Start-Service <ServiceName>
```

---

## Section 6 — Maintenance & Best Practices

### Before Making Any Changes

Take a snapshot. Always. Name it something descriptive like `DC01 - Before GPO Change - Feb 2026`. It takes 2 minutes and has saved hours of work more than once.

---

### Password Policy Reminder

Current policy enforces:
- Minimum 12 characters
- Complexity required (uppercase, lowercase, number or symbol)
- 90-day maximum age
- 1-day minimum age (prevents password cycling)
- Account locks after 5 failed attempts

Default temporary password for new accounts is `Welcome2026!` — users are forced to change at first login.

---

### Monthly Checks

- Review disabled accounts older than 90 days for deletion
- Check DHCP lease utilization per scope
- Verify all 5 DHCP scopes are active
- Review Event Viewer on DC01 for any errors (Event ID 4625 = failed logins worth watching)
- Confirm all VMs have recent snapshots

---

*For issues not covered here, check Microsoft Learn or escalate to senior IT staff.*
