# DHCP and DNS Server Configuration Using Cisco Packet Tracer

**Computer Networks Lab Project**
**Spring Semester 2026**

---

## Team Members

| Name | Student ID |
|------|------------|
| Mubashra Imaan | F2024376359 |
| Fatima Rehan | F2024376172 |
| Rameen Zainab | F2024376148 |

**Instructor:** Samar Ikram

---

## Table of Contents
1. [Introduction](#introduction)
2. [Project Objectives](#project-objectives)
3. [Devices Used](#devices-used)
4. [Network Topology](#network-topology)
5. [Configuration Steps](#configuration-steps)
   - [DHCP Server Configuration](#dhcp-server-configuration)
   - [DNS Server Configuration](#dns-server-configuration)
   - [Router Configuration](#router-configuration)
6. [DHCP Relay Configuration](#dhcp-relay-configuration)
7. [Client Verification](#client-verification)
8. [Final Topology](#final-topology)
9. [Conclusion](#conclusion)

---

## Introduction

This project involves the configuration of DHCP (Dynamic Host Configuration Protocol) and DNS (Domain Name System) servers within a simulated network environment using Cisco Packet Tracer. The network comprises multiple subnets, each containing client PCs that obtain their IP configurations automatically from the DHCP server. The DNS server is utilized for domain name resolution, enabling clients to access resources using hostnames rather than IP addresses.

---

## Project Objectives

- Configure a DHCP server to automatically assign IP addresses to network clients
- Set up a DNS server for domain name resolution
- Implement DHCP relay on the router to facilitate DHCP communication across different subnets
- Verify successful IP address allocation and domain name resolution for all clients
- Understand the role of DHCP and DNS in modern network environments

---

## Devices Used

| Device Type | Quantity |
|-------------|----------|
| DHCP Server | 1 |
| DNS Server | 1 |
| Switches | 4 |
| Router | 1 |
| PCs | 15 |

---

## Network Topology

The network is divided into three separate subnets:

- **Subnet 1:** 192.168.1.0/24 (Clients connected via Switch 1)
- **Subnet 2:** 192.168.2.0/24 (Clients connected via Switch 2)
- **Subnet 3:** 192.168.3.0/24 (Clients connected via Switch 3)
- **Server Network:** 192.168.0.0/24 (DHCP and DNS servers)

### Topology Diagram

![Screenshot 2024-10-15 164328](https://github.com/user-attachments/assets/deeb4dce-0590-476d-aab7-db71bdb243bf)

---

## Configuration Steps

### 1. DHCP Server Configuration

#### IP Address Assignment

The DHCP server is assigned a static IP address to ensure consistent communication with clients across all subnets.

| Setting | Value |
|---------|-------|
| IP Address | 192.168.0.69 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.0.1 |

![Screenshot 2024-10-15 164434](https://github.com/user-attachments/assets/062941dc-ce5d-4d20-a7e2-c7a1458530ec)

#### DHCP Pools Configuration

Three DHCP pools are created to serve clients on different subnets:

| Pool Name | Network Address | Range | Default Gateway |
|-----------|-----------------|-------|-----------------|
| serverPool | 192.168.0.0/24 | 192.168.0.100 - 192.168.0.199 | 192.168.0.1 |
| Pool-1 | 192.168.1.0/24 | 192.168.1.100 - 192.168.1.199 | 192.168.1.1 |
| Pool-2 | 192.168.2.0/24 | 192.168.2.100 - 192.168.2.199 | 192.168.2.1 |
| Pool-3 | 192.168.3.0/24 | 192.168.3.100 - 192.168.3.199 | 192.168.3.1 |

**Additional DHCP Options:**
- DNS Server: 192.168.0.2
- Domain Name: example.com

![Screenshot 2024-10-15 164615](https://github.com/user-attachments/assets/03c67843-54ac-4f30-b982-9c384c4458ee)

---

### 2. DNS Server Configuration

#### IP Address Assignment

| Setting | Value |
|---------|-------|
| IP Address | 192.168.0.2 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.0.1 |

![Screenshot 2024-10-15 164742](https://github.com/user-attachments/assets/f371d9c5-d8e2-4e9b-896a-6547d8d57dc4)

#### DNS Records

DNS records are configured to map domain names to IP addresses. Sample entries include:

| Domain Name | IP Address |
|-------------|------------|
| server.example.com | 192.168.0.69 |
| www.example.com | 192.168.1.100 |

---

### 3. Router Configuration

#### Interface Configuration

The router is configured with the following IP addresses:

| Interface | IP Address | Connected Network |
|-----------|------------|-------------------|
| G0/0 | 192.168.0.1/24 | Server Network |
| G1/0 | 192.168.1.1/24 | Subnet 1 |
| G2/0 | 192.168.2.1/24 | Subnet 2 |
| G3/0 | 192.168.3.1/24 | Subnet 3 |

---

### 4. DHCP Relay Configuration

Since the DHCP server is on a different network than the clients, DHCP relay is configured on the router interfaces to forward DHCP requests to the DHCP server.

#### Configuration Commands

Router> enable
Router# configure terminal
Router(config)# interface g0/0
Router(config-if)# ip helper-address 192.168.0.69
Router(config-if)# exit
Router(config)# interface g1/0
Router(config-if)# ip helper-address 192.168.0.69
Router(config-if)# exit
Router(config)# interface g2/0
Router(config-if)# ip helper-address 192.168.0.69
Router(config-if)# exit
Router(config)# interface g3/0
Router(config-if)# ip helper-address 192.168.0.69
Router(config-if)# exit
Router(config)# exit
Router# write memory

**Note:** The `ip helper-address` command relays DHCP broadcast messages to the specified DHCP server IP address, allowing clients across different subnets to obtain IP configurations.

---

## Client Verification

Each client PC is configured to obtain its IP address automatically via DHCP.

### Client IP Configuration

![Screenshot 2024-10-15 165443](https://github.com/user-attachments/assets/4e426bf9-7dfd-4fa9-a60d-1a48b60cc54b)

**Verification Steps:**

1. Set each PC's IP configuration to DHCP
2. Verify the assigned IP address using `ipconfig` command
3. Test connectivity using `ping` to other devices
4. Verify DNS resolution using `nslookup` or `ping` with domain names

### Sample Verification Commands
C:> ipconfig

Ethernet adapter Local Area Connection:
IP Address. . . . . . . . . . . . : 192.168.1.100
Subnet Mask . . . . . . . . . . . : 255.255.255.0
Default Gateway . . . . . . . . . : 192.168.1.1
DHCP Server . . . . . . . . . . . : 192.168.0.69
DNS Servers . . . . . . . . . . . : 192.168.0.2

C:> ping 192.168.2.100
Reply from 192.168.2.100: bytes=32 time=1ms TTL=128

C:> ping server.example.com
Reply from 192.168.0.69: bytes=32 time=1ms TTL=128

---

## Final Topology

### Complete Network Layout

![Screenshot 2024-10-15 165728](https://github.com/user-attachments/assets/0d776bab-f279-4dbe-bdec-e4a16321ed5b)

The final topology includes:

- **DHCP Server** (192.168.0.69) providing IP addresses to all clients
- **DNS Server** (192.168.0.2) resolving domain names
- **Router** acting as gateway and DHCP relay agent
- **4 Switches** connecting clients in their respective subnets
- **15 PCs** distributed across three subnets

---

## Conclusion

The successful configuration of DHCP and DNS servers in this project demonstrates several key networking concepts:

1. **DHCP Automation:** Clients can automatically obtain IP addresses, subnet masks, default gateways, and DNS server information without manual configuration.

2. **DHCP Relay:** The use of the `ip helper-address` command enables DHCP communication across different subnets, centralizing IP address management.

3. **DNS Resolution:** Domain names are resolved to IP addresses, simplifying network resource access and making the network more user-friendly.

4. **Scalability:** The configuration supports dynamic IP allocation, making it suitable for large and growing networks.

### Learning Outcomes

- Understanding DHCP server configuration and pool management
- Implementing DHCP relay on a router
- Configuring DNS server for domain name resolution
- Verifying network connectivity and configuration
- Troubleshooting common network configuration issues

### Future Enhancements

- Implement DHCP reservation for specific clients
- Configure DHCP options for advanced settings (e.g., VOIP, TFTP servers)
- Implement DHCP snooping for security
- Configure a secondary DHCP server for redundancy
- Implement dynamic DNS updates

---

## References

- Cisco Networking Academy. (2024). CCNA Introduction to Networks
- Cisco Systems. (2024). DHCP Configuration Guide
- Cisco Packet Tracer Documentation

---

*This project was completed as part of the Computer Networks Lab course, Spring Semester 2026, under the supervision of Ms. Samar Ikram.*
### Explanation of the Project
## What is DHCP?
DHCP (Dynamic Host Configuration Protocol) is a network protocol that automatically assigns IP addresses and other network configuration parameters to devices on a network. This eliminates the need for manual IP configuration on each device.

### What is DNS?
DNS (Domain Name System) translates human-readable domain names (like www.example.com) into IP addresses that computers use to communicate. This makes the network more user-friendly.

### How Our Network Works
DHCP Server (192.168.0.69):

Has three separate address pools for different subnets

Automatically assigns IP addresses to clients when they connect

Also provides default gateway and DNS server information

DNS Server (192.168.0.2):

Resolves domain names to IP addresses

Clients can use domain names instead of remembering IP addresses

Router:

Acts as the gateway connecting different subnets

Forwards DHCP requests from clients to the DHCP server using ip helper-address

Routes traffic between different subnets

Clients:

15 PCs distributed across 3 subnets

Each automatically receives an IP address from the DHCP server

Can access resources using domain names via the DNS server

### What We Achieved
Automated IP address management for all 15 clients

Centralized configuration control through the DHCP server

Domain name resolution through the DNS server

Seamless communication between devices across different subnets

