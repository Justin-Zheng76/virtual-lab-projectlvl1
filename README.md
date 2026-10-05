# virtual-lab-projectlvl1
self built home lab that covers virtualization, SSH, networking, enterprise services, and security fundamentals documented by levels of sophistication
# Home Lab: Virtualization, Networking & Security

An ongoing, self-built home lab project. Built bottom-up across four levels, 
starting with basic virtualization and working toward traffic simulation 
and incident response.

## Why This Project
I'm currently pursuing an accelerated B.S./M.S. in Information Technology 
through WGU, alongside certifications in AWS Cloud Practitioner, CompTIA 
A+, CompTIA Network+, and LPI Linux Essentials. This lab is where I apply 
that knowledge hands-on, building real infrastructure instead of just 
studying for exams.

## Structure

| Level | Focus | Status |
|---|---|---|
| [Level 1](./level-1-virtualization) | Virtualization & networking fundamentals | ✅ Complete |
| [Level 2](./level-2-services) | Core enterprise services (AD, DNS, DHCP, file server) | 🔲 Not started |
| [Level 3](./level-3-segmentation) | Segmentation, proxies & containers | 🔲 Not started |
| [Security Layer](./security-layer) | Traffic simulation & incident response | 🔲 Not started |

Each level sits on top of the one before it. The security layer runs on 
top of all three, since detecting and responding to attacks assumes the 
whole environment already exists.

## Tools Used
VirtualBox, Ubuntu Server, Kali Linux, Windows Server (evaluation), 
pfSense/OPNsense, Docker, Wazuh, DVWA, and more as the project grows, 
see each level's README for specifics.

## What's in Each Level Folder
- A README documenting what was built, steps taken, and what was learned
- Screenshots of key milestones
- Any troubleshooting notes worth keeping for reference

---
*This project is actively being worked on. Check individual level folders 
for the most current progress.*
