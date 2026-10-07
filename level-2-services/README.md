# Level 2: Core Enterprise Services

## Goal
Build a small Active Directory domain on top of the Level 1 foundation: a domain
controller providing DNS and DHCP, a domain-joined Windows client, and a file share
delivered to users through Group Policy.

## What I Built
- **Hypervisor:** VirtualBox on a Windows host
- **Domain Controller:** Windows Server 2025 Standard Evaluation (Desktop Experience),
  hostname `DC01`, domain `lab.local`, 6 GB RAM, 40 GB dynamic disk
- **Client:** Windows 11 Enterprise Evaluation, hostname `Win11Client`, joined to `lab.local`
- **Services on DC01:** Active Directory Domain Services, DNS, DHCP, SMB file share
- **Networking:** DC01 has two adapters, NAT for internet access and activation, and an
  Internal Network named `labnet` (192.168.10.10/24) for lab traffic. The client sits on
  `labnet` only.
- **DHCP:** scope 192.168.10.100 to 192.168.10.200 (/24), with DNS pointing at DC01
- **Directory:** `LabUsers` OU, a test user, and a `FileShare-Users` security group
- **File services:** `C:\Shares\Company` shared as `\\dc01\Company`
- **Group Policy:** "Map Company Drive" linked to the `LabUsers` OU, mapping the share as `S:`

```
         Internet
            |
          [NAT]
            |
  +-----------------------+
  | DC01                  |
  | AD DS, DNS, DHCP, SMB |
  | 192.168.10.10         |
  +-----------+-----------+
              |
     Internal Network "labnet"
              |
  +-----------+-----------+
  | Win11Client           |
  | DHCP lease 192.168.10.x |
  +-----------------------+
```

## Steps Taken
1. Created the Server 2025 VM and installed it with Desktop Experience
2. Renamed the server to `DC01` and set DNS to point at itself before promotion
3. Installed the AD DS role and promoted the server to a new forest, `lab.local`
4. Added a second adapter on the `labnet` internal network and gave it a static IP
5. Installed the DHCP role, authorized it in AD, and created and activated a scope
6. Created the Windows 11 client VM on `labnet` and confirmed it received a DHCP lease
7. Created the `LabUsers` OU, a test user, and the `FileShare-Users` security group
8. Joined the client to the domain and signed in with the domain account
9. Created and shared `C:\Shares\Company`, with share permissions for Authenticated Users
   and NTFS Modify permission for `FileShare-Users` only
10. Created a GPO linked to `LabUsers` that maps `\\dc01\Company` as drive `S:`
11. Verified the drive appears at login, then took snapshots of both VMs

## Troubleshooting Notes
- **Black screen after the install's first restart:** the optical drive was ahead of the
  hard disk in the boot order. Moved Hard Disk first and adjusted display settings.
- **Client received a 169.254.x.x address:** this self-assigned address means the client
  sent DHCP requests and got no answer. Worked through the network name on both VMs,
  whether the scope was active, and the DC's adapter bindings. [Add the cause you found.]
- **Mapped drive did not appear:** `gpresult /r` showed no applied GPOs for the user,
  which pointed to a missing GPO link. Created the GPO directly on the OU with
  "Create a GPO in this domain, and Link it here", and the drive appeared at next login.

## What I Learned
- How AD, DNS, and DHCP depend on each other, especially why a DC's DNS must point at itself
- Why VirtualBox NAT can't serve DHCP between VMs, and how an Internal Network solves it
- Share permissions versus NTFS permissions: the stricter of the two wins
- Using security groups instead of individual users to control access
- How user Group Policy applies at logon, and how to check it with `gpresult /r`

## Milestone: ✅ Complete
A user signs in with AD credentials on a domain-joined client and sees a mapped network
drive, with DHCP, DNS, and permissions all working.

---
*Part of an ongoing home lab project. See the root README for all levels.*
