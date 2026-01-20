# Mail Server Lab
Inside this lab I implemented a centralized mail server, a single switch, and three client PC's. I enabled SMTP and POP3 on the mail server and created a domain name.
Then I setup three user email accounts and tested them by sending emails back and fourth.


## Network Topolgy
- 1 2960 Switch
- 1 Server-PT
- 3 PC's

![topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Mail%20server/images/topology.png)


## Configuration
### IP addressing
Each devices has been assigned an static IP address. The client PC's were assigned _192.168.1.10-12_, and the Server _192.168.1.1_


### Mail Server
The server must have Email services enabled. To do this,
1. Open **Services**
2. Enable:
   - SMTP
   - POP3
3. Create user accounts


![server-config](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Mail%20server/images/server-config.png)


## Testing & Verification
### Ping test
To test reachability between the client PC and the Server, **Ping** the IP Addresses.


![ping-test](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Mail%20server/images/ping-test.png)


### Email test
On each device in the **Email** Desktop tab, I configured all three user accounts. Next, I sent a test email from one user to another user.

1. Send an email from one user account email to another user account.
![send-email](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Mail%20server/images/send-email.png)


---

2. Log into the user account and ensure that it received the email.
![receive-email](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/labs/Mail%20server/images/receive-email.png)






