# Lab 2 – Extended ACL Security

## 1. Objective

Configure an extended Access Control List (ACL) on a Cisco router to control traffic between Users, Management and Servers networks.

**Technologies:** Cisco Packet Tracer, Cisco IOS, IPv4, Extended ACLs.

## 2. Topology

/lab2_Extended_ACLs.png

## 3. IP Addressing Scheme

| Device    | Interface | IP Address       | Default Gateway |
|-----------|-----------|------------------|-----------------|
| R1        | G0/0      | 192.168.10.1/24  |        —        |
| R1        | G0/1      | 192.168.20.1/24  |        —        |
| R1        | G0/2      | 192.168.30.1/24  |        —        |
| PC1       | NIC       | 192.168.10.10/24 | 192.168.10.1    |
| PC2       | NIC       | 192.168.10.11/24 | 192.168.10.1    |
| PC3       | NIC       | 192.168.20.10/24 | 192.168.20.1    |
| PC4       | NIC       | 192.168.20.11/24 | 192.168.20.1    |
| Server1   | NIC       | 192.168.30.10/24 | 192.168.30.1    |
| Server2   | NIC       | 192.168.30.11/24 | 192.168.30.1    |

## 4. Implementation 

- Configured IPv4 addresses on all end devices and R1 interfaces.
- Verified connectivity between each end device and its default gateway.
- Created two named extended ACLs: USERS_SECURITY AND MANAGEMENT_SECURITY.
- Applied ACLs inbound on R1 g0/0 and g0/1 respectively.


## 5. ACL Security Requirements


| Source Network | Destination Network | Protocol / Service | Action     |
|----------------|---------------------|--------------------|------------|
| Users          | Servers             | HTTP (TCP port 80) | Permit ✅  |
| Users          | Servers             | All other traffic  | Deny   ☠️  |
| Users          | Management          | All traffic        | Deny   ☠️  |
| Management     | Servers             | All traffic        | Permit ✅  |
| Management     | Users               | All traffic        | Permit ✅  |

### Security Objectives

- Allow Users to access HTTP services on both servers.

- Prevent Users from initiating connections to Management.

- Block Users from accessing server services other than HTTP.

- Allow Management to communicate with Users and Servers.

- Account for the implicit deny at the end of each ACL.

### 6. Verification and Testing

- Verified HTTP access from both User PCs to both servers.

- Confirmed that non-HTTP traffic from Users to Servers was denied.

- Confirmed that Users could not initiate communication with Management.

- Verified that Management could communicate with Users and Servers.

- Used show ip access-lists to inspect ACL entries and match counters.

### 7. Troubleshooting

Issue: Management PCs could initiate ping requests to Users PCs, but received no replies.

Cause: The inbound Users ACL blocked ICMP Echo Replies returning from the Users network.

Resolution: Added an ICMP Echo Reply permit statement before the relevant deny entry in USERS_SECURITY.

permit icmp 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255 echo-reply

### 8. Key Learning Outcomes

- Configuring and applying named extended ACLs.

- Matching source and destination networks using wildcard masks.

- Filtering traffic using TCP ports and ICMP message types.

- Understanding ACL direction, sequence numbers and implicit deny.

- Troubleshooting return traffic in stateless ACLs.

