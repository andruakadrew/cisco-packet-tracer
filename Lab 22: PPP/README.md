# PPP Authentication — PAP & CHAP

A Packet Tracer lab configuring Point-to-Point Protocol authentication across serial WAN links — PAP between R1 and SPR1, and CHAP between R2 and SPR2. Both use two-way authentication where each side verifies the identity of the other.



## Topology

![ppp-topology](https://github.com/andruakadrew/cisco-packet-tracer/blob/main/images/ppp-topology.JPG)

| Link        | Network       | Authentication |
|-------------|---------------|----------------|
| R1 ↔ SPR1  | 100.0.0.0/30  | PAP            |
| R2 ↔ SPR2  | 200.0.0.0/30  | CHAP           |



## PAP vs CHAP

PAP (Password Authentication Protocol) sends credentials in plaintext during a two-way handshake — the authenticating router sends its username and password directly over the link. It is considered weak and is only used in legacy environments.

CHAP (Challenge Handshake Authentication Protocol) never sends the password. Instead it uses a three-way handshake — the authenticating server sends a challenge, the client responds with an MD5 hash of the challenge combined with the password, and the server verifies the hash. The password is never transmitted, making CHAP significantly more secure.

| | PAP | CHAP |
|---|---|---|
| Password sent over link | Yes — plaintext | No — MD5 hash only |
| Handshake | Two-way | Three-way challenge/response |
| Username requirement | Configurable | Must match remote router hostname |
| Key command | `ppp pap sent-username` | Local `username` entry only |



## PAP Configuration (R1 ↔ SPR1)

Two-way PAP requires each side to have a local username entry matching what the remote sends, and a `sent-username` statement defining what it sends to the remote. SPR1 is pre-configured — only R1 is configured here.

R1 stores credentials for SPR1 to authenticate against, and defines what it sends to SPR1:

```
username Packet password Tracer

interface s0/0
 encapsulation ppp
 ppp authentication pap
 ppp pap sent-username Cisco password CCNA
```

`username Packet password Tracer` — the account SPR1 uses when authenticating to R1.

`ppp pap sent-username Cisco password CCNA` — what R1 sends to SPR1 to prove its identity.



## CHAP Configuration (R2 ↔ SPR2)

CHAP authentication uses the router's hostname as the username. The local `username` entry must match the **hostname of the remote router** exactly — case sensitive. The password must be identical on both sides. No `sent-username` command is needed since CHAP handles the exchange automatically via the challenge/response mechanism.

```
username SPR2 password CCNA

interface s0/0
 encapsulation ppp
 ppp authentication chap
```

`username SPR2` matches SPR2's hostname. When SPR2 sends a CHAP challenge, R2 looks up `SPR2` in its local database, hashes the challenge with `CCNA`, and sends the response. SPR2 performs the same hash and compares — if they match, authentication succeeds.



## Verification

```
R1# show interfaces s0/0     
R2# show interfaces s0/0     
```

A successful PPP session shows:
```
Encapsulation PPP
LCP Open
Open: IPCP, CDPCP
```

`LCP Open` confirms PPP link negotiation succeeded. `IPCP Open` confirms IP addresses were negotiated over the PPP link. If authentication fails the interface stays `up/down` with `LCP Closed`.

To watch the authentication exchange in real time:
```
R1# debug ppp authentication
R2# debug ppp authentication
```

Disable after verifying:
```
R1# no debug ppp authentication
```
