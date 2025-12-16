# FTP Configuration lab
In this lab I configured a basic routed network where multiple network clients can connect and access a centralized FTP server. The goal of this lab is to get hands on expereince with using FTP in network configurations and CLI usage.


## Network topology
This network consist of:
- Three client PC's on the same Network
- One FTP server
- Router
- Switch


![ftp-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/FTP/images/FTP%20server%20topology.png)


## IP Address setup
### Client Network
- Network: _192.168.1.0/24_
- Router (Default Gateway): _192.168.1.1_
- Client PC's:


_192.168.1.2_


_192.168.1.3_


_192.168.1.4_


![router-ip-arp](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/FTP/images/VLAN%20router%20ip%20arp.png)


### FTP Server Network
- Network: _10.10.10.0/24_
- Router interface (Default Gateway): _10.10.10.1_
- FTP Server:

  
IP Address: _10.10.10.10_


Subnet Mask: _255.255.255.0_


Default Gateway: _10.10.10.1_


![ftp-server-ipconfig](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/FTP/images/FTP%20server%20ipconfig.png)


## Configuration Summary
### Routers
- Interfaces were configured as static IP Addresses
- Router interfaces served as default gateways for connected devices

![router-interface](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/FTP/images/FTP%20server%20router%20interface%20config.png)


### FTP Server
- Static IP Configuration
- Local FTP User account created for authentication
- FTP Service enabled via: **Server > Services > FTP**
![ftp-server-services](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/FTP/images/ftp-server-services.png)


### Client PC's
- Static IP addressing (manual configuration)
- Default gateway set to router interface IP


### Testing & Verification
1. Ping Test:


```
ping 10.10.10.10
```


![ping-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/FTP/images/test1.png)


2. FTP test from a client PC:


```
ftp 10.10.10.10
```


![ftp-connection-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/FTP/images/test2.png)
