# Lab 3 – NAT/PAT Internet Access

## 1. Objective
Configure Port Address Translation (PAT) on an edge router to allow multiple internal PCs to access an external server using a single public IP address.

## 2. Topology
Lab3_NAT_PAT_Topology.png


## 3. IP Addressing

| Device  | Interface | IP Address    | Subnet Mask     |
|---------|-----------|---------------|-----------------|
| PC1     | NIC       | 192.168.10.11 | 255.255.255.0   |
| PC2     | NIC       | 192.168.10.12 | 255.255.255.0   |
| PC3     | NIC       | 192.168.10.13 | 255.255.255.0   |
| R1      | G0/1      | 192.168.10.1  | 255.255.255.0   |
| R1      | G0/0      | 203.0.113.2   | 255.255.255.252 |
| ISP1    | G0/0      | 203.0.113.1   | 255.255.255.252 |
| ISP1    | G0/1      | 198.51.100.1  | 255.255.255.0   |
| Server1 | NIC       | 198.51.100.10 | 255.255.255.0   |

**Default Gateways:**
- Internal PCs: 192.168.10.1
- Server1: 198.51.100.1

## 4. Configuration

### R1 – Routing and PAT

1.Configure the default route towards ISP1:

```
ip route 0.0.0.0 0.0.0.0 203.0.113.1
```

2.Identify NAT inside and outside interfaces:

```
interface g0/1
 ip nat inside
exit

interface g0/0
 ip nat outside
exit
```

3.Create an named ACL to match the internal network:

```
access-list standard Intra_Users
 permit 192.168.10.0 0.0.0.255
```

4.Configure PAT using the outside interface address:

```
ip nat inside source list Intra_Users interface g0/0 overload
```

### ISP1 – Return Route

1.Configure a static route towards the internal network:

```
ip route 192.168.10.0 255.255.255.0 203.0.113.2
```

### Server1

Configured with a static IP address, subnet mask and default gateway as shown in the addressing table.

## 5. Verification

The following commands were used to verify the configuration:

```
show ip interface brief
show ip route
show ip access-lists
show ip nat translations
show ip nat statistics
```

Connectivity was tested by pinging the external server (198.51.100.10) from all three internal PCs.

**Results:**
- All three PCs successfully reached the external server.
- NAT translations showed all internal PCs using the same public IP address (203.0.113.2).
- Different port numbers were assigned to distinguish the translated connections.

## 6. Key Learning Outcomes

- Identifying NAT inside and outside interfaces.
- Configuring standard ACLs to identify internal addresses.
- Implementing PAT using the `overload` keyword.
- Understanding the use of a single public IP address for multiple internal devices.
- Configuring default and return static routes.
- Verifying NAT translations and troubleshooting connectivity.
