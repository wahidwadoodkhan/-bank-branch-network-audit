
# Bank Branch Network Audit

## 1. Project Overview

This project demonstrates the setup of a basic computer network for a small bank branch using Cisco Packet Tracer.

The network consists of:
- Two computers (PC0 and PC1)
- One Cisco 2960-24TT switch
- Ethernet connections between both PCs and the switch

The purpose is to establish communication between the two computers using IP addressing and verify connectivity using the ping command.

## 2. Network Topology

The following diagram shows the network connection:

```text
       PC0                         PC1
  192.168.1.10                192.168.1.11
       |                           |
       |                           |
       +------------+--------------+
                    |
             Cisco 2960 Switch
                    |
              Local Network
```

Both PCs are connected to the same switch using Ethernet cables.

## 3. IP Addressing Table

| Device | Interface | IP Address | Subnet Mask |
|--------|-----------|------------|-------------|
| PC0 | FastEthernet0 | 192.168.1.10 | 255.255.255.0 |
| PC1 | FastEthernet0 | 192.168.1.11 | 255.255.255.0 |

Network Address: 192.168.1.0/24

## 4. Role of the Switch

The Cisco 2960 switch connects both computers within the same local area network (LAN).

It forwards Ethernet frames to the correct device using MAC addresses. This allows PC0 and PC1 to communicate with each other through the switch.

## 5. Why Do Both PCs Need the Same Subnet Mask?

Both computers use the subnet mask 255.255.255.0 because they belong to the same /24 network.

This subnet mask identifies which part of an IP address represents the network and which part represents the host.

Both computers can communicate directly within the same local network without requiring a router.

## 6. Connectivity Testing

Connectivity was tested using the ping command in Cisco Packet Tracer.

- PC0 pinged PC1 (192.168.1.11).
- PC1 pinged PC0 (192.168.1.10).

Both computers successfully exchanged ping packets.

The results showed 0% packet loss.

Detailed command outputs are provided in:

- `outputs/ipconfig.txt`
- `outputs/ping.txt`
- 
## 7. Problem Encountered and Troubleshooting

## Problem:
During testing, a ping from PC0 to PC1 (192.168.1.11) failed with the message
"Request timed out" and 100% packet loss. This meant PC0 could not reach PC1
through the switch.

## Troubleshooting steps:
1. Checked the topology and found that the link between PC1 and the switch was
   not active (no green link light).
2. Checked the IP configuration on both PCs with `ipconfig` and confirmed that
   both used the same subnet (192.168.1.0/24, mask 255.255.255.0), so the
   addressing was not the cause.
3. Found that the PC1 port was turned off, so no traffic could pass through the
   switch.

## Solution:
Turned the PC1 FastEthernet0 port back on (and confirmed the cable was
connected to the switch). After the link light turned green, the ping was
repeated.

## Result:
PC0 successfully pinged PC1 with 4 packets sent, 4 received, and 0% loss.
The same test from PC1 to PC0 also succeeded.

## Lesson learned:
"Request timed out" usually points to a physical or connectivity problem (port
down, cable, wrong IP or subnet). Checking the link lights first, then the IP
settings, is a quick way to find the cause. The failed ping is shown in
`outputs/ping_timeout.png` and the successful ping in `outputs/ping.txt`


## 8. Project Tools

- Cisco Packet Tracer
- Cisco 2960-24TT Switch
- Two PCs
- Ethernet cables
- GitHub

## 9. Project Outcome

Successfully created a basic LAN connecting two computers through a Cisco switch.

Both computers were configured with IP addresses in the same subnet, and successful ping tests confirmed network connectivity.
# -bank-branch-network-audit
Audit and redesign of a flat office network for a small bank branch.
