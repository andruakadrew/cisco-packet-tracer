# GRE Site-to-Site Tunnel lab
In this lab I created a network that implements the configuration and verification of a **Site-to-Site GRE tunnel** between two separate networks that are located in different parts of the country. I used **GRE** to create a virtual point-to-point link
across intermediate networks, allowing routed connectivity between private LANs.


## Network Topology
### Sites
- San Diego Office (Router1)
- El Paso Server Site (Router2)
- Internet/ISP Router (Router0)


### Logicial Design
- Router1 and Router2 are the edge routers for their LAN
- Router0 is the acting ISP router
- A GRE tunnel is formed directly between Router1 and Router2
- Router0 has no involvment in the GRE process, it's only job is to route the underlying WAN traffic

  
![gre-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/GRE/images/topology.png)


## Configuration
All three routers must get assigned an IP address for their connected interfaces and an IP route. Additionally, both Router1 and Router2 will be configured for GRE tunnels.


### Router1 Configuration
**Interfaces**
```
interface fa0/0
 description WAN-to-Router0
 ip address 209.165.200.226 255.255.255.252
 no shutdown
exit

interface fa1/0
 description LAN
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
```


**GRE Tunnel**
```
interface tunnel0
 ip address 10.10.10.1 255.255.255.252
 tunnel source fa0/0
 tunnel destination 209.165.200.230
 no shutdown
exit
```


**Routing**
```
ip route 192.168.20.0 255.255.255.0 10.10.10.2
end
wr
```


### Router2 Configuration
**Interfaces**
```
interface fa0/0
 description WAN-to-Router0
 ip address 209.165.200.230 255.255.255.252
 no shutdown
exit

interface fa1/0
 description LAN
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit
```


**GRE Tunnel**
```
interface tunnel0
 ip address 10.10.10.2 255.255.255.252
 tunnel source fa0/0
 tunnel destination 209.165.200.226
 no shutdown
exit
```


**Routing**
```
ip route 192.168.10.0 255.255.255.0 10.10.10.1
end
wr
```


### Router0 Configuration
**Interfaces**
```
interface fa0/0
 description WAN-to-Router1
 ip address 209.165.200.225 255.255.255.252
 no shutdown
exit

interface fa1/0
 description WAN-to-Router2
 ip address 209.165.200.229 255.255.255.252
 no shutdown
exit
```


**Routes for WAN**
```
ip route 192.168.10.0 255.255.255.0 209.165.200.226
ip route 192.168.20.0 255.255.255.0 209.165.200.230
end
wr
```


## Verification
**Check tunnel status on both Router1 and Router2**
```
show interface tunnel0
```


![tunnel-status-router1](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/GRE/images/tunnel%20status%20router1.png)
![tunnel-status-router2](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/GRE/images/tunnel%20status%20router2.png)


**Check routing**
```
show ip route
```


![ip-route-router1](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/GRE/images/ip%20route%20router1.png)
![ip-route-router2](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/GRE/images/ip%20route%20router2.png)


**Ping test**
From PC0
```
ping 192.168.20.10
```


![ping-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/GRE/images/ping%20test.png)
