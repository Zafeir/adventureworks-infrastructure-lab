# AdventureWorks Enterprise Infrastructure Lab
## Complete Syllabus & Learning Roadmap

---

## Project Overview

**What You're Building:**
A fully functional corporate IT infrastructure running in VirtualBox, managing 290 real employees from Microsoft's AdventureWorks sample company database. This simulates the exact environment you'd manage as a Junior Systems Engineer.

**End Result:**
- 5 virtual servers and workstations networked together
- Active Directory managing all user accounts
- File servers with department-based access controls
- Automated administration scripts
- Professional documentation portfolio

**Time Commitment:** 30-40 hours over 3-4 weeks
**Cost:** $0 (all free software)
**Difficulty:** Beginner to Intermediate

---

## Prerequisites

### Hardware Requirements
- **RAM:** 16GB minimum (20GB recommended)
- **Storage:** 200GB free disk space
- **Processor:** Modern CPU with virtualization support (Intel VT-x or AMD-V)
- **OS:** Windows 11 (your current setup)

### Software Requirements (You'll Download)
- VirtualBox (virtualization platform)
- Windows Server 2022 ISO
- Windows 10 ISO
- Ubuntu Server 22.04 ISO
- SQL Server 2022 Developer Edition
- AdventureWorks database backup
- Draw.io Desktop (for diagrams)

