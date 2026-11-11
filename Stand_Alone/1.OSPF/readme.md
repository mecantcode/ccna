This is my very first lab. Subsequently a very poor try at documentation follows.

# Lab 01 — Single-Area OSPF Routing

## Overview
Configured a three-router network using OSPF Area 0 to enable dynamic routing and communication between two remote LANs.

**Tools:** Cisco Packet Tracer

## Topology
/

## IP Addressing

| Device | Interface | IP Address   | Subnet Mask |
|--------|-----------|--------------|-------------|
| R1     | G0/0      | 192.168.10.1 | /24         |
| R1     | S0/0/0    | 10.0.12.1    | /30         |
| R2     | S0/0/0    | 10.0.12.2    | /30         |
| R2     | S0/0/1    | 10.0.23.1    | /30         |
| R3     | S0/0/0    | 10.0.23.2    | /30         |
| R3     | G0/0      | 192.168.20.1 | /24         |

## Implementation
- Configured router interfaces and serial links.
- Configured OSPF Area 0 on all three routers.
- Advertised LAN and point-to-point networks.
- Established OSPF neighbour adjacencies.

## Verification
- Verified interface status using `show ip interface brief`.
- Verified OSPF neighbours using `show ip ospf neighbor`.
- Checked dynamically learned routes using `show ip route`.
- Tested end-to-end connectivity between PCs on both LANs.

## Troubleshooting
Initially the ping from PC1 to PC3 across the two LANs failed and R1 routing table showed no router to R3 LAN.
Investigation was done using the show commands.Later I found out that I had enabled ospf on a wrong interface at R3 using an incorrect IP address.Later I configure it and voila ping succeeded. 
 from
