# Cisco Packet Tracer Networking Project
I created a my own Cisco Packet Tracer home lab environment, designed to help me practice networking fundamentals, develop troubleshooting skills, and simulate a real world enterprise network.

## Overview
Hands on Cisco Packet Tracer labs designed to demonstrate core network concepts. Each lab focuses on the practical implementation of essential protocols and services used in modern networks.

### Technologies Covered
- DHCP (Dynamic Host Configuration Protocol) - Automated IP address assignment
- DNS (Domain Name System) - Name resolution services
- Static & Dynamic Routing - Network path determination
- VLANs - Network segmentation and traffic management
- NAT/PAT - Address translation techniques
- FTP - Secure communication through the FTP server
- GRE - Linking two separate networks using GRE tunneling
- ACL - Set up and enable permission access on a network
- Switching & Routing - Layer 2 and Layer 3 operations
- Wireless connectivity - End devices connect to networks using Wi-Fi
- Mail Server - Create user accounts and send emails
- HTTP Server - Exchange web traffic through various networks
- Home Network - Simulated Home Network containing a wireless router, the internet, and multiple end devices

### Labs included
1. [DHCP Server](https://github.com/andruakadrew/cisco-packet-tracer/tree/main/labs/DHCP%20Server%20Configuration)
- **Objective:** Configure DHCP Server to automatically assign IP addresses to network clients
- **Skills:** DHCP pool creation, IP address allocation, gateway and DNS configuration
- **Topology:** Multi PC network with centeralized DHCP server
  
---

2. [DNS Services](https://github.com/andruakadrew/cisco-packet-tracer/tree/main/labs/DNS%20Server)
- **Objective:** Implement DNS for hostname resolution  
- **Skills:** DNS Server configuration, forward/reverse lookup zones, DNS records
- **Topology:** Client server DNS architecture

 
---


3. [Routing Protocols](https://github.com/andruakadrew/cisco-packet-tracer/tree/main/labs/Routing)
- **Objective:** Configure static and dynamic routing
- **Skills:** Static routes, RIP, OSPF, EIGRP configuration
- **Topology:** Multi router network infrastructure


---


4. [VLAN Implementation](https://github.com/andruakadrew/cisco-packet-tracer/tree/main/labs/VLANs)
- **Objective:** Segment networks using Virtual LANs 
- **Skills:** VLAN creation, trunk ports, inter-VLAN routing
- **Topology:** Switched network with multiple VLANs


---


5. [NAT Configuration](https://github.com/andruakadrew/cisco-packet-tracer/tree/main/labs/NAT%5CPAT)
- **Objective:** Enable communication between private and public networks by using NAT/PAT features
- **Skills:** NAT/PAT enabling, CLI usage, Port configuration
- **Topology:** One local network and one outside network


---


6. [FTP Server](https://github.com/andruakadrew/cisco-packet-tracer/tree/main/labs/FTP)
- **Objective:** Connect multiple clients to a Server that provides FTP services. Use FTP in the command line to send and receive files
- **Skills:** Secure file transferring
- **Topology:** One local network with a server


---


7. [GRE Tunneling](https://github.com/andruakadrew/cisco-packet-tracer/tree/main/labs/GRE)
- **Objective:** Create secure site-to-site GRE tunnel between two networks
- **Skills:** Link tunneling, ISP router configuration, GRE configuration 
- **Topology:** Two networks located in different parts of the country, communicating internally through a GRE Linked tunnel


---


8. [Network Secuirty](https://github.com/andruakadrew/cisco-packet-tracer/tree/main/labs/ACL)
 - **Objective:**  Control network traffic using Access Control Lists
 - **Skills:** Standard and extended ACLs, traffic filtering
 - **Topology:** Segmented network with security policies


---


9. [Wireless Connectivity](https://github.com/andruakadrew/cisco-packet-tracer/tree/main/labs/Wireless%20connectivity)
- **Objective:** Connect different types of end devices (e.g. PCs, cell phones, laptops) to a network using Wireless technology
- **Skills:** Wireless Access Point and security implementation
- **Topology:** Multiple end devices connected to a centralized Wireless access point


---


10. [Mail Server](https://github.com/andruakadrew/cisco-packet-tracer/tree/main/labs/Mail%20server)
- **Objective:** Configure a server that provides mailing functions and test by sending emails from multiple accounts
- **Skills:** POP3/SMTP Services, User account security
- **Topology:** One local network with a server


---


11. [HTTP Server](https://github.com/andruakadrew/cisco-packet-tracer/tree/main/labs/HTTP)
- **Objective:** Configure a HTTP server and exchange web traffic between different networks 
- **Skills:** HTTP services, HTML scripting
- **Topology:** Multi network layout with one router, one switch for each network, and one server


---


12. [IoT Services](https://github.com/andruakadrew/cisco-packet-tracer/tree/main/labs/IoT)
- **Objective:** Connect multiple IoT devices to a network and view them through a IoT Monitoring tool
- **Skills:** Wireless connectivity, Interface configuration
- **Topology:** A single smartphone and a Home Gateway wirelessly connected to various IoT devices


---


13. [Home Network Simulation](https://github.com/andruakadrew/cisco-packet-tracer/tree/main/labs/Home%20Network)
- **Objective:** Set up a home network using common devices such as a Wireless router, Cable Modem, Laptops, and a Office PC
- **Skills:** Network implementation, Router installation
- **Topology:** A home environemnt that contains a Wireless router, Cable modem, and multiple end devices such as TV's, PCs and laptops. All network devices in the home are connected to the router, which is then connected to the Modem and provides internet for all users connected to the home network
