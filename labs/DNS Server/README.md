# DNS Server Lab
I implemented a DNS Server alongside a DHCP Server. DNS Servers are crucial for a working environment, 
where devices need to resolve domain names using a local DNS server. I used tools to verify functionality 
of the DNS Server using _ping_ and _nslookup_.


## Lab Overview
In this setup, I used:
- DNS Server (static IP: 192.168.1.3)
- DHCP Server (static IP: 192.168.1.2)
- Router (gateway: 192.168.1.1)
- Switch
- Client PCs receiving IP configuration automatically through DHCP.
The DNS Server returns an A record for the domain _andru.com_, which points to _192.168.1.50_
![dns-server-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/DNS%20Server/images/DNS%20Server%20Topology.png)


## DNS Server IP Configuration
The first thing I did was assign the DNS Server to a static IPv4 address so it would remain consistent on the network.
![dns-ipconfig](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/DNS%20Server/images/DNS%20Server%20ipconfig.png)


## DNS Service Configuration
Inside the DNS Services tab, I enabled DNS and added an A record.
![dns-services](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/DNS%20Server/images/DNS%20Services.png)


## Updating the DHCP Server
Since clients receive their network configuration from DHCP, I updated the DHCP scope to hand out the DNS server's
correct IP address. 
![dhcp-server-update](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/DNS%20Server/images/DHCP%20services%20update.png)


## Testing with _nslookup_ and _ping_
After the clients pulled the new DNS settings, I tested by querying _andru.com_ using the tool _nslookup_.
After confirming the DNS exist and is working correctly, I use the _ping_ tool to test network connectivity to the domain.
![dns-server-confirmation](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/DNS%20Server/images/DNS%20Server%20ping%20and%20query.png)
_Note: I pinged the domain using a client PC that I manually assigned the IP Address: 192.168.1.50, in order
to successully get a response from the domain. Any other IP Address will not receive a response after pining andru.com since
the DNS Record only points to the IP Address: 192.168.1.50_
