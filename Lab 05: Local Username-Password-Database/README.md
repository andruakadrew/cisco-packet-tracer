# Local Username and Password configuration
## Overview
To prevent authorized access to the default gateway, usernames and passwords should be enabled. Implementing these features will ensure that only authorized users can enter the 
restricted User EXEC mode and Configuration terminal. Initial setup and configuration for Cisco devices are performed in the CLI, either by direct console
connection or through secure remote access (SSH). 

I performed these task in my Lab, first by connecting a PC to the router via the console port. After configuration, I tested the newly implemented features to ensure
that a username and password is required before accessing the router's EXEC mode.

## Features
- x1 Router (1941)
- x1 PC

## Router username and password during initial setup
To setup a Cisco router, you must first access the CLI. Use a console cable and connect the PC's _RS 232_ port into the Router's _Console_ port.

Once the cable is connected, log into the PC's terminal. A Command Line Interface will open.

## Steps
**1. Enter EXEC mode and Configuration mode**
```
R1>en
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#
```
**2. Enter the usernames and passwords**
```
R1(config)#username ccna password Cisco
R1(config)#username ccnp password Cisco
```
**3. Confiugre console to use local database**

This will tell the router to look for username/password inside the local database
```
R1(config)#line console 0
R1(config-line)#login local
```
**4. Test Login**

Exit out of the routers Configuration and EXEC mode, username and password are now prompted
```
User Access Verification

Username: ccna
Password: 

R1>
```

Examine the running-configuration file in EXEC mode. Valid usernames and passwords are shown.
```
R1#show running-config
Building configuration...

Current configuration : 714 bytes
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname R1
!
!
!
!
!
!
!
!
ip cef
no ipv6 cef
!
!
!
username CISCO password 0 router
username ccna password 0 Cisco
username ccnp password 0 Cisco
```

