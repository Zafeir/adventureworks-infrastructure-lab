# Adventureworks-infrastructure-lab

Enterprise IT infrastructure lab - Active Directory, file servers, GPOs, and automation scripts

A complete enterprise IT infrastructure lab simulating a bicycle manufacturing company with 290 employees across 4 divisions. Built to demonstrate hands-on experience with Active Directory, file servers, network segmentation, Group Policy, and PowerShell automation.

## 🎯 Project Overview

**Purpose:** Demonstrate real-world enterprise IT administration skills applicable to Junior Systems Engineer and IT Administrator roles.

**Environment:** 5 virtual machines running Windows Server 2022, Ubuntu Server, and Windows 10, networked across 5 VLANs using VirtualBox.

**Scope:** Complete infrastructure deployment from bare metal to production-ready environment with 290 real employee accounts imported from Microsoft's AdventureWorks sample database.

---

## 🏗️ Infrastructure Architecture

### Virtual Machines

| VM | Role | OS | IP Address | Services |
|----|------|----|-----------:|----------|
| DC01 | Domain Controller | Windows Server 2022 | 10.0.1.10 | Active Directory, DNS, DHCP |
| FS01 | File Server | Windows Server 2022 | 10.0.1.11 | SMB Shares, SQL Server 2022 |
| LINUX01 | Web/File Server | Ubuntu Server 24.04 | 10.0.1.20 | Apache, Samba |
| CLIENT-SALES | Sales Workstation | Windows 10 Pro | 10.0.20.10 | Domain-joined client |
| CLIENT-FIN | Finance Workstation | Windows 10 Pro | 10.0.30.10 | Domain-joined client |

### Network Segmentation

5 VLANs with DHCP scopes and firewall rules:

- **Management** (10.0.1.0/24) - Infrastructure servers
- **Manufacturing** (10.0.10.0/24) - 207 employees
- **Sales** (10.0.20.0/24) - 27 employees
- **Corporate** (10.0.30.0/24) - Finance, HR, Executive (23 employees)
- **IT-Infrastructure** (10.0.40.0/24) - 32 employees

### Active Directory Structure

```
adventureworks.local
├── Manufacturing OU (207 users)
│   ├── Users
│   ├── Groups (Manufacturing-Users)
│   └── Computers
├── Sales OU (27 users)
│   ├── Users
│   ├── Groups (Sales-Users)
│   └── Computers (CLIENT-SALES)
├── Corporate OU (23 users)
│   ├── Users
│   ├── Groups (Finance-Users, HR-Users, Executive-Users)
│   └── Computers (CLIENT-FIN)
└── IT-Infrastructure OU (32 users)
    ├── Users
    ├── Groups (IT-Users, IT-Admins)
    └── Computers
```

---

## 🔐 Security Implementation

### Group Policy Objects

| GPO | Scope | Configuration |
|-----|-------|---------------|
| Baseline Security Policy | Domain-wide | 12-char min password, 90-day expiry, 5-attempt lockout |
| Sales - USB Restrictions | Sales OU | All removable storage blocked |
| Finance - Screen Lock | Corporate OU | 10-minute inactivity timeout |

### File Share Permissions

Role-based access control (RBAC) using security groups and NTFS permissions:

- Manufacturing share → Manufacturing-Users (Full Control)
- Sales share → Sales-Users (Full Control)
- Finance share → Finance-Users + Executive-Users (Full Control)
- HR share → HR-Users + Executive-Users (Full Control)
- CompanyWide share → All Domain Users (Modify)

**Security hardening:**
- Removed BUILTIN\Users from all shares
- Corrected permission inheritance on Corporate subfolders
- SMB firewall rules restrict access to authorized VLANs only

---

## ⚙️ Automation Scripts

### 1. New-Employee.ps1
Automates new user provisioning:
- Creates AD account with proper naming convention
- Assigns to correct OU based on department
- Adds to appropriate security group
- Sets temporary password with forced change at first login

**Usage:**
```powershell
.\New-Employee.ps1 -FirstName "John" -LastName "Smith" -Department "Sales" -Title "Sales Representative"
```

### 2. AD-HealthCheck.ps1
Generates HTML health report including:
- AD service status (ADWS, DNS, DHCP, Netlogon)
- Locked user accounts
- Disabled accounts
- User distribution by division

**Usage:**
```powershell
.\AD-HealthCheck.ps1
# Opens HTML report in browser
```

### 3. DiskSpace-Monitor.ps1
Monitors disk space across servers:
- Checks DC01 and FS01
- Alerts if any drive below 20% free
- Generates report with per-server breakdown

