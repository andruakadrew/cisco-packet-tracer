# HTTP Server Configuration
In this lab I configured a server and enabled HTTP/HTTPS services for a simple two network layout. I edited the _index.html_ file and tested the web page using each device from both networks.


## Topology
- 1 **ISR4321** router
- 1 **Server-PT**
- 2 **2960** switches
- 2 **PCs**


## Routing
All IP addresses are statically assigned. The client PC on Network 1 received the address _192.168.1.10_ and the client PC on Network 2 received _192.168.2.10_. The router interface facing Network 1 was assigned
the address _192.168.1.1_ and the interface facing Network 2 _192.168.2.1_. The server was assigned to the Network B portion, given the address _192.168.2.2_.



![topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/HTTP/images/topology.png)



## Web Server
To enable web services, turn on HTTP/HTTPS in the _Services_ desktop tab. In the tab will contain a file manager containing multiple files (e.g. html, jpg).


![web-services](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/HTTP/images/http%20services.png)


### Modify HTML file
In the _index.html_ file contains a program that creates a basic web page. I edited some of the content so that way it would be easy to distinguish during the testing phase.


![html-modification](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/HTTP/images/edit%20html%20file.png)


## Ping Test
Since the web server should be accessable to various networks, I used the **ping** command to ensure reachability to the host.


![ping-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/HTTP/images/ping%20server.png)


## Web Browser Test
On both PC's I entered the web server IP address into the URL. I ensured that all devices on each network were able to access the webpage successfully.


![webpage-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/HTTP/images/test%20webpage.png)

