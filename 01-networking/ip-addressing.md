# IP Addressing and Subnetting

**Phase:** 01 — Networking | **Status:** Lab subnet configured; server addresses planned

## Objective

I configured a predictable private IPv4 network for the lab and used it as a practical way to study subnet masks, CIDR notation, gateways, DHCP, and routing between subnets.

## Deployed network

| Property | Value |
|---|---|
| Network | `10.10.10.0/24` |
| Subnet mask | `255.255.255.0` |
| Network address | `10.10.10.0` |
| Broadcast address | `10.10.10.255` |
| Conventional usable host range | `10.10.10.1`–`10.10.10.254` |
| Total IPv4 addresses | 256 |
| Conventional usable host addresses | 254 |
| Router / gateway | `10.10.10.1` |
| DHCP provider | Dedicated lab router |

The `/24` prefix allocates 24 bits to the network portion of a 32-bit IPv4 address. That leaves 8 host bits, so `2^8 = 256` total addresses; the network and broadcast addresses are not assigned as ordinary hosts.

## Address plan

| System | Address | State |
|---|---|---|
| Lab gateway | `10.10.10.1` | Configured |
| HV-01 | `10.10.10.10` | Planned static address |
| HV-02 | `10.10.10.11` | Planned static address |
| DC01 | `10.10.10.20` | Planned static address |
| FILE01 | `10.10.10.21` | Planned static address |
| DC02 | `10.10.10.22` | Planned static address |
| WS-01 | DHCP assignment | Connected; exact lease not recorded here |
| WS-02 | DHCP assignment | Not yet connected |

**Important:** An address in the subnet's usable range is not necessarily in the router's current DHCP pool. Before assigning static addresses to hypervisors or servers, I will confirm that those addresses are outside the dynamic lease pool or otherwise protected by appropriate exclusions/reservations.

## Static and dynamic configuration

Infrastructure components and servers benefit from predictable addresses because management tools, services, and other devices need stable destinations. Workstations can generally obtain addresses dynamically.

The lab router currently supplies DHCP information such as IP address, mask, default gateway, and DNS server. In a later exercise, I plan to configure Windows Server DHCP. I will coordinate the cutover so the router and Windows Server are not independently offering conflicting settings on the same subnet.

Even if Windows Server becomes the DHCP provider, the lab router remains the default gateway at `10.10.10.1` unless I explicitly redesign routing.

## Subnetting exercises

I practiced subdividing the original `10.10.10.0/24` into equally sized subnets:

| Prefix | Number of equal subnets from /24 | Total addresses each | Conventional usable hosts each |
|---|---:|---:|---:|
| `/24` | 1 | 256 | 254 |
| `/25` | 2 | 128 | 126 |
| `/26` | 4 | 64 | 62 |
| `/27` | 8 | 32 | 30 |

Formula: **total addresses per subnet = `2^(32 - prefix)`**. For conventional IPv4 LAN subnets, subtract 2 for the network and broadcast addresses.

### Splitting a /24 into two /25 networks

| Network | Usable range | Broadcast |
|---|---|---|
| `10.10.10.0/25` | `10.10.10.1`–`10.10.10.126` | `10.10.10.127` |
| `10.10.10.128/25` | `10.10.10.129`–`10.10.10.254` | `10.10.10.255` |

The `.128` address begins the second subnet, rather than ending the first.

### Splitting a /24 into four /26 networks

| Network | Usable range | Broadcast |
|---|---|---|
| `10.10.10.0/26` | `.1`–`.62` | `.63` |
| `10.10.10.64/26` | `.65`–`.126` | `.127` |
| `10.10.10.128/26` | `.129`–`.190` | `.191` |
| `10.10.10.192/26` | `.193`–`.254` | `.255` |

I initially estimated 63 usable addresses per /26, then corrected it to **62** after accounting for the network and broadcast addresses.

### Identifying when routing is required

- `10.10.10.150/26` belongs to `10.10.10.128/26`; `10.10.10.200/26` belongs to `10.10.10.192/26`. Communication between these IP subnets requires routing.
- `10.10.10.50/25` and `10.10.10.100/25` both belong to `10.10.10.0/25`; if they share an appropriate local Layer 2 network and policy permits it, they can communicate without a router.

These splits were **learning exercises**, not changes applied to the production lab configuration. The lab remains `10.10.10.0/24`.

## Concepts reinforced

- A subnet mask distinguishes the network and host portions of an address.
- Increasing a CIDR prefix by one divides an equal-sized address block into two smaller blocks.
- Devices use their mask and route table to decide whether to send packets locally or through a gateway.
- Devices on a local IPv4 Ethernet network typically use ARP to learn the destination MAC address.
- DHCP assignment and routing are separate functions.

## Planned follow-up

After Proxmox deployment, I will record actual management addresses and DHCP pool limits. Future networking exercises will include VLANs, additional subnets, and controlled inter-VLAN routing.
