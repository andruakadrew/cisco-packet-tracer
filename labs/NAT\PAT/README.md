# NAT/PAT Configuration lab
In this lab I configured Network Address Translation (NAT) and Port Address Resolution (PAT) to enable multiple devices with private IP addresses to share a single public IP address to access the internet. This represents how home and office networks
connect to the internet, where one public IP address is provided by the ISP.


![nat/pat-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/NAT%5CPAT/images/NAT-PAT%20topology.png)


## Adding and connecting devices
**Inside the Network:**
1. 3 Client pc's
2. 2960 Switch
3. NAT Router


**Outside the Network:**


4. Added second router (ISP router)
5. Added server (Web Server)


**Physical Connections:**
- PC0 -> Switch Fa0/1 (Copper-Straight-Through)
- PC1 -> Switch Fa0/2 (Copper-Straight-Through)
- PC2 -> Switch Fa0/3 (Copper-Straight-Through)
- Switch Gig0/1 -> Router1 Gig0/0 (Copper-Straight-Through)
- Router1 Gig0/1 -> Router2 Gig0/0 (Copper-Cross-Over)


## Configuring the NAT Router (Router1)
```powershell
Router> enable
Router# configure terminal
Router(config)# hostname Router1

# Configure INSIDE interface (facing private network)
Router1(config)# interface g0/0
Router1(config-if)# ip address 192.168.1.1 255.255.255.0
Router1(config-if)# ip nat inside
Router1(config-if)# no shutdown
Router1(config-if)# exit

# Configure OUTSIDE interface (facing public internet)
Router1(config)# interface g0/1
Router1(config-if)# ip address 203.0.113.2 255.255.255.252
Router1(config-if)# ip nat outside
Router1(config-if)# no shutdown
Router1(config-if)# exit

# Create access list to identify inside traffic for NAT
Router1(config)# access-list 1 permit 192.168.1.0 0.0.0.255

# Configure PAT (Port Address Translation) using overload
Router1(config)# ip nat inside source list 1 interface g0/1 overload

# Add default route pointing to ISP
Router1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1

Router1(config)# exit
Router1# write memory
```


## Configuring the ISP Router (Router2)
```powershell
Router> enable
Router# configure terminal
Router(config)# hostname Router2

# Interface facing NAT router (WAN link)
Router2(config)# interface g0/0
Router2(config-if)# ip address 203.0.113.1 255.255.255.252
Router2(config-if)# no shutdown
Router2(config-if)# exit

# Interface facing "internet" (where the server is)
Router2(config)# interface g0/1
Router2(config-if)# ip address 8.8.8.1 255.255.255.0
Router2(config-if)# no shutdown
Router2(config-if)# exit

# Static route back to private network (through NAT router)
Router2(config)# ip route 192.168.1.0 255.255.255.0 203.0.113.2

Router2(config)# exit
Router2# write memory
```


## Configuring the Web Server
I set up the server to simulate a public website on the internet. 


![web-server-ipconfig](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/NAT%5CPAT/images/web%20server%20ipconfig.png)


Since the server would be handling web traffic, HTTP and DNS must be turned _on_ in the _services_ tab


![web-server-services](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/NAT%5CPAT/images/web%20server%20services%20config.png)



## Configuring the Client PC's
I assigned each client pc a _static_ address for simplicity. 


**PC0:**


![pc0-ipconfig](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/NAT%5CPAT/images/pc0-ipconfig.png)


---


**PC1:**


![pc1-ipconfig](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/NAT%5CPAT/images/pc1-ipconfig.png)


---


**PC2:**


![pc2-ipconfig](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/NAT%5CPAT/images/pc2-ipconfig.png)


---


## Testing configurations
### Test 1: Basic connecivity to the Web Server


![test1](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/NAT%5CPAT/images/test1.png)


Results: First attempt I got 50% packet loss (2 out of 4 packets). The second attempt I got 100% success (4 out of 4 pakcets).


---


### Test 2: Viewing NAT translations


![test2](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/NAT%5CPAT/images/test2.png)


Results: Proves that NAT was translating the PC's private IP to the router's public IP.


---


### Test 3: Web Browsing


![test3](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/NAT%5CPAT/images/test3.png)


Results: Proves that NAT works for both ICMP (ping) and TCP (HTTP) traffic.


---


### Test 4: Verifying NAT statistics


![test4](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/NAT%5CPAT/images/test4.png)


Results: Shows that NAT is active with 1 translation, correct interfaces marked as outside/inside, and a mix of hit/missed packets.
