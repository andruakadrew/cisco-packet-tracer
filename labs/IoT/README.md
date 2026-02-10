# IoT Lab
In this lab I configured multiple end devices to all be connected wirelessly to a networks default gateway. All devices have received IP addresses dynamically from the gateway and their power signal can be controlled from end devices (e.g. phones, PC, laptop).


## Topology
- 1 Home Gateway (DLC100)
- 1 Smartphone (PT)
- Garage IoT
  - Garage Door
  - Webcam
  - Carbon Dioxide Detector
  - Smoke Detector
- Living Room IoT
  - Home Speaker
  - Fan
  - Motion Detector
- Air Conditioner


_topology_
---
![topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/IoT/images/topology.png)


## Configurations
The first step in configuring this lab is to set up the home default gateway.
### Home Gateway
- LAN Interface > IPv4 Address > **192.168.25.1**
- Wireless Interface > Create unique **SSID** >> Select **WPA2-PSK** and create an authentication password.


_LAN interface_
---
![homegateway_lan](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/IoT/images/homegateway_lan.png)


_Wireless interface_
---
![homegateway-wireless](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/IoT/images/homegateway_wireless.png)


### SmartPhone
In order to manage IoT devices such as turning them on/off, or creating events you must have a end device such as a phone, PC, or laptop. These devices allow you to manage all connected IoT devices through the desktop. To wirelessly connect a phone to a network consist of:
- Config tab > Wireless0 Interface > Enter the networks **SSID** > Select the networks Authentication **(WPA2-PSK)** and enter the PSK Pass Phrase
- Ensure the encryprtion type matches with the network
- Receive a IPv4 address from DHCP


_Smartphone Wireless Interface_
---
![phone-wireless](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/IoT/images/smartphone_wireless_interface.png)


### IoT Devices
Every IoT Device must be powered on through the Physical tab, press the power button and a green light will appear resembling the device is now on


![iot-device-poweron](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/IoT/images/gateway_physcial_tab.png)


IP Configuration is done through:
- Config > Wireless Interface > Enter the networks **SSID** > Select the Networks Authentication > Enter the PSK Pass Phrase > Receive IPv4 Address from DHCP


Each IoT Device must modify their IoT Server settings from  _None_ to _Home Gateway_


IoT Server Settings
---
![iot-server-settings](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/IoT/images/iot_server_settings.png)


## Testing & Verification
Once all devices establish a wireless connection, the testing phase can begin. 
### Smartphone testing
Test connectivity to the Home Gateway by pinging it from the Smartphone
- Desktop > Command Prompt > enter the command _ping 192.168.25.1_


_Ping Results_
---
![ping-results](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/IoT/images/ping-results.png)


### IoT testing
All IoT devices connected to a network can be centerally managed by a smartphone or PC/Laptop that has admin access. To view active IoT connections within the network:
- Desktop > IoT Monitor > Enter the username and password > View all devices within the server


![IoT-devices](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/IoT/images/iot_monitor.png)
