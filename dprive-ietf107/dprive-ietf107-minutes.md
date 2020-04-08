 DNS Privacy Exchange (DPRIVE) WG

### IETF 107 Interim 

* Date: 8 April 2020
* Time: 1600-1730 UTC
* WebEx: https://ietf.webex.com/ietf/j.php?MTID=m88e3d0d091544ea59c79eb5e610a115a

###
* Jabber:  dprive@jabber.ietf.org
* EtherPad: https://etherpad.ietf.org:9009/p/notes-ietf-interim-2020-dprive-01?useMonospaceFont=true

### Chairs
* Tim Wicinski tjw.ietf@gmail.com
* Brian Haberman brian@innovationslab.net

### Very Responsible Area Director
* Eric Vyncke evyncke@cisco.com

### Datatracker
* https://datatracker.ietf.org/group/dprive/documents/

# 
## Agenda

Technical issues move meeting start to 16:10 UTC
58 Participants

### Administrivia (16:10 UTC)
    * Agenda Bashing, Blue Sheets, etc,  10 min

Tim & Brian open the meeting, share chair slides, Agenda, note well.

https://datatracker.ietf.org/doc/slides-interim-2020-dprive-01-sessa-chairs-slides/

### Current Working Group Business

Tim: (16:14UTC)

7626-bis: One outstanding item, sorted out with the IESG at the moment.
BCP going with 7626-bis.

Phase 2: reqs - will sit with jason/benno/alex to sort out open issues

XFR over TLS: pretty quiet around that doc, expection new version from authors

####   DNS over Quic (16:16 UTC)
    - https://datatracker.ietf.org/doc/draft-huitema-quic-dnsoquic/
    - Christian Huitema, 15min
    - Chairs Action: Adopt?

Slides: https://datatracker.ietf.org/doc/slides-interim-2020-dprive-01-sessa-dns-over-quic/

Started in quic WG, and was sent over to DPRIVE.

Christian explains packet flow of proposed DNS over QUIC - 1-RTT and (reusing connection 0-RTT)
Uses mechanisms of QUIC itself for flow control
Responses don't have to be sent in order. Multiple responses can be sent in identical UDP packet.

Why & why now?

DoH3 should work as DoH, but has a "higher stack" compared to DoQ (no HTTP layer)
Why now? QUIC is almost ready, spec largely frozen, DoQ does not require changes to QUIC
Means we can work on DoQ outside of / on top of QUIC -> no req back to QUIC WG.

Work for DPRIVE: what to do with EDNS, what policy for 0-RTT (security & privacy implications)

Questions:

ekr: neutral on adopting it two technical comments: Why dedicated port? ESNI/ECHO: large numbers of environments will probably suppress ECHO
Christian: comments are correct. comments apply also to DoT - not a new debate, identical issues.
Dedicated port makes it easire for net mgrs to identify what's going on

Jim Reid: Good idea, support adoption. Why Query ID MUST be set to 0?
Christian: Must be clear what we demux on, either on stream number OR on query ID, making Query ID 0 makes it clear. makes it simpler.

Ben Schwartz: ECHO does not apply to DNS trasports, as currently specified. can't do ECHO
Seems at it will absolutely work, very small performance diff vs. DoH, maybe more relevant in recursive to auth.
Christian: motivation was designing it for recursive to authoritative. lightweight protocol has a smaller 
attack surface. will work on recursive to auth later

Ted Hardie: in favour of supporting adoption, very good use of QUIC, has head of line blocking avoidancein stub to recursive, which is very useful. esp. with 0-RTT recursive to authoritiave is very useful. Understand people want to do DoH o QUIC as well, but caching is simpler in DoQ. WG should 
take up this work.

Brian: Tim will issue adoption call.

####   Using Early Data in DNS over TLS (16:40 UTC)
    - https://datatracker.ietf.org/doc/draft-ghedini-dprive-early-data/
    - Alessandro Ghedini, 15 min
    
Update on this individual draft.

TLS 1.3 introduced 0-RTT session resumption, called "early data". Can be used to send DNS
queries. eg when long lived connections are not feasible (eg. infrequently used connections in recurs to auth)

