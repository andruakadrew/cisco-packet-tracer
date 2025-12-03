# DHCP Server Configuration Project
In this lab, I configured a DHCP server to automatically assign IP addresses to numerous PC's on a network. This project helped me understand how DHCP works and how to 
properly troubleshoot common issues that occur when configuring DHCP settings.

## What I learned
Through this lab, I gained hands on experience with:
- Setting up a DHCP server with static IP configuration
- Creating and configuring DHCP address pools
- Assigning dynamic IP addresses to multiple client devices
- Configuring DHCP options like default gateway and DNS server
- Testing and verifying DHCP assignments on client PCs
- Troubleshooting DHCP issues (e.g. clients receiving wrong IPs)


### Network Topology
![dhcp-config-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/dhcp-config-topology.png)


---


### Service Settings
![dhcp-service-settings](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/dhcp-services-settings.png)


**DHCP POOL RANGE** -> 192.168.1.10 - 192.168.1.209 (200 addresses)


---


### DHCP Server IP Configuration
![dhcp-server-ipconfig](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/dhcp-server-ipconfig.png)


The DHCP IP _MUST_ be static, to ensure clients can always find it to request an IP address.

---


### Client PC 1 IP Configuration
![client-pc1-ip-config](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/pc1-ipconfig.png)


### Client PC 2 IP Configuration
![client-pc2-ip-config](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/pc2-ipconfig.png)


### Client PC 3 IP Configuration
![client-pc3-ip-config](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/pc3-ipconfig.png)