**Usage:**
```powershell
.\DiskSpace-Monitor.ps1
# Default 20% threshold, configurable via -ThresholdPercent
```

### 4. Get-InactiveUsers.ps1
Security audit tool:
- Finds users inactive 90+ days
- Exports to CSV for review
- Groups by department for analysis

**Usage:**
```powershell
.\Get-InactiveUsers.ps1
# Outputs CSV report
```

---

## 📊 Technical Skills Demonstrated

### Windows Server Administration
- Active Directory Domain Services deployment and configuration
- DNS and DHCP server management
- File server role with SMB share configuration
- SQL Server 2022 installation and database restoration
- Group Policy creation and troubleshooting

### Networking
- VLAN design and implementation
- IP address planning and subnetting
- Inter-VLAN routing configuration
- Windows Firewall rule management
- Network troubleshooting (connectivity, DNS resolution)

### PowerShell Scripting
- User provisioning automation
- Active Directory queries and reporting
- WMI/CIM for system monitoring
- HTML report generation
- Error handling and parameter validation

### Security
- Role-based access control (RBAC) implementation
- NTFS vs Share permissions configuration
- Group Policy security hardening
- Account lockout policies
- Permission inheritance troubleshooting

### Linux Integration
- Ubuntu Server installation and configuration
- Apache web server deployment
- Samba file server for Windows integration
- Cross-platform file sharing

### Documentation
- Network topology diagrams
- Active Directory structure visualization
- IT runbook for common administrative tasks
- Technical process documentation

---

## 🛠️ Tools & Technologies

**Infrastructure:**
- VirtualBox 7.0
- Windows Server 2022
- Ubuntu Server 24.04
- Windows 10 Pro

**Services:**
- Active Directory Domain Services
- DNS Server
- DHCP Server
- SQL Server 2022 Developer Edition
- Apache 2.4
- Samba 4.x

**Scripting & Automation:**
- PowerShell 5.1
- PowerShell ISE
- SQL Server Management Studio

**Documentation:**
- Markdown
- SVG diagrams
- HTML reporting

---

## 📁 Repository Structure

```
├── Scripts/
│   ├── New-Employee.ps1
│   ├── AD-HealthCheck.ps1
│   ├── DiskSpace-Monitor.ps1
│   └── Get-InactiveUsers.ps1
├── Documentation/
│   ├── Network_Topology.svg
│   ├── AD_Structure.svg
│   ├── IT_Runbook.md
│   └── Test_Results.md
├── Syllabus.md
└── README.md
```

---

## 🎓 Learning Outcomes

This project required understanding and implementing:

1. **Active Directory fundamentals** - OU design, user/group management, domain join process
2. **DNS/DHCP services** - Forward lookup zones, DHCP scopes, DNS troubleshooting
3. **File server permissions** - NTFS vs Share permissions, group-based access control, inheritance
4. **Group Policy** - GPO creation, scope/linking, troubleshooting application with gpresult
5. **Network segmentation** - VLAN design, inter-VLAN routing, firewall rules
6. **PowerShell automation** - Scripting for user management, reporting, monitoring
7. **Troubleshooting methodology** - Systematic approach to connectivity, authentication, and permission issues
8. **Professional documentation** - Runbooks, diagrams, technical writing

---

## 🚀 How to Use This Repository

### For Employers/Recruiters
- **Scripts/** - Review automation code for PowerShell proficiency
- **Documentation/** - View network diagrams and runbook for documentation skills
- **Syllabus.md** - See full project scope and implementation details

### For IT Students/Lab Builders
This repository serves as a reference implementation. All scripts are functional and can be adapted to similar lab environments. The Syllabus.md provides a phase-by-phase build guide.

---

## 📝 Project Context

**Built for:** Job application portfolio demonstrating hands-on IT infrastructure skills

**Target Role:** Junior Systems Engineer / IT Administrator positions /Entry to Intermediate

**Time Investment:** ~22 hours over 2 week

**Dataset:** Microsoft AdventureWorks sample database (290 employees, 16 departments)

---

## 🔗 Contact

**LinkedIn:** https://www.linkedin.com/in/moises-o%E2%80%99neal-1ab8308b/
**Email:** Moises3874@gmail.com

---

## 📄 License

This project is provided as-is for educational and portfolio purposes. AdventureWorks is a Microsoft sample database used under their license terms.

---

## ⭐ Acknowledgments

- Microsoft AdventureWorks sample database
- VirtualBox by Oracle
- Ubuntu Server by Canonical
- PowerShell community resources