Caveat: early data can be intercepted / replayed in encrypted form -> TLS 1.3 requires to define for
app protocol when it is safe to do so -> this is the contents of this document.

Draft says one "query" opcode are allowed (removes edge cases previously not considered)
Introduces "whitelist" of "safe" RR types to be asked for in early data (IANA registry)
List is currently incomplete -> discuss additions on adoption?

Not 100% sure we need the RR type whitelist - is restricting on "query" opcode enough? 

Questions:

Ben Schwartz: Supports adoption, thinks we don't need a list.
Ben Kadac: Reasonable to have a list both for opcodes and RR types. "Needs to be idempotent" requires a stronger criterion. Maybe "query needs same result when re-ordered with other queries"? 
Alessandro: Will clarify

Christian Huitema: Misses one point that dkg has been making: attacker can replay attack, and look at side effects (eg. requests at other nameservers triggered by that request.. leaks) - best to go to dkg's original analysis.
Alessandro: draft contains some sec. considerations from a couple of years ago, will maybe need something around caching, will add text, but accepts PRs

Brian: Tim & Brian are going to raise the question of adoption, please indicate on list.

    - Chairs Action: Adopt?
    
Slides: https://datatracker.ietf.org/meeting/interim-2020-dprive-01/materials/slides-interim-2020-dprive-01-sessa-using-early-data-in-dns-over-tls

### New Working Group Business

####   Signaling resolver's filtering policies (16:50 UTC)
    - https://datatracker.ietf.org/doc/draft-mglt-add-signaling-filtering-policies/
    - Daniel Migault, 15min
    - Chairs Action: ?
    
Slides: https://datatracker.ietf.org/meeting/interim-2020-dprive-01/materials/slides-interim-2020-dprive-01-sessa-filtering-signaling-policies

Filtering policies by resolvers are important for selection of resolver. believe standard mechanism is required.

Document defines a) resolver informing client about filtering b) client requests filtering policies which are in place

History of similar communication: eg. TA (8145), sentinel (8509)

Draft is inspired by 8145 - filtering policy is represented by DATA
REsolver advertises by responding with DATA in an EDNS0 response.
Client asks for specific qname.

Table with "filtering descriptions"

QNAME is _filtering_policies.example.com with example.com the domain name of the resolver

considerations: EDNS0 is not DNSSEC protected. should be more characteristics of the filtering be included?
 
Alex: a) table with filtering policies is a can of worms. "illegal" in what jurisdiction? server? client? b) qname using the domain is not very elegant, but doesn't have a solution to that. how do we identify the "domain" from the nameserver name?
Daniel: Yes, table is a discussion point. Can go anything from binary "yes, we filter" to a detailed list. 
reverse resolution? We can also agree on a very "specific" name, like "home.arpa" etc..

ekr: Do you know of anyone wanting to consume this information? 
Daniel: could be the client using the canary domain? There has to be a use case.
ekr: detail on filtering policy creates lot of ambigiuty. wants to know "is there policy enforcement yes or no". Is more confusing. Plus, "will the OS allow me to get that data at all"
Daniel: suggest we drop the EDNS0 option? Limit the scope on the DNS exchange?
ekr: requirement is to "get through" the existing OS DNS APIs

Chris Box: cloudflare offers different IPs for different filtering policies. maybe useful when in a hotel 
to learn what a resolver applies. Why not in an ADD protocol?
Daniel: yes, might be available via different "discovery" protocols.. this is complimentary..

Vittorio: Useful only if there are clients implementing it. problem between too complex when we add detail to categories. go back what we want to communicate to whom, and then go ahead. Should make sure it goes to the right WG (DPRIVE or ADD?)
Eric: People agree we should not have a detailed filtering policy description. Work could be hosted in ADD, but ADD chairs said DPRIVE is more appropriate.  Different way to advertise outside ADD protocols, but doesn't think it conflicts. We should not have overlap.

