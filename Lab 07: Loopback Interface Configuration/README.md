# Loopback Interface Lab
## Overview
Loopback interfaces serve many functions in a router. For example,
1. The main function of the Loopback Interface is to always remain available.
2. Reachbility can be accessed via remote login using the Loopback Interfaces IP address.
3. As the IGP and BGP rely on neighbors to form neighbor relationships, Loopback interfaces are preferred for router ID's, as they will not change, even when the physical interface or the path between the
neighbors fail.
4. The Loopback Interface can be used during testing when no other IP addresses are available beyond the router or can't connect to a device and be checked remotely.

In this lab, I will configure the Loopback Interface for two routers connected together. First, the IP address and subnet mask of the router interface must be configured for both routers. Then, the Loopback
interface can be configured with its IP and subnet mask.

## Steps
**Configure Physical Interfaces**

**On R1:**
```
enable
configure terminal

interface g0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
end
```

**On R2:**
```
enable
configure terminal

interface g0/0
ip address 192.168.1.2 255.255.255.0
no shutdown
end
```

---

**Configure Loopback Interfaces**

**On R1:**
```
configure terminal

interface loopback 0
ip address 1.1.1.1 255.255.255.255
end
```

**On R2:**
```
configure terminal

interface loopback 0
ip address 2.2.2.2 255.255.255.255
end
```

---

## Ping test
**From R1: Ping local interface**
```
ping 1.1.1.1
```

**From R2: Ping local interface**
```
ping 2.2.2.2
```
