# Bank Branch Network Audit

Audit and redesign of a flat office network for a small bank branch.

**Intern:** Wahid Wadood Khan
**Tool:** Cisco Packet Tracer
**Current stage:** Task 2 of 5: as-is write-up of the flat network

---

## 1. Project Overview

A small bank branch has a single flat office network with no documentation. This project audits that network and later redesigns it in five tasks:

| Task | Description | Status |
|------|-------------|--------|
| 1 | First network: two PCs and a switch | Done |
| 2 | As-is write-up: one flat /24, one switch, drawn and described | **This README** |
| 3 | Risks of the flat design and what each costs the business | Planned |
| 4 | New design: VLANs and VLSM addressing plan | Planned |
| 5 | Handover: before and after, test table, final README | Planned |

This stage records the network exactly as it is. Nothing has been hardened or changed to improve it yet.

## 2. Network Summary

- One Cisco 2960-24TT switch (`BRANCH-SW1`)
- Seven end devices: three tellers, one manager, one banking server, one printer, one visitor laptop
- One flat network: `192.168.1.0/24`, with no VLANs and no router
- All devices connected to the switch by copper straight-through Ethernet cables
- All IP addresses set manually (static)

## 3. As-Is Topology

```text
  Teller1   Teller2   Teller3   Manager   Banking    Printer    Visitor
   PC0       PC1       PC2       PC3      Server0    Printer0   Laptop0
   .10       .11       .12       .13       .20        .30        .40
    |         |         |         |         |          |          |
  Fa0/1     Fa0/2     Fa0/3     Fa0/4     Fa0/5      Fa0/6      Fa0/7
    +---------+---------+----+----+---------+----------+----------+
                              |
                  BRANCH-SW1 (Cisco 2960-24TT)
                    VLAN 1, 192.168.1.2/24
```

Screenshot from Packet Tracer:

![Network topology](diagrams/topology.png)

## 4. Addressing Table

Network: `192.168.1.0/24` (254 usable host addresses)

| Device | Role | Interface | Switch Port | IP Address | Subnet Mask |
|--------|------|-----------|-------------|------------|-------------|
| BRANCH-SW1 | Switch (management) | VLAN 1 | n/a | 192.168.1.2 | 255.255.255.0 |
| PC0 | Teller 1 | FastEthernet0 | Fa0/1 | 192.168.1.10 | 255.255.255.0 |
| PC1 | Teller 2 | FastEthernet0 | Fa0/2 | 192.168.1.11 | 255.255.255.0 |
| PC2 | Teller 3 | FastEthernet0 | Fa0/3 | 192.168.1.12 | 255.255.255.0 |
| PC3 | Branch manager | FastEthernet0 | Fa0/4 | 192.168.1.13 | 255.255.255.0 |
| Server0 | Core banking server | FastEthernet0 | Fa0/5 | 192.168.1.20 | 255.255.255.0 |
| Printer0 | Shared printer | FastEthernet0 | Fa0/6 | 192.168.1.30 | 255.255.255.0 |
| Laptop0 | Visitor laptop | FastEthernet0 | Fa0/7 | 192.168.1.40 | 255.255.255.0 |

No default gateway is configured because the network has no router.

## 5. Findings: What Was Found

Because there was no documentation, the network was discovered and recorded using `show` commands and `ipconfig /all`.

1. **One network for everything.** All devices share `192.168.1.0/24` and one broadcast domain.
2. **No segmentation.** Every port is in the default VLAN 1. Tellers, manager, server, printer and a visitor laptop are all on the same network.
3. **One switch.** All devices depend on a single switch, with no redundancy.
4. **Basic switch configuration only.** The switch has a hostname and a management IP on VLAN 1. It has no passwords, banner or port security.
5. **Static addressing.** IP addresses are assigned by hand, so there is no DHCP and no central record of who has which address.
6. **No documentation.** No diagram or address list existed before this audit. This README is the first record.

The business risks of these findings are assessed in Task 3.

## 6. Switch Configuration (As Found)

```text
hostname BRANCH-SW1
!
interface Vlan1
 ip address 192.168.1.2 255.255.255.0
 no shutdown
```

The full output is in `configs/BRANCH-SW1-running-config.txt`.

## 7. Verification

PC0 was used to test connectivity to every other device. Results:

| Test | Destination | Result |
|------|-------------|--------|
| Ping | PC1 (192.168.1.11) | 4 sent, 4 received, 0% loss |
| Ping | PC2 (192.168.1.12) | 4 sent, 4 received, 0% loss |
| Ping | PC3 (192.168.1.13) | 4 sent, 4 received, 0% loss |
| Ping | Server0 (192.168.1.20) | 4 sent, 4 received, 0% loss |
| Ping | Printer0 (192.168.1.30) | 4 sent, 4 received, 0% loss |
| Ping | Laptop0 (192.168.1.40) | 4 sent, 4 received, 0% loss |
| Ping | BRANCH-SW1 (192.168.1.2) | 4 sent, 4 received, 0% loss |
| Traceroute | Server0 (192.168.1.20) | 1 hop (same network) |

The single-hop traceroute confirms all devices are on one flat network with no router in between. Edit the table if any of your results differ.

## 8. Evidence Files

| Evidence | File |
|----------|------|
| Switch running-config | `configs/BRANCH-SW1-running-config.txt` |
| PC0 IP settings | `configs/PC0-ipconfig.txt` |
| PC1 IP settings | `configs/PC1-ipconfig.txt` |
| PC2 IP settings | `configs/PC2-ipconfig.txt` |
| PC3 IP settings | `configs/PC3-ipconfig.txt` |
| Server0 IP settings | `configs/Server0-ipconfig.txt` |
| Laptop0 IP settings | `configs/Laptop0-ipconfig.txt` |
| Interface summary | `outputs/show-ip-interface-brief.txt` |
| VLAN table | `outputs/show-vlan-brief.txt` |
| MAC address table | `outputs/show-mac-address-table.txt` |
| Port status | `outputs/show-interfaces-status.txt` |
| Ping tests | `outputs/ping-tests.txt` |
| Traceroute | `outputs/traceroute.txt` |
| Topology image | `diagrams/topology.png` |

## 9. Repository Structure

```text
bank-branch-network-audit/
├── README.md
├── configs/
├── outputs/
└── diagrams/
```

## 10. How to Reproduce

1. Open Cisco Packet Tracer and add one 2960-24TT switch and seven end devices.
2. Connect each device to the switch with a copper straight-through cable (ports as in the addressing table).
3. Set the static IP addresses and the `255.255.255.0` mask on each device. Leave the gateway empty.
4. On the switch, set the hostname and the VLAN 1 management address.
5. Run the `show` commands and ping tests, then save the output as text files.

## 11. Problem Encountered and Troubleshooting

Describe one real issue you faced here, or delete this section if there was none. Include what you saw (for example "Request timed out"), how you found the cause, and how you fixed it.

## 12. Next Steps

- **Task 3:** list the risks of this flat design and what each costs the business.
- **Task 4:** redesign with VLANs and a VLSM addressing plan.
- **Task 5:** before and after comparison, test table and final handover.