Ben Schwartz: Should not be in DPRIVE, not a privacy enhancement. Clearly overlaps with some ADD work. If not included there, then reflects lack of space in ADD, and might need to be combined with exisitng proposals there? Does want a uniformed discovery mechanism for resolver info. This is an "Evil Bit". cannot be verified, contrary to the canary domain which *measures* and *verifies*. 
Daniel: shares Ben's point, but thinks not everything can be rolled into the discovery protocol
Ben: didn't say we need everything in the discovery protocol, but we need a uniformed "metadata" protocol instead. See "RESINFO" adopted draft - thinks that is the architecture we should be aiming for.
(https://tools.ietf.org/html/draft-ietf-dnsop-resolver-information-01)
Daniel: DPRIVE was suggested by ADD chairs.

Barry: ADD charter is very narrow by design. might need to hold off on this for now
Tim: Agree with what Ben said - we need a common framework - all very interseting, but have all their own ways. resolver info can maybe be extended 

Benno (Jabber comment): Why not use a bitmask for the filtering policy?
Daniel:If we remove the filtering policies, we don't need to think about it, but yes, could use bitmask.

Jim: Why EDNS option, and not RRTYPE?
Daniel: Had already experience in that field, but can take other suggestions.

Erik Kline: Jurisdictions problem - clients & servers are in "some" jurisdiction. "illegal" might not be a useful signal. etc.. citizenship of clients.. 
Daniel: Tried to reflect the options of filtering, agrees that "illegal" might be vague, but meant "illegal content" 


# 
## Reference 

### BlueSheets

Attendees are asked to visit and enter your Name+Affiliation in the Blue-Sheet section of the DPRIVE Etherpad.

### Mic Line Queue

The Mic Line will use the WebEx chat channel.  To get in the queue type q+ to leave type q-.
Please don't type questions or other things into the WebEx chat channel as that will make
managing the queue very hard for the chairs.  Please use the Jabber channel for side conversations.
 
When you connect into WebEx you should start off as auto-muted so you'll 
need to unmute yourself to speak when called.

### Helpful Info & Prep

The IETF has prepared a couple of documents to help get everyone ready, 
including a reminder that you need to register for IETF107 as a remote participant to join remotely.
 
  https://www.ietf.org/how/meetings/107/session-participant-guide/
 
  https://www.ietf.org/how/meetings/107/session-presenter-guide/

=======
Attendee List

Brian Haberman, Johns Hopkins University
Sean Turner, sn3rd
daniel migault ericsson
Scott Hollenbeck, Verisign
Eric Orth, Google
Alex Mayrhofer, nic.at
Vittorio Bertola, Open-Xchange
Ted Hardie, Google
Éric Vyncke, Cisco
Mike Bishop, Akamai
Rafael Obelheiro, State University of Santa Catarina (Brazil)
Jonathan Hoyland, Cloudflare
Benno Overeinder, NLnet Labs
Wes Hardaker, ISI
Erik Kline, Loon LLC
Christian Huitema, Private Octopus Inc.
Michael Breuer, ilSF
Lucas Pardue, Cloudflare
Barbara Stark, AT&T
Shumon Huque, Salesforce
Jim Reid, rtfmllp
Russ Housley, Vigil Security LLC
Paul Ebersman, Neustar
James Gruessing, BBC
Ralph Dolmans, NLnet Labs
Eric Rescorla, Mozilla
Vladimir Cunat, cz.nic
Chris Box, BT
Jon Reed, Akamai
Sam Weiler, W3C/MIT
Zaid AlBanna, Verisign
Jonathan Hammell, Canadian Centre for Cyber Security
Barry Leiba, FutureWei
Melinda Shore, Fastly
Martin Duke, F5 Networks
Kazunori Fujiwara,JPRS
Duane Wessels, Verisign
Richard Wilhelm, Verisign
Amelia Andersdotter, CENTR
Mallory Knodel, CDT
Godfred Ahuma, Packetfile
Ben Kaduk, Akamai
Warren Kumari, Google.
Alessandro Ghedini, Cloudflare
Willem Toorop, NLnet Labs
chi-jiun su, hughes network systems
James Gould, Verisign
Sara Dickinson, Sinodun
Simon Hicks, DCMS
Kaustubha Govind, Google Chrome
Alissa Cooper, Cisco
Dragana Damjanovic, Mozilla
Michael Gibbs, Verisign
Hugo Salgado, NIC Chile
John Border, Hughes
Tim Wicinski,
Suzanne Woolf
Ben Schwartz
Hitesh Kotian, Verisign
Robert Story, USC/ISI
