# VLAN Configuration
In this lab, I configured multiple VLANs (Virtual Local Area Networks) to segment a single physical network into separate logical networks. I implemented inter-VLAN routing using a router-on-a-stick configuration, allowing different VLANs to communicate
through a central router. This project also involved setting up multiple DHCP pools to serve each VLAN separately.


![vlan-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/VLANs%20topology.png)


## Creating VLANs on the switch
To create the VLANs, go to Switch > Config > VLAN Database. There you can add new configurations such including the _VLAN No._ and _VLAN name._ In my lab I created three different VLANS.


VLAN 10 ---> SALES


VLAN 20 ---> HR


VLAN 30 ---> IT


![vlan-database](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/VLAN%20database.png)


## Port Configurations
It's important that you assign each client device their respected VLAN. For example, if I have _Client PC44_ who belongs to the IT Department on VLAN 30, and that client PC is connected to the switch port via _fastEthernet0/9_. That exact port
'fastEthernet0/9' must be configured for VLAN 30 on the switch and ensure the port is in access mode. In my lab, I have three client PC's assigned to each of my three VLANs.


![vlan10-config](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/VLAN10%20port%20config.png)


_FastEthernet0/4 ---> PC0_


---


![vlan20-config](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/VLAN20%20port%20config.png)


_FastEthernet0/5 ---> PC1_


---


![vlan30-config](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/VLAN30%20port%20config.png)


_FastEthernet0/6 ---> PC2_


---

### Configure router as trunk port
Another aspect of properly configuring ports is assigning the router as a trunk port. This allows a single physical cable to carry traffic for all different VLANs between the router and the switch. This also provides the benefit of enabling inter-VLAN
communication by tagging frames with their specific VLAN ID, saving ports and creating efficient routing between separated subnets.


![router-port-config](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/Router%20port%20config.png)


_FastEthernet0/3 ---> Router_


## Configuring Router Subinterfaces
On the router, I created subinterfaces for each VLAN as part of the Router-on-a-Stick configuration process. Each subinterface acts as the default gateway for its respective VLAN.


![vlan-subinterfaces](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/VLAN%20subinterfaces.png)


## Configuring DHCP Server
When moving from a single VLAN network address (e.g. 192.168.1.1) over to a network consisting of multiple VLANs, the static addresses of all servers must get adjusted. In my DHCP server, I assigned it the network address _192.168.10.2_ and the 
Default Gateway _192.168.10.1_


![dhcp-address-settings](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/DHCP%20server%20ipconfig%20update.png)


### DHCP Pool Configuration
For every VLAN you create, there must be a Pool range which define groups of IP addresses that the DHCP server can assign. In my DHCP server, I created 3 Pool ranges, one for each VLAN.


![dhcp-pool-ranges](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/VLAN%20pool%20ranges.png)


**Note:** Since the DHCP server is in VLAN 10, I needed to configure DHCP relay (IP helper) on the router for VLAN 20 and 30.


## Testing Configurations


![vlan-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/vlan-test.png)


**Verify VLAN creation**


---


![trunk-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/trunk-test.png)


**Verify trunk port configuration**

---


![subinterface-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/subinterfaces-test.png)


**Verify subinterfaces**


---


![routing-table-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/subinterfaces-test.png)


**Verify routing table**


---

**Note:** I will be using Client PC5 on VLAN20 to simulate client device connectivity.


![ipconfig](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/ipconfig-test.png)


**_ipconfig_ - Verify IP assignment**


---


![ping-vlan-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/ping-neighbor-vlan-test.png)


**_ping 192.168.10.10_ - Test connectivity with a different VLAN**


---


![router-gateway-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/VLANs/images/router-gateway-test.png)


**_ping 192.168.10.1_ - Test connectivity to router gateway**





