# Phase 01 — Networking Infrastructure

**Status:** Completed initial deployment and validation | **Environment:** Isolated home lab

## Overview

I built a dedicated wired network to support my systems administration home lab. My main design requirement was to give lab devices internet access without allowing experiments with servers, DHCP, DNS, or firewalls to disrupt my household network.

The first phase covered physical connectivity, private IPv4 addressing, router and firewall configuration, and connectivity testing. The network is operational; Proxmox and Windows Server deployment are next.

## Network topology

![Home Lab Network Topology](diagrams/Household%20and%20Lab%20Network%20Diagram.png)


The diagram shows the household side (`192.168.1.0/24`) connected through an unmanaged switch to the lab router's WAN interface. The lab router's LAN interface (`10.10.10.1/24`) connects to the managed lab switch. HV-01, HV-02, and WS-01 are connected; WS-02 is not yet connected.

The unmanaged switch remains on the household side so existing devices keep their normal connectivity independent of the lab. The dedicated router creates a separate lab subnet; a firewall access-control rule restricts lab-initiated traffic to the household subnet. Merely using a second router or a different subnet would not, by itself, guarantee isolation.

## Environment

| Asset | Role | Current state |
|---|---|---|
| Lab router/firewall | Routing, NAT, access control, current DHCP | Configured |
| 8-port managed switch | Wired lab connectivity, future VLAN experiments | Connected |
| Upstream unmanaged switch | Shares household uplink | Connected |
| HV-01 | Primary virtualization host | Physically connected; Proxmox pending |
| HV-02 | Secondary virtualization host | Physically connected; Proxmox pending |
| WS-01 | Windows administration/test workstation | Connected |
| WS-02 | Additional Windows test workstation | Not yet connected |

Physical systems are intentionally documented with generic asset IDs instead of identifying model numbers.

## Addressing at a glance

| Item | Value |
|---|---|
| Lab subnet | `10.10.10.0/24` |
| Lab router / default gateway | `10.10.10.1` |
| Protected household subnet | `192.168.1.0/24` |
| Current DHCP provider | Lab router |
| Proposed hypervisor addresses | `10.10.10.10`, `10.10.10.11` |
| Proposed first domain controller | `10.10.10.20` |

**Note:** Planned static server/host addresses are not presented as deployed. See [IP Addressing](ip-addressing.md).

## Isolation policy

I configured an access-control policy named `BLOCK_LAB_TO_HOME`, using source group `LAB_NETWORK` (`10.10.10.0/24`) and destination group `HOME_NETWORK` (`192.168.1.0/24`). The policy blocks LAN-to-WAN traffic matching those groups while other permitted outbound connections, including internet access, remain available.

See [Firewall Configuration](firewall-configuration.md) for the saved rule and reasoning.

## Testing and evidence

During setup, I used `ping`, `Test-NetConnection` with TCP port 80, and `nslookup`. I also checked connectivity between household devices as a baseline before trying to reach a household device from the lab.

**Observed outcomes:**
- Lab internet connectivity worked.
- DNS resolution from the lab workstation succeeded.
- Home-to-home connectivity succeeded in the baseline check.
- Lab-to-home connectivity attempts did not succeed, consistent with the intended block.
- The router's access-control interface showed the configured deny rule.

These observations support the isolation objective but do not include archived raw command output or firewall drop logs. A failed TCP port test alone is not proof of firewall enforcement unless that port is confirmed reachable from an allowed location.

See [Network Verification](network-verification.md) for methods, results, and limitations.

## Key lessons

- **Switching and routing are different:** a managed switch connects local devices; the router handles traffic between IP networks.
- **Subnetting is not a firewall:** different address ranges do not automatically block communication.
- **DHCP and the default gateway are separate roles:** the router currently provides both, but Windows Server can take over DHCP in a later exercise without becoming the gateway.
- **Connectivity is not the same as service availability:** ICMP success cannot prove that DNS or an HTTP service is healthy.
- **Validate restrictions with a baseline:** confirm a destination works from an allowed location before interpreting failed tests from a restricted one.

## Project documentation

| Document | Contents |
|---|---|
| [IP Addressing](ip-addressing.md) | Subnet, gateway, DHCP, planned static addresses, subnetting practice |
| [Firewall Configuration](firewall-configuration.md) | Access-control rule, address groups, routing vs. filtering |
| [Network Verification](network-verification.md) | Test methods, observations, limitations, future evidence |

## Planned additions

- Upload the finished network topology image to `diagrams/Household%20and%20Lab%20Network%20Diagram.png`.
- Sanitized screenshots of the router access-control policy.
- Redacted command outputs from repeatable verification tests, clearly labeled with the date captured.
- Proxmox host configurations and later VLAN/DNS/DHCP exercises.

> **Privacy:** This is a personal learning environment, not a production deployment. Exact physical model names, serial numbers, MAC addresses, credentials, and sensitive management details are intentionally omitted.
