# sysadmin-homelab

# Systems Administration Home Lab

## Project Overview

I'm building a dedicated home lab to strengthen my hands-on experience with IT infrastructure and further develop the skills needed to transition into a Systems Administrator role.

My background includes professional IT support experience and certifications such as CompTIA Cloud+ and CySA+. Through this project, I want to expand my practical knowledge of Windows Server administration, networking, virtualization, security, and PowerShell automation.

Rather than simply following tutorials, my goal is to understand how these technologies work together, troubleshoot issues independently, and document the decisions I make throughout the process.

This repository serves as a technical portfolio documenting my configurations, testing procedures, troubleshooting experiences, and lessons learned.

## Lab Environment

The lab consists of two dedicated virtualization hosts, two Windows workstations, and a separate network environment.

To maintain privacy, physical systems are identified using standardized asset IDs rather than manufacturer model names.

### Hardware Overview

| Device | Specifications | Purpose |
|---|---|---|
| HV-01 | 14-core Intel CPU, 32 GB RAM, 512 GB SSD | Primary virtualization host |
| HV-02 | 14-core Intel CPU, 16 GB RAM, 512 GB SSD | Secondary virtualization host |
| WS-01 | Intel processor, 32 GB RAM | Administration workstation |
| WS-02 | Intel processor, 16 GB RAM | Secondary domain workstation |
| Lab Router/Firewall | Dedicated Gigabit router | Network isolation, DHCP, and routing |
| Managed Switch | 8-port Gigabit switch | Lab network connectivity and future VLAN configuration |

### Network Overview

The lab uses a dedicated private subnet separated from my household network.

| Setting | Configuration |
|---|---|
| Lab Subnet | 10.10.10.0/24 |
| Default Gateway | 10.10.10.1 |
| Address Assignment | Router-based DHCP |
| Network Isolation | Firewall access-control policy |
| Internet Access | Enabled |

A dedicated router/firewall separates the lab from my existing household network.

This configuration allows the lab to maintain internet connectivity while restricting lab-initiated access to household devices.

Future networking exercises will include VLANs, routing, and additional network segmentation.

## Project Progress

| Phase | Description | Status |
|---|---|---|
| 01 — Networking | Physical network, IP addressing, firewall configuration, and verification | Completed |
| 02 — Virtualization | Install and configure Proxmox VE on two physical hosts | Planned |
| 03 — Windows Server | Deploy Windows Server and configure Active Directory/DNS | Planned |
| 04 — Windows Administration | Group Policy, file sharing, permissions, and DHCP | Planned |
| 05 — PowerShell | Administrative scripting and automation | Planned |
| 06 — Advanced Networking & Security | VLANs, firewall policies, monitoring, and troubleshooting | Planned |
| 07 — Linux | Linux server deployment and administration | Planned |
| 08 — Backup & Recovery | Backups, restoration testing, and recovery procedures | Planned |

Progress will be updated as each phase is implemented and verified.

## Completed Project: Network Infrastructure

The first phase of my home lab focused on creating a functional and isolated network environment.

### Objectives

- Provide internet access to lab devices.
- Separate lab traffic from household devices.
- Establish a consistent IP addressing scheme.
- Configure network access restrictions.
- Verify permitted and blocked connectivity.
- Create a foundation for future server deployments.

### Implementation

I configured a dedicated router and managed switch to support the lab environment.

The router provides the `10.10.10.0/24` network and acts as the default gateway at `10.10.10.1`.

I also implemented a firewall access-control rule to restrict communication from the lab network to the household network.

### Verification

After configuration, I performed connectivity and isolation tests using Windows networking utilities, including:

- `ping` — ICMP connectivity testing.
- `Test-NetConnection` — TCP connectivity testing using port 80.
- `nslookup` — DNS resolution testing.

I also verified connectivity between household devices to establish a working baseline before testing restrictions from the lab.

The tests confirmed that the lab retained internet connectivity while connections to protected household devices were blocked as intended.

### Key Lessons

This phase helped strengthen my understanding of subnetting, DHCP, DNS, routing, switching, and firewall policies.

One important lesson was that network separation alone does not necessarily guarantee security. Firewall policies must be configured correctly and their intended behavior verified through testing.

I also gained experience distinguishing basic host connectivity from application and service availability.

**Detailed documentation:** [Networking Projects](01-networking/README.md)

## Planned Technologies

As the project develops, I plan to work with:

- Proxmox Virtual Environment
- Windows Server 2025 and Windows Server 2022
- Active Directory Domain Services
- DNS and DHCP
- Group Policy
- PowerShell
- Ubuntu Server
- Virtual networking and VLANs
- Backup and recovery tools

These technologies will be introduced progressively rather than deployed all at once.

## Documentation Approach

For each major exercise, I aim to document:

1. The objective and expected outcome.
2. The infrastructure and technologies involved.
3. The implementation and configuration decisions.
4. Testing and verification.
5. Troubleshooting steps and challenges.
6. Lessons learned and potential improvements.

My focus is on understanding the reasoning behind each configuration and developing practical troubleshooting skills.

## Future Development

The next phase of this project involves installing Proxmox VE on both virtualization hosts and deploying my first Windows Server virtual machine.

From there, I plan to build an Active Directory environment, join physical and virtual workstations to the domain, implement administrative policies, and develop PowerShell scripts to automate routine tasks.

This repository will continue to evolve as I expand the environment and gain experience with additional technologies.
