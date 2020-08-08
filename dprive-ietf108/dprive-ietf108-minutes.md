
---
title: DPRIVE IETF 108
---
# DNS Privacy Exchange (DPRIVE) WG
## IETF108 

* Date: Friday 31 July 2020
* Time: 1410- 1550 UTC
* MeetEcho: http://www.meetecho.com/ietf108/dprive/

### Chairs
* Tim Wicinski tjw.ietf@gmail.com
* Brian Haberman brian@innovationslab.net

### Very Responsible Area Director
* Eric Vyncke evyncke@cisco.com

### DataTracker
* https://datatracker.ietf.org/group/dprive/documents/

#
## Agenda

### Administrivia
* Agenda Bashing, Blue Sheets, etc,  10 min

### Current Working Group Business

*   DNS Zone Transfer-over-TLS
    - https://datatracker.ietf.org/doc/draft-ietf-dprive-xfr-over-tls/
    - Authors, 20 min
    - Chairs Action: closer to WGLC?

Daniel Gilmor: ALPN handshake is cleartext so appears as zone transfer over aDOT query.  Risks of distinguisability? 
Sara: Would have to look at padding and timing to disguise it. Determine which nameservers host zones. 
DKG: Detect Zone Transfer in order to prevent it. 

Peter van Dijk: limit ALPN to SOA and XFR, PowerDNS does NSQuery. 
Sara: did not want to force servers to answer over an encrypted connection if not supported. 

Jim Reid: consider TLS session resumption work
Sara: Little Premature

Jon Reid: Favor of ALPN

EKR: What happens right now when I ask for a zone xfr over tcp. Why ALPN
Sara: What we didn't want the requirement that if auth was supporting xot, it had to be ready to answer query over that connection.
EKR: They can refuse to answer those queries
Sara: We think its clearer. Also, IP Based ACL/TSIG, Determine when to control zone transfer access
EKR: Not what ALPN intended for.
Sara: Weaknesses in both approaches

DKG: If you done xfr over this connection, this is how it is done.

Petr Spacek: if Draft says handle these queries, and the rest you send back refused. 
Sara: Not all operatoers agree to open TLS queries

Erik Nygren: use SNI for this?
Sara: would we allow any queries for those SNI
Sara: Not using ALPN is a tuning problem

### New Working Group Business

*   Signalling Authoritative DoT support in DS records, with key pinning  
    - https://datatracker.ietf.org/doc/draft-vandijk-dprive-ds-dot-signal-and-pin/:  
    - Peter van Dijk, 20 min
    - Chairs Action: Adopt?

*   Opportunistic Encryption Use Case
    - No draft as of yet
    - Paul Hoffman, time permitting
    - Chairs Action: ??