### Knowledge Requirements
**You DON'T need to know:**
- PowerShell scripting (you'll learn it)
- Networking concepts (you'll learn them)
- Active Directory (you'll learn it)
- Linux commands (you'll learn them)

**You DO need:**
- Basic computer literacy (you can navigate Windows, install software)
- Ability to follow detailed instructions
- Willingness to troubleshoot when things don't work perfectly
- Patience (some installations take 15-20 minutes)

---

## Key Terms & Acronyms

### Core Infrastructure Terms

**VM (Virtual Machine)**
- A software-based computer running inside your physical computer
- Acts like a completely separate computer with its own OS, IP address, hard drive
- You'll create 5 VMs for this project
- Think of it like running multiple computers on one physical machine

**VirtualBox**
- Free software that creates and manages VMs
- Made by Oracle
- Alternative to VMware or Hyper-V

**ISO File**
- Disk image file containing an operating system
- Like a virtual DVD/CD
- You'll use ISOs to install Windows and Linux on your VMs

**Snapshot**
- A saved state of a VM at a specific point in time
- Like a restore point
- If you break something, you can revert to the snapshot

### Active Directory Terms

**AD (Active Directory)**
- Microsoft's centralized user and computer management system
- Think of it as a corporate phone book + security system
- Stores all employee accounts, passwords, computer accounts
- One login works on any company computer

**DC (Domain Controller)**
- The server that hosts Active Directory
- The "brain" of the network
- All computers check with the DC to verify logins
- Usually named DC01, DC02, etc.

**Domain**
- The network that the DC manages
- Example: adventureworks.local
- All computers join this domain to be managed centrally

**OU (Organizational Unit)**
- A folder inside Active Directory
- Used to organize users, computers, groups
- Example: Sales OU contains all Sales department users
- Lets you apply different policies to different departments

**User Account**
- An employee's login credentials in Active Directory
- Includes username, password, email, department, etc.
- Created once, works on all domain computers

**Group**
- A collection of user accounts
- Used to assign permissions easily
- Example: "Sales Team" group gets access to Sales files
- Add/remove users from groups instead of managing individual permissions

**GPO (Group Policy Object)**
- A set of rules/settings applied to users or computers
- Examples: password requirements, software installations, security settings
- Applied to OUs to affect everyone in that OU

### Networking Terms

**IP Address**
- A unique number identifying a computer on a network
- Format: 10.0.1.10 (four numbers, 0-255 each)
- Like a street address for computers
- Two types: Static (manually set) or DHCP (automatically assigned)

**Subnet**
- A section of a network
- Example: 10.0.1.0/24 means all IPs from 10.0.1.1 to 10.0.1.254
- Used to organize and segment networks

**VLAN (Virtual Local Area Network)**
- A way to separate network traffic
- Like creating multiple isolated networks on the same physical network
- Example: Sales VLAN can't talk to HR VLAN (unless you allow it)
- Improves security and organization

**DHCP (Dynamic Host Configuration Protocol)**
- Service that automatically assigns IP addresses to computers
- When you connect to WiFi and "just works" - that's DHCP
- Saves you from manually configuring every device
- The DC will run DHCP for our network

**DNS (Domain Name System)**
- Translates names to IP addresses
- You type "google.com", DNS says "that's 142.250.80.46"
- The DC will run DNS for our network
- Critical for everything to work

**Gateway**
- The "door" out of a network
- Usually the router
- Example: 10.0.1.1 (typically the .1 address)
- Computers send external traffic to the gateway

**NAT (Network Address Translation)**
- Allows VMs to access the internet through your host computer
- VirtualBox feature we'll use
- Think of it like sharing your computer's internet connection

### File Server Terms

**SMB (Server Message Block)**
- Protocol Windows uses for file sharing
- When you access \\\\server\\share, you're using SMB
- Also called CIFS (Common Internet File System)

**File Share**
- A folder on a server that's accessible over the network
- Example: \\\\FS01\\Sales
- Users connect to shares to access company files

**NTFS Permissions**
- Security settings on files and folders stored on the disk
- Applied at the file system level
- Examples: Read, Write, Modify, Full Control

**Share Permissions**
- Security settings on the network share itself
- Applied when accessing over the network
- Work together with NTFS permissions (most restrictive wins)

**UNC Path (Universal Naming Convention)**
- The \\\\server\\share format
- How you access network resources
- Example: \\\\FS01\\Sales\\Reports

### Server Roles

**File Server**
- Server that stores and shares files
- Our FS01 will be the file server
- Hosts department folders and databases

**SQL Server**
- Database management system
- Stores structured data in tables
- We'll use it to host the AdventureWorks company database
- SQL = Structured Query Language

**Web Server**
- Serves web pages
- We'll use Apache on Linux for a company intranet
- Accessed via web browser (HTTP/HTTPS)

### PowerShell Terms

**PowerShell**
- Microsoft's command-line shell and scripting language
- More powerful than old Command Prompt
- Used to automate Windows administration
- Built into Windows

**Cmdlet (Command-let)**
- A PowerShell command
- Format: Verb-Noun
- Examples: Get-ADUser, New-Item, Set-Location
- Pronounce: "command-let"

**Parameter**
- An option you pass to a cmdlet
- Starts with a dash: -Name, -Path, -Identity
- Tells the command what to operate on or how to behave

**Pipeline**
- The | symbol
- Sends output from one command to another
- Example: Get-ADUser | Where-Object {$_.Department -eq "Sales"}

**Script**
- A file containing PowerShell commands
- Extension: .ps1
- Runs multiple commands in sequence
- Can be scheduled or run on-demand

### Linux Terms

**Ubuntu**
- A popular Linux distribution (version/flavor)
- Free and open source
- We'll use Ubuntu Server (no graphical interface)

**Bash**
- Linux's command-line shell
- Like PowerShell but for Linux
- Default shell on Ubuntu

**SSH (Secure Shell)**
- Protocol for remote login to Linux servers
- Encrypted connection
- How you'd manage Linux servers remotely

**Apache**
- Popular open-source web server software
- Runs on Linux
- We'll use it to host a company intranet page

**Samba**
- Software that lets Linux share files with Windows
- Implements SMB protocol on Linux
- Allows Windows users to access Linux file shares

### Acronyms Quick Reference

| Acronym | Full Name | What It Is |
|---------|-----------|------------|
| AD | Active Directory | User/computer management system |
| DC | Domain Controller | Server hosting Active Directory |
| OU | Organizational Unit | Folder in Active Directory |
| GPO | Group Policy Object | Set of configuration rules |
| VM | Virtual Machine | Software-based computer |
| VLAN | Virtual Local Area Network | Network segmentation |
| DHCP | Dynamic Host Configuration Protocol | Auto IP assignment |
| DNS | Domain Name System | Name to IP translation |
| SMB | Server Message Block | Windows file sharing protocol |
| UNC | Universal Naming Convention | \\\\server\\share format |
| SQL | Structured Query Language | Database query language |
| SSH | Secure Shell | Remote Linux access |
| NAT | Network Address Translation | Internet sharing for VMs |
| IP | Internet Protocol | Network addressing |
| NTFS | New Technology File System | Windows file system |
| ISO | International Organization for Standardization | Disk image file format |

---

## Project Phases

### Phase 0: Environment Setup (2-3 hours)
**What You'll Do:**
- Download all required software and ISOs
- Create project folder structure
- Install VirtualBox and Draw.io
- Create initial network diagram
- Set up VirtualBox networks

**What You'll Learn:**
- How to organize IT projects professionally
- Basic network planning
- VirtualBox network configuration
- Network diagramming

**Deliverables:**
- All ISOs downloaded
- VirtualBox networks configured
- Initial network topology diagram

---

### Phase 1: Domain Controller (DC01) (4-5 hours)
**What You'll Do:**
- Create DC01 virtual machine
- Install Windows Server 2022
- Configure static IP addressing
- Install Active Directory Domain Services
- Promote server to Domain Controller
- Install and configure DNS
- Install and configure DHCP

**What You'll Learn:**
- Windows Server installation
- Static IP configuration
- What Active Directory does and why it's needed
- DNS fundamentals
- DHCP fundamentals
- PowerShell basics

**Key Concepts:**
- Domain vs Workgroup
- What "promoting" a server means
- How DNS resolves names
- How DHCP assigns IPs
- Forest, Tree, Domain hierarchy

**Deliverables:**
- Functioning Domain Controller
- adventureworks.local domain created
- DNS and DHCP services running

---

### Phase 2: File Server (FS01) (3-4 hours)
**What You'll Do:**
- Create FS01 virtual machine
- Install Windows Server 2022
- Join server to adventureworks.local domain
- Install File Server role
- Install SQL Server 2022
- Restore AdventureWorks database

**What You'll Learn:**
- Joining computers to a domain
- File Server role and capabilities
- SQL Server basics
- Database restoration
- How to verify services are running

**Key Concepts:**
- Domain-joined vs standalone
- SQL Server instances
- Database backups and restores
- Service accounts

**Deliverables:**
- FS01 joined to domain
- SQL Server installed
- AdventureWorks database restored and accessible

---

### Phase 3: Explore AdventureWorks Data (2 hours)
**What You'll Do:**
- Connect to SQL Server with Management Studio
- Explore database structure
- View employee tables
- Understand department organization
- Export employee data to CSV
- Analyze what data we have to work with

**What You'll Learn:**
- SQL Server Management Studio basics
- Simple SQL SELECT queries
- Database table relationships
- How to export data
- Planning based on actual data (not assumptions)

**Key Concepts:**
- Tables, rows, columns
- Primary keys and foreign keys
- JOINs between tables
- CSV exports

**Deliverables:**
- Understanding of AdventureWorks structure
- Employee data exported
- Department list identified
- Data plan for AD import

---

### Phase 4: Active Directory Structure (3-4 hours)
**What You'll Do:**
- Create OU structure for departments
- Create security groups for each department
- Import 290 employees as AD users (via PowerShell)
- Verify user accounts created
- Test user login

**What You'll Learn:**
- OU design and planning
- Security group strategy
- PowerShell scripting for bulk operations
- CSV imports
- User account attributes

**Key Concepts:**
- OU vs Group (when to use each)
- Global vs Domain Local groups
- User Principal Name (UPN)
- SamAccountName vs UPN
- Account passwords and policies

**Deliverables:**
- OU structure for all departments
- Security groups created
- 290 user accounts imported
- Users organized by department

---

### Phase 5: File Shares & Permissions (3-4 hours)
**What You'll Do:**
- Create folder structure for departments
- Extract real data from AdventureWorks database
- Create SMB shares for each department
- Configure NTFS permissions
- Configure Share permissions
- Test access from different users

**What You'll Learn:**
- File server folder structure design
- NTFS vs Share permissions
- Permission inheritance
- Access Control Lists (ACLs)
- How permissions combine
- SQL queries to extract business data

**Key Concepts:**
- Read vs Modify vs Full Control
- Deny vs Allow permissions
- Most restrictive permission wins
- Group-based permissions vs individual

**Deliverables:**
- Department folder structure
- Real company data in shares
- Properly configured permissions
- Tested and verified access

---

### Phase 6: Linux Server (LINUX01) (2-3 hours)
**What You'll Do:**
- Create LINUX01 virtual machine
- Install Ubuntu Server
- Configure static IP
- Install Apache web server
- Install Samba file server
- Create company intranet page
- Test access from Windows clients

**What You'll Learn:**
- Linux installation
- Linux networking configuration
- Basic Linux commands (ls, cd, nano, sudo)
- Apache configuration
- Samba configuration
- Cross-platform file sharing

**Key Concepts:**
- Linux file permissions (rwx)
- sudo and root user
- systemd services
- Package management (apt)
- Configuration file editing

**Deliverables:**
- Ubuntu server running
- Apache hosting intranet page
- Samba sharing files with Windows
- Cross-platform integration working

---

### Phase 7: Client Workstations (3-4 hours)
**What You'll Do:**
- Create CLIENT-SALES VM
- Install Windows 10
- Join to domain
- Place on Sales VLAN
- Create CLIENT-FIN VM
- Repeat for Finance VLAN
- Test user logins
- Test file share access

**What You'll Learn:**
- Windows 10 domain join process
- VLAN assignment
- User profile creation
- Drive mapping
- Network troubleshooting

**Key Concepts:**
- Local user vs Domain user
- Roaming profiles
- Group Policy application to clients
- Network location awareness

**Deliverables:**
- Two client workstations
- Domain-joined and on correct VLANs
- Users can login and access shares
- Verified network segmentation

---

### Phase 8: DHCP & DNS Configuration (2-3 hours)
**What You'll Do:**
- Create DHCP scopes for all VLANs
- Configure DHCP options (DNS, gateway)
- Create DNS A records for all servers
- Create DNS CNAME aliases
- Test DHCP from clients
- Test DNS resolution

**What You'll Learn:**
- DHCP scope planning
- DHCP options and their purposes
- DNS record types (A, CNAME, PTR)
- Forward vs Reverse lookup zones
- DNS troubleshooting with nslookup

**Key Concepts:**
- Scope vs Reservation
- Lease duration
- DNS suffix search order
- Split-brain DNS

**Deliverables:**
- DHCP scopes for all VLANs
- Clients receiving IPs automatically
- All servers resolvable by name
- DNS aliases working

---

### Phase 9: Firewall Rules (2 hours)
**What You'll Do:**
- Configure Windows Firewall on FS01
- Create rules allowing specific VLANs
- Block unauthorized access
- Test connectivity between VLANs
- Verify restrictions work

**What You'll Learn:**
- Windows Firewall configuration
- Inbound vs Outbound rules
- Port numbers and protocols
- Rule priority and evaluation
- Security through network segmentation

**Key Concepts:**
- Stateful firewall
- Allow vs Block rules
- Source/Destination addressing
- Common ports (445 SMB, 80 HTTP, 443 HTTPS)

**Deliverables:**
- Firewall rules on file server
- Tested and verified access controls
- Network security improved

---

### Phase 10: Group Policy (3-4 hours)
**What You'll Do:**
- Create baseline security GPO
- Configure password policy
- Create department-specific GPOs
- Configure USB restrictions for Sales
- Configure screen lock for Finance
- Test policy application
- Troubleshoot policy issues

**What You'll Learn:**
- Group Policy architecture
- Computer vs User policies
- Policy precedence and inheritance
- gpupdate and gpresult commands
- Common policy settings

**Key Concepts:**
- LSDOU (Local, Site, Domain, OU)
- Policy inheritance
- Enforced policies
- Block inheritance
- Loopback processing

**Deliverables:**
- Baseline security policy applied
- Department policies working
- Tested and verified restrictions
- GPO documentation

---

### Phase 11: Testing & Validation (2-3 hours)
**What You'll Do:**
- Test user authentication
- Test file share access from multiple users
- Test network connectivity



- Test Group Policy application
- Test DHCP and DNS
- Document all test results

**What You'll Learn:**
- Systematic testing methodology
- Test case creation
- Result documentation
- Troubleshooting failed tests

**Key Concepts:**
- Expected vs Actual results
- Test coverage
- Regression testing

**Deliverables:**
- Complete test results document
- Verified all functionality
- Identified and fixed issues

---

### Phase 12: Documentation (4-5 hours)
**What You'll Do:**
- Complete network topology diagram
- Create Active Directory structure diagram
- Create permissions matrix
- Write IT runbook for common tasks
- Document all IP addresses
- Create disaster recovery notes

**What You'll Learn:**
- Professional IT documentation standards
- Network diagramming tools and symbols
- Runbook creation
- Knowledge transfer documentation

**Key Concepts:**
- Documentation as code
- Diagrams as documentation
- Runbooks vs procedures
- Version control for documentation

**Deliverables:**







- Complete network diagram
- AD structure diagram
- Permissions matrix
- IT runbook
- Disaster recovery plan

---

### Phase 13: Automation & Scripting (4-5 hours)
**What You'll Do:**
- Create AD health check script
- Create disk space monitoring script
- Create automated user provisioning script
- Create automated reporting script
- Schedule scripts with Task Scheduler
- Test all automation

**What You'll Learn:**
- PowerShell scripting fundamentals
- Variables, loops, conditionals
- Functions and parameters
- Error handling
- Task Scheduler
- HTML report generation

**Key Concepts:**
- DRY (Don't Repeat Yourself)
- Idempotency
- Scheduled tasks
- Script parameters
- Script logging

**Deliverables:**
- 4+ working PowerShell scripts
- Scheduled automation tasks
- Script documentation
- Tested and verified automation

---

### Phase 14: Portfolio & GitHub (2-3 hours)
**What You'll Do:**
- Create GitHub account (if needed)
- Create repository for project
- Upload all scripts
- Upload all documentation
- Write comprehensive README
- Add screenshots and diagrams
- Make repository public

**What You'll Learn:**
- Git basics
- GitHub repository management
- Markdown formatting
- Portfolio presentation
- README best practices

**Key Concepts:**
- Version control
- Repository structure
- Documentation for others
- Professional presentation

**Deliverables:**
- Public GitHub repository
- All project files uploaded
- Professional README
- Portfolio-ready project

---

## Skills Matrix

### Technical Skills You'll Gain

| Skill Category | Specific Skills | Proficiency Level |
|----------------|-----------------|-------------------|
| **Windows Server** | Installation, Configuration, Role Management | Intermediate |
| **Active Directory** | User/Group Management, OU Design, Schema Understanding | Intermediate |
| **Group Policy** | GPO Creation, Configuration, Troubleshooting | Beginner-Intermediate |
| **Networking** | IP Addressing, Subnetting, VLANs, DNS, DHCP | Intermediate |
| **File Services** | Share Creation, NTFS Permissions, Access Control | Intermediate |
| **PowerShell** | Scripting, Automation, Cmdlets, Pipelines | Beginner-Intermediate |
| **Linux** | Ubuntu Administration, Apache, Samba, Bash Basics | Beginner |
| **SQL Server** | Installation, Database Restoration, Basic Queries | Beginner |
| **Virtualization** | VirtualBox, VM Management, Snapshots | Intermediate |
| **Documentation** | Diagramming, Runbooks, Technical Writing | Intermediate |

### Soft Skills You'll Develop

- **Problem-solving:** Troubleshooting failed installations and configurations
- **Attention to detail:** Following complex multi-step processes
- **Project management:** Tracking progress through phases
- **Self-directed learning:** Reading documentation and figuring things out
- **Technical communication:** Documenting your work clearly
- **Persistence:** Working through challenges and errors

---

## Learning Resources

### Official Documentation
- **Microsoft Learn:** https://learn.microsoft.com
  - Active Directory documentation
  - PowerShell documentation
  - Windows Server guides
  
- **Ubuntu Documentation:** https://ubuntu.com/server/docs
  - Ubuntu Server guide
  - Package management
  - System administration

- **VirtualBox Manual:** https://www.virtualbox.org/manual/
  - Networking configuration
  - VM management

### Community Resources
- **Reddit:**
  - r/sysadmin - Professional sysadmin discussion
  - r/PowerShell - PowerShell help and examples
  - r/homelab - Home lab projects and ideas

- **Stack Overflow / Server Fault**
  - Q&A for specific technical problems
  - Search before asking

- **YouTube Channels:**
  - Professor Messer (for certification prep)
  - PowerShell.org
  - Learn Linux TV

### Tools & Utilities
- **PowerShell ISE** - Built into Windows, for script writing
- **Visual Studio Code** - Better code editor (free)
- **Windows Terminal** - Modern terminal (free)
- **SQL Server Management Studio** - Database management (free)
- **Draw.io** - Diagramming tool (free)

---

## Troubleshooting Guide

### Common Issues & Solutions

**VirtualBox Won't Start VMs**
- Cause: Virtualization not enabled in BIOS
- Solution: Reboot, enter BIOS, enable Intel VT-x or AMD-V

**VMs Can't Access Internet**
- Cause: NAT adapter not configured
- Solution: Check Adapter 1 is set to NAT in VM settings

**Can't Join Computer to Domain**
- Cause: DNS not pointing to Domain Controller
- Solution: Set DNS to DC's IP (10.0.1.10)

**Users Can't Login to Domain**
- Cause: Computer not joined to domain or DC unreachable
- Solution: Verify computer is domain-joined, ping DC, check DNS

**File Shares Not Accessible**
- Cause: Permissions or firewall blocking
- Solution: Check both Share and NTFS permissions, verify firewall rules

**PowerShell Scripts Won't Run**
- Cause: Execution policy restriction
- Solution: Run `Set-ExecutionPolicy RemoteSigned` as admin

**DHCP Not Assigning IPs**
- Cause: DHCP service not running or scope not configured
- Solution: Check service status, verify scope exists and is active

**DNS Not Resolving Names**
- Cause: DNS service not running or records missing
- Solution: Check service, verify A records exist, check forwarders

### When to Take Snapshots

**Take a snapshot:**
- Before major configuration changes
- After completing each phase successfully
- Before running untested scripts
- Before making permission changes

**Snapshot naming convention:**
```
VM-Name - Phase - Description
Examples:
DC01 - Phase1 - Base Install Complete
DC01 - Phase4 - AD Populated
FS01 - Phase5 - Shares Configured
```

---

## Assessment Checkpoints

### Phase Completion Criteria

After each phase, verify you can answer these questions:

**Phase 1 - Domain Controller:**
- [ ] What does a Domain Controller do?
- [ ] What is the difference between DNS and DHCP?
- [ ] What forest/domain did you create?
- [ ] Can you ping the DC from your host computer?

**Phase 4 - Active Directory:**
- [ ] How many OUs did you create?
- [ ] How many users are in your domain?
- [ ] What's the difference between an OU and a Group?
- [ ] How would you find all users in the Sales department?

**Phase 5 - File Shares:**
- [ ] What's the difference between NTFS and Share permissions?
- [ ] Which permission is more restrictive if they conflict?
- [ ] Can you explain your permission strategy?
- [ ] What groups have access to the Finance share?

**Phase 10 - Group Policy:**
- [ ] What is a GPO?
- [ ] How does GPO inheritance work?
- [ ] What's the difference between Computer and User policies?
- [ ] How do you force a GPO update?

**Phase 13 - Automation:**
- [ ] What does your user provisioning script do?
- [ ] How would you modify it to add a new parameter?
- [ ] How do you schedule a script to run automatically?
- [ ] What error handling did you implement?

---

## Project Milestones

### Week 1: Foundation
- [ ] Environment setup complete
- [ ] DC01 installed and configured
- [ ] FS01 installed and configured
- [ ] AdventureWorks database restored

### Week 2: Population
- [ ] Active Directory structure created
- [ ] 290 users imported
- [ ] Security groups configured
- [ ] File shares created with permissions

### Week 3: Integration
- [ ] Linux server operational
- [ ] Client workstations joined
- [ ] DHCP and DNS fully configured
- [ ] Group Policies applied

### Week 4: Finalization
- [ ] Testing complete
- [ ] Documentation finished
- [ ] Automation scripts working
- [ ] GitHub repository published

---

## Resume & Interview Preparation

### How to Describe This Project

**Elevator Pitch (30 seconds):**
"I built a complete enterprise IT infrastructure lab with 5 servers managing 290 users across multiple departments. I configured Active Directory, implemented network segmentation with VLANs, created automated administration scripts in PowerShell, and integrated Windows and Linux systems. The entire environment is documented and available on my GitHub."

**Resume Bullet Points:**
```
Enterprise IT Infrastructure Lab Project
- Designed and deployed multi-server Windows domain environment supporting 290+ users
- Configured Active Directory, Group Policy, DHCP, and DNS across 5 network VLANs
- Implemented role-based access control with department-specific file shares and permissions
- Automated user provisioning and system monitoring using PowerShell scripting
- Integrated Linux services (Apache, Samba) with Windows Server infrastructure
- Created comprehensive network documentation, diagrams, and administrative runbooks
- Technologies: Windows Server 2022, Active Directory, PowerShell, Ubuntu, SQL Server, VirtualBox
```

### Interview Talking Points

**When asked about Active Directory:**
"In my lab project, I managed 290 user accounts across 16 departments. I designed an OU structure that mirrored the company's organizational hierarchy, created security groups for each department with different access levels, and used Group Policy to enforce security settings like password complexity and screen lock timeouts."

**When asked about automation:**
"I wrote PowerShell scripts to automate common administrative tasks. My user provisioning script creates AD accounts, assigns them to the correct groups and OUs, sets up their home folder with proper permissions, and configures their profile - all in one command. I also created monitoring scripts that check disk space and AD health, generating HTML reports that could be emailed to administrators."

**When asked about troubleshooting:**
"When users couldn't access file shares, I'd check three things: First, verify the user is in the correct security group. Second, check both NTFS and share permissions - remembering that the most restrictive applies. Third, check if Group Policy is blocking access. I documented this troubleshooting process in my runbook."

**When asked about networking:**
"I implemented network segmentation using VLANs - Sales, Finance, HR, and IT each had their own subnet. I configured DHCP scopes for each VLAN, set up Windows Firewall rules to control traffic between them, and used DNS for name resolution. This improved security by ensuring departments could only access the resources they needed."

---

## Next Steps After Completion

### Expand Your Lab

**Add These Components:**
1. **Exchange Server** - Corporate email system
2. **WSUS (Windows Server Update Services)** - Centralized patch management
3. **Certificate Authority** - PKI infrastructure
4. **VPN Server** - Remote access
5. **Backup Solution** - Veeam or Windows Server Backup
6. **Monitoring** - Nagios, Zabbix, or PRTG
7. **Second Domain Controller** - High availability
8. **Read-Only Domain Controller (RODC)** - Branch office simulation

### Pursue Certifications

**Recommended Path:**
1. **CompTIA A+** - Hardware and OS basics (if needed)
2. **CompTIA Network+** - Networking fundamentals
3. **CompTIA Security+** - Security concepts (you're working on this)
4. **Microsoft MD-102** - Endpoint Administrator
5. **Microsoft AZ-104** - Azure Administrator (you have AZ-900)
6. **Microsoft MS-102** - Microsoft 365 Administrator

### Continue Learning

**Areas to Explore:**
- **Containers:** Docker and Kubernetes
- **Cloud:** Azure, AWS, or Google Cloud
- **Infrastructure as Code:** Terraform, Ansible
- **CI/CD:** GitHub Actions, Azure DevOps
- **Advanced PowerShell:** Desired State Configuration (DSC)
- **Python:** For cross-platform automation
- **Cybersecurity:** Penetration testing, blue team tools

---

## Project File Structure
```
C:\LabProject\
├── ISOs\
│   ├── WindowsServer2022.iso
│   ├── Windows10.iso
│   ├── ubuntu-22.04-server.iso
│   ├── SQL2022-SSEI-Dev.exe
│   └── AdventureWorks.bak
│
├── Scripts\
│   ├── Import-ADUsers.ps1
│   ├── New-Employee.ps1
│   ├── AD-HealthCheck.ps1
│   ├── DiskSpace-Monitor.ps1
│   └── (other automation scripts)
│
├── Documentation\
│   ├── IP_Plan.txt
│   ├── Network_Topology.png
│   ├── AD_Structure.png
│   ├── Permissions_Matrix.txt
│   ├── IT_Runbook.txt
│   ├── Access_Test_Results.txt
│   └── Project_Notes.md
│
├── Backups\
│   └── (VM snapshots via VirtualBox)
│
└── Syllabus.md (this file)
```

---

## Support & Help

### When You Get Stuck

**1. Check the troubleshooting guide** in this syllabus

**2. Review the lesson** - did you miss a step?

**3. Check your work:**
- Is the VM running?
- Is the service started?
- Are you on the right network?
- Did you type the command correctly?

**4. Use built-in help:**
```powershell
Get-Help <cmdlet-name> -Full
Get-Help <cmdlet-name> -Examples
```

**5. Search online:**
- Error messages: Copy exact error, search in quotes
- Concepts: Search "how does [thing] work"
- Examples: Search "[thing] PowerShell examples"

**6. Community resources:**
- Reddit r/sysadmin, r/PowerShell
- Server Fault (like Stack Overflow for IT)
- Microsoft Q&A forums

### Good Questions to Ask

**Bad question:**
"It doesn't work, help?"

**Good question:**
"I'm trying to join CLIENT-SALES to the domain (Phase 7, Step 3). I get error 'The specified domain either does not exist or could not be contacted.' I verified the DNS is set to 10.0.1.10, I can ping DC01 successfully, and the domain name is adventureworks.local. What else should I check?"

**Include:**
- What you're trying to do
- What you expected to happen
- What actually happened (exact error message)
- What you've already tried
- Your configuration details

---

## Success Criteria

### You've Successfully Completed This Project When:

**Technical Achievements:**
- [X] All 5 VMs are running and networked
- [X] 290 users can authenticate to the domain
- [X] File shares work with proper permissions
- [X] DHCP assigns IPs to clients
- [X] DNS resolves all server names
- [X] Group Policies apply correctly
- [X] Linux server serves web pages and files
- [X] Automation scripts run successfully

**Learning Achievements:**
- [X] You can explain what Active Directory does
- [X] You can describe your network topology
- [X] You can write basic PowerShell scripts
- [X] You can troubleshoot common AD/network issues
- [ ] You can create new users without following instructions
- [X] You understand NTFS vs Share permissions
- [X] You can read and understand your own scripts

**Professional Achievements:**
- [ ] Complete documentation exists
- [ ] Network diagrams are professional quality
- [ ] GitHub repository is public and well-organized
- [ ] You can confidently discuss this project in interviews
- [ ] You have working code samples to show employers

---

## Estimated Timeline

### Full-Time (8 hours/day)
- Week 1: Complete
- Total: 5 working days

### Part-Time (2 hours/day)
- Week 1-2: Phases 0-4
- Week 3-4: Phases 5-9
- Week 5: Phases 10-14
- Total: 4-5 weeks

### Weekend Warrior (8 hours/weekend)
- Weekend 1-2: Phases 0-4
- Weekend 3-4: Phases 5-9
- Weekend 5: Phases 10-14
- Total: 5 weekends

**Pace yourself.** Quality learning is more important than speed.

---

## Final Notes

**This is a learning project.** You will make mistakes. Things will break. That's expected and valuable - troubleshooting is where real learning happens.

**Ask questions.** If something doesn't make sense, stop and ask. Don't just copy/paste commands you don't understand.

**Document as you go.** Take notes about what you learn, what problems you encountered, how you solved them. This becomes your personal knowledge base.

**Have fun.** Yes, this is serious professional development, but building your own network is genuinely cool. Enjoy the process.

**You've got this.** Thousands of people have learned these skills. There's no magic knowledge required - just patience, persistence, and willingness to learn from mistakes.

---

## Version History

- **v1.0** - Initial syllabus creation
- Living document - will be updated as we work through phases together

---

**Ready to begin? Save this file and let's start with Phase 0.**

# AdventureWorks Enterprise Infrastructure Lab
## Complete Syllabus & Learning Roadmap
### Last Updated: February 2026

---

## Project Overview

**What You're Building:**
A fully functional corporate IT infrastructure running in VirtualBox, managing 290 real employees from Microsoft's AdventureWorks sample company database. This simulates the exact environment you'd manage as a Junior Systems Engineer.

**End Result:**
- 5 virtual servers and workstations networked together
- Active Directory managing all user accounts
- File servers with department-based access controls
- Automated administration scripts
- Professional documentation portfolio

**Time Commitment:** 30-40 hours over 3-4 weeks
**Cost:** $0 (all free software)
**Difficulty:** Beginner to Intermediate

---

## Project Status

| Phase | Name | Status |
|-------|------|--------|
| 0 | Environment Setup | ✅ Complete |
| 1 | Domain Controller (DC01) | ✅ Complete |
| 2 | File Server (FS01) | ✅ Complete |
| 3 | Explore AdventureWorks Data | ✅ Complete |
| 4 | Active Directory Structure | ✅ Complete |
| 5 | File Shares & Permissions | ✅ Complete |
| 6 | Linux Server (LINUX01) | ⚠️ Partial - Built, not domain integrated |
| 7 | Client Workstations | ✅ Complete |
| 8 | DHCP & DNS Configuration | ✅ Complete |
| 9 | Firewall Rules | ✅ Complete |
| 10 | Group Policy | ✅ Complete |
| 11 | Testing & Validation | ✅ Complete |
| 12 | Documentation | 🔄 In Progress |
| 13 | Automation & Scripting | ⏳ Remaining |
| 14 | Portfolio & GitHub | ⏳ Remaining |

---

## Infrastructure Summary

### Virtual Machines Built

| VM | Role | IP | OS | Status |
|----|------|----|----|--------|
| DC01 | Domain Controller | 10.0.1.10 | Windows Server 2022 | ✅ Running |
| FS01 | File Server / SQL Server | 10.0.1.11 | Windows Server 2022 | ✅ Running |
| LINUX01 | Web / File Server | 10.0.1.20 | Ubuntu Server 24.04 | ⚠️ Partial |
| CLIENT-SALES | Sales Workstation | 10.0.20.10 | Windows 10 Pro | ✅ Running |
| CLIENT-FIN | Finance Workstation | 10.0.30.10 | Windows 10 Pro | ✅ Running |

### Network Layout

| VLAN | Network | Purpose |
|------|---------|---------|
| Management | 10.0.1.0/24 | Servers |
| Manufacturing | 10.0.10.0/24 | Manufacturing dept |
| Sales | 10.0.20.0/24 | Sales dept |
| Corporate | 10.0.30.0/24 | Finance, HR, Executive |
| IT-Infrastructure | 10.0.40.0/24 | IT dept |

### Active Directory

- **Domain:** adventureworks.local
- **Users:** 290 employees imported from AdventureWorks database
- **Divisions:** 4 (Manufacturing, Sales, Corporate, IT-Infrastructure)
- **Departments:** 16
- **Security Groups:** 8 (Manufacturing-Users, Sales-Users, Corporate-Users, IT-Users, Finance-Users, HR-Users, Executive-Users, IT-Admins)

### File Shares

| Share | Path | Access |
|-------|------|--------|
| Manufacturing | \\FS01\Manufacturing | Manufacturing-Users |
| Sales | \\FS01\Sales | Sales-Users |
| Corporate | \\FS01\Corporate | Corporate-Users |
| Corporate\Finance | \\FS01\Corporate\Finance | Finance-Users, Executive-Users |
| Corporate\HR | \\FS01\Corporate\HR | HR-Users, Executive-Users |
| Corporate\Executive | \\FS01\Corporate\Executive | Executive-Users |
| IT-Infrastructure | \\FS01\IT-Infrastructure | IT-Users |
| CompanyWide | \\FS01\CompanyWide | All Domain Users |

### Group Policies Applied

| GPO | Scope | Settings |
|-----|-------|---------|
| Baseline Security Policy | Entire Domain | 12 char min password, 90 day expiry, 5 attempt lockout |
| Sales - USB Restrictions | Sales OU | All removable storage denied |
| Finance - Screen Lock | Corporate OU | 10 minute inactivity lock |

---

## Key Terms & Acronyms

| Acronym | Full Name | What It Is |
|---------|-----------|------------|
| AD | Active Directory | User/computer management system |
| DC | Domain Controller | Server hosting Active Directory |
| OU | Organizational Unit | Folder in Active Directory |
| GPO | Group Policy Object | Set of configuration rules |
| VM | Virtual Machine | Software-based computer |
| VLAN | Virtual Local Area Network | Network segmentation |
| DHCP | Dynamic Host Configuration Protocol | Auto IP assignment |
| DNS | Domain Name System | Name to IP translation |
| SMB | Server Message Block | Windows file sharing protocol |
| UNC | Universal Naming Convention | \\server\share format |
| SQL | Structured Query Language | Database query language |
| SSH | Secure Shell | Remote Linux access |
| NAT | Network Address Translation | Internet sharing for VMs |
| NTFS | New Technology File System | Windows file system |
| RBAC | Role Based Access Control | Access controlled by group membership |
| ACL | Access Control List | List of permissions on a file/folder |

---

## Phases Detail

### Phase 0: Environment Setup ✅
- VirtualBox installed and configured
- 5 NAT Networks created (Management, Manufacturing, Sales, Corporate, IT-Infrastructure)
- Project folder structure created at C:\LabProject\
- ISOs downloaded

### Phase 1: Domain Controller (DC01) ✅
- Windows Server 2022 installed
- Static IP: 10.0.1.10
- Active Directory Domain Services installed
- Domain: adventureworks.local promoted
- DNS Server running
- DHCP Server running
- DC01 has adapters on Management, Sales, and Corporate networks

### Phase 2: File Server (FS01) ✅
- Windows Server 2022 installed
- Joined to adventureworks.local domain
- SQL Server 2022 Developer Edition installed
- AdventureWorks2025 database restored
- FS01 has adapters on Management, Sales, and Corporate networks

### Phase 3: Explore AdventureWorks Data ✅
- Connected to SQL Server via SSMS
- Explored HumanResources, Person, Sales schemas
- Identified 290 employees across 16 departments
- Built SQL export query
- Exported employee data to CSV

### Phase 4: Active Directory Structure ✅
- 4 division OUs created (Manufacturing, Sales, Corporate, IT-Infrastructure)
- Each division has Users, Groups, Computers sub-OUs
- 16 department sub-OUs created
- 290 employees imported via PowerShell script
- 8 security groups created and populated

### Phase 5: File Shares & Permissions ✅
- C:\Shares folder structure created on FS01
- 5 SMB shares created
- NTFS permissions configured per division/department
- Real AdventureWorks data exported into shares
- BUILTIN\Users removed from all shares (security fix)
- Corporate subfolder inheritance corrected

### Phase 6: Linux Server (LINUX01) ⚠️
- Ubuntu Server 24.04 installed
- Static IP: 10.0.1.20
- Apache web server installed and running
- AdventureWorks intranet page created
- Samba file sharing configured with domain user authentication
- **Note:** Not domain-joined. Future enhancement to integrate with AD using realmd.

### Phase 7: Client Workstations ✅
- CLIENT-SALES built on Sales network (10.0.20.10)
- CLIENT-FIN built on Corporate network (10.0.30.10)
- Both joined to adventureworks.local domain
- Sales user (wbenshoof) verified on CLIENT-SALES
- Finance user (dbarber) verified on CLIENT-FIN
- Computer accounts moved to correct OUs
- File share access verified and tested

### Phase 8: DHCP & DNS Configuration ✅
- 5 DHCP scopes created (one per VLAN)
- DHCP options set (DNS server, gateway per scope)
- DNS A records added for FS01 and LINUX01
- CNAME aliases created (fileserver, intranet)
- DNS listening configured on all DC01 interfaces

### Phase 9: Firewall Rules ✅
- SMB (port 445) allowed from all 5 VLANs on FS01
- Rules named per VLAN for documentation clarity

### Phase 10: Group Policy ✅
- Baseline Security Policy linked to domain root
- Sales - USB Restrictions linked to Sales OU
- Finance - Screen Lock linked to Corporate OU
- Computer accounts verified in correct OUs
- GPO application verified via gpresult

### Phase 11: Testing & Validation ✅
- User authentication tested
- File share access tested for Sales and Finance users
- Cross-division access confirmed blocked
- GPO application confirmed via HTML report
- DNS resolution verified
- DHCP scopes verified
- Test results documented in C:\LabProject\Documentation\Test_Results.md

### Phase 12: Documentation 🔄
- Syllabus updated (this file)
- Test Results document created
- Network topology diagram: ⏳ Remaining
- AD structure diagram: ⏳ Remaining
- Permissions matrix: ⏳ Remaining
- IT Runbook: ⏳ Remaining

### Phase 13: Automation & Scripting ⏳
- AD health check script
- Disk space monitoring script
- User provisioning script
- Scheduled tasks

### Phase 14: Portfolio & GitHub ⏳
- GitHub repository creation
- All scripts uploaded
- All documentation uploaded
- README written
- Repository made public

---

## Known Issues & Resolutions

| Issue | Root Cause | Resolution |
|-------|-----------|------------|
| Inter-VLAN routing not working | VLANs isolated, no routing planned | Added Sales and Corporate adapters to DC01 and FS01 |
| BUILTIN\Users had read access to all shares | Default Windows permissions | Removed BUILTIN\Users from all share NTFS permissions |
| Corporate subfolder inheritance | Parent OU permissions inherited by sub-folders | Removed Corporate-Users from parent Corporate folder |
| Computer accounts in default Computers container | Domain join defaults to CN=Computers | Moved CLIENT-SALES to Sales OU, CLIENT-FIN to Corporate OU |
| DNS not listening on new adapters | DNS bound to original interface only | Used dnscmd /ResetListenAddresses to add all interfaces |
| td'hers username apostrophe | Special character in username breaks AD filters | Added manually using Distinguished Name |

---

## Interview Talking Points

**When asked "Walk me through your lab project":**
"I built a complete enterprise IT infrastructure lab using VirtualBox to simulate a bicycle manufacturing company called AdventureWorks. I deployed a Domain Controller running Active Directory with 290 real employees organized across 4 divisions and 16 departments. I configured a file server with department-specific shares, controlling access through security groups and NTFS permissions. I set up two client workstations on different VLANs to simulate real end-user environments, applied Group Policy for security enforcement, and documented the entire environment."

**When asked about Active Directory:**
"Active Directory manages user accounts, organizes them into Organizational Units by department, and uses security groups to control what resources each user can access. In my lab I imported 290 employees from a real database, organized them into 4 division OUs with 16 department sub-OUs, and created security groups that control file share access."

**When asked about permissions:**
"I used two layers of permissions on my file shares. Share Permissions control network-level access and NTFS permissions control file system-level access. The most restrictive permission wins. I set Share Permissions to allow Everyone, then restricted access at the NTFS level using security groups - so only Manufacturing employees can access Manufacturing files, only Finance can access Finance data, and so on."

**When asked about troubleshooting:**
"I ran into a permissions inheritance issue where Finance users could access HR data because the parent Corporate folder was granting access to all Corporate-Users. I caught it during testing, traced it through icacls, and fixed it by removing the Corporate-Users group from the parent folder while keeping the correct permissions on the sub-folders."

---

## Success Criteria

### Technical Achievements
- [x] All 5 VMs running and networked
- [x] 290 users authenticate to the domain
- [x] File shares work with proper permissions
- [x] DHCP scopes configured for all VLANs
- [x] DNS resolves all server names
- [x] Group Policies apply correctly
- [ ] Linux server fully domain-integrated
- [ ] Automation scripts running

### Learning Achievements
- [x] Can explain what Active Directory does
- [x] Can describe network topology
- [x] Understand NTFS vs Share permissions
- [x] Can troubleshoot common AD/network issues
- [x] Understand Group Policy inheritance
- [ ] Can create new users without instructions
- [ ] Can write PowerShell scripts independently

### Professional Achievements
- [ ] Complete documentation package
- [ ] Network diagrams professional quality
- [ ] GitHub repository public and organized
- [x] Can discuss project confidently in interviews
- [ ] Working code samples to show employers

---

## Remaining Work

1. Complete network topology diagram (Draw.io)
2. Create AD structure diagram
3. Write permissions matrix document
4. Write IT runbook
5. Build automation scripts (Phase 13)
6. Create GitHub repository and upload everything (Phase 14)

---

## Version History
- v1.0 - Initial syllabus created
- v2.0 - Updated February 2026 - Reflects actual build progress, known issues, and resolutions