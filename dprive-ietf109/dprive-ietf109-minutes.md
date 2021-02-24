# DNS Privacy Exchange (DPRIVE) WG
IETF109
Date: Friday 20 Nov 2020
Time: 0900-1100 UTC
MeetEcho: http://www.meetecho.com/ietf109/dprive/
Minutes: https://codimd.ietf.org/notes-ietf-109-dprive

Chairs
* Tim Wicinski tjw.ietf@gmail.com
* Brian Haberman brian@innovationslab.net

Responsible Area Director
* Eric Vyncke evyncke@cisco.com

DataTracker
https://datatracker.ietf.org/group/dprive/documents/

## Administrivia
* Slides: https://datatracker.ietf.org/meeting/109/materials/slides-109-dprive-dprive-chair-slides-00
* Agenda Updates, etc, 10 min
    * Paul Hoffman: Hopes that last section about Phase 2 will talk about larger issues that have been discussed on the list as well
* NOTE WELL : https://www.ietf.org/about/note-well.html

## Current Working Group Business

### DNS Zone Transfer-over-TLS

https://datatracker.ietf.org/doc/draft-ietf-dprive-xfr-over-tls/
Slides: https://datatracker.ietf.org/meeting/109/materials/slides-109-dprive-zone-transfer-over-dot-00
GitHub repo: https://github.com/hanzhang0116/hzpa-dprive-xfr-over-tls
Presenter: Sara Dickinson
Chairs Action: closer to WGLC?

* Sara requests that people indicate in the chat if they have read this -03 version by typing "+1"
* Sara ends with a request for reviews.

* Alex Mayrhofer - how do you want reviews?
    * Sara - open to either email or GitHub issues. Slight preference for email.
    * Brian - Is the GitHub repo set up to reflect back to the list?
    * Tim - No, it's not. Please send all big issues to the email list. Editorial comments to GitHub are fine.
* Puneet Sood - Should we say that XFR should be on a different port?
    * Sara - Does not see that as necessary at this time.
* Tim - Please send feedback to list. Sees this as close to WGLC but would like final comments.
    * Jim Reid - are you going to set a deadline for review comments?
    * Tim - Yes. We should be looking at WGLC by the end of the year.

### DNS over QUIC

https://datatracker.ietf.org/doc/draft-ietf-dprive-dnsoquic/
Slides: https://datatracker.ietf.org/meeting/109/materials/slides-109-dprive-dns-over-quic-update-00
Presenter: Christian Huitema
Chairs Action: ?

* Christian notes that the QUIC protocol is now in WGLC and on the path to publishing. With that, this draft becomes more relevant. The draft has not changed in some time. He would like feedback from the group as to whether there is interest in this work.
* Christian asks:
    * How much interest is there for DoQ from stub to recursive?
        * Do we know of prototype deployments?
    * Should we study DoQ for recursive to authoritative?

* Peter van Dijk - You say that several implementations are available, but there is no "Implementation" section in the draft. Info should be added.
    * Christian - Thanks. We'll add that.

* Ben Schwartz - Agrees that DoQ is interesting for recursive to authoritative. He thinks this draft is worth pursuing.

* Daniel Kahn Gillmor - Can you speak about your server resources claim? Do you have data showing fewer resources for tracking DoQ state vs DoT state?
    * Christian - Doesn't have specific measurement for DoQ. Measurement on H3 is that he can manage 50,000 connections on a single server. Overhead of handliing connections is not very high.

* Paul Hoffman - answering questions in chat about whether this is raw DNS over QUIC or DNS over HTTP/3. This work is intended to be focused on raw DNS over QUIC, and he hopes it stays that way.

* James Gressing - Need a public benchmark for doing DoQ versus DNS over HTTP/3 to understand if this draft should be expanded.
    * Christian - Agrees with Paul Hoffman that introducing HTTP/3 adds an unnecessary layer.

* Wes Hardaker - Is torn because he can see DoQ giving benefits, but notes that IETF has historically had challenges deploying protocols over multiple transports. We are at the point now to have DNS over 3 secure transports (DoH, DoT, DoQ) without a clear reason to pick them. Concerned that deployment will wind up seeing one of these paths dominating, and the others fading away. Wants to have DoQ on the table, but concerned about the complexity of DNS implementations, and wasting time of implementors on developing code that ultimately doesn't get used.

* Ben Schwartz - Would like a little more thought in the draft about the AXFR use case.
    * Christian - That was out of scope when we started this draft.
    * Ben - With DPRIVE recharter, this is now in scope.
* Ben - Thinks there are use cases where DoQ will not suffice and where DoH3 will be needed. Gives use case of NextDNS.

* Christian to chairs - what next?  He's heard two suggestions:
    * Add Implementation section
    * Add AFXR use case
* Brian - encourages adding implementation, and then waiting on AXFR for discussion through Phase 2 requirements.


### Phase 2 Requirements

https://datatracker.ietf.org/doc/draft-ietf-dprive-phase2-requirements/
Slides: https://datatracker.ietf.org/meeting/109/materials/slides-109-dprive-phase-2-requirements-discussion-slides-00
Working Group, time remaining
Chairs Action: Facilitate new edits

* Tim - There has been a great amount of discussion in the mailing list, particularly around opportunistic connections. Is there any discussion on that?
    * Paul Hoffman - Does not see opportunistic as being in competition with other approaches.
    * Peter van Dijk - Agrees with Paul that there is no competition. We should pursue both approaches.
    * Sara Dickinson - Is it written down anywhere what we mean by whole authentication?
        * Tim - I don't think it's written down and that may be an issue.
        * Paul Hoffman - No, it's not. It's based on assumptions.
        * Pete van Dijk - the bin draft ...  In the context of the TLSA drafts, Sara's question is very valid.
    * dkg - Three use cases - Do we want to cover them all?
        * Probing opportunistic transport
        * Signaled opportunistic transport
        * Signal that server should expect encrypted transport (DNS equivalent of STS) "Signaled strict mode"
    * dkg - other axes that we need to consider:
        * signalling mechanism (how is it announced?)
        * signalling scope (e.g. signal applies to zone, to named NS, to NS IP)
    * Sara - Agrees with DKG on use cases. Ideally would like to only deal with two, but acknowledges there may be more. Notes that servers may need to signal back to clients with error codes.
    * Ben - Thinks there are inherently MANY different and complex conditions here. Multiple axes. Suggests we charge ahead with prototyping.


* Brian - ordered discussion of requirements based on his view of what he was seeiing on the mailing lists.
* Suggests we have an interim in January and February to progress this work.

#### Requirement 7
>The authoritative domain owner or their administrator MUST have the option to specify their secure transport preferences (e.g. what specific protocols are supported). This SHALL include a method to publish a list of secure transport protocols (e.g. DoH, DoT and other future protocols not yet developed). In addition this SHALL include whether a secure transport protocol MUST always be used (non-downgradable) or whether a secure transport protocol MAY be used on an opportunistic (not strict) basis in recognition that some servers for a domain might use a secure transport protocol and others might not.

* Ben Schwartz - He has a draft proposal (and there are others) on "Service Binding for DNS" in ADD WG related to this. His draft is at:
    * https://tools.ietf.org/html/draft-schwartz-svcb-dns-01
* Paul Hoffman - Making servers demand authentication seems over-the-top. Would like the latter part to be more of a MIGHT versus a hard requirement.
* Peter van Dijk - Concerned that there is a separation issue between DPRIVE and ADD. Is not following the work there, and was not aware this was an area of work.
* dkg - Responding to Paul that strict mode is available in other protocols. Mentions HSTS, and MTA-STS. "I guarantee you secure transport is available. You should use it. If you can see this message, you must connect overs secure transport."
* Ralf Weber - DNS is more complex than HTTP. Harder for components to determine what to do. Often more important for an operator to give an answer than to not give an answer.
* Peter Koch (in chat) - on req 7: instead of saying which entity should be able to specify (the 'owner' or 'holder' is more a registry side concept) it might make sense to make this a property of either the zone or the authoritative server (likely the former) 
* Duane Wessels - Wants to be clear we are talking about *zones* signaling their preferences versus *servers* signaling their preferences.
* Ben Schwartz - Thinks many of these requirements are too detailed and too strict and is not sure he'd want to implement them. For example doesn't think that signaling a non-strict connection in in Requirement 7 is important. Also gives example from Requirement 9.
* dkg - Agrees with Ben that these requirements seem a bit strange. Again points to requirement 9 and asks why we care? Asks about requirement 7 and signaling. He sees multiple mechanisms. Have we collectively settled on a specific mechanism?
* Alex Mayrhofer - Agree to what Daniel said, it's important to make a designation of whether the focus is per-zone, per-nameserver-name, per-ip-address, and the existing requirements are a vague interpretation that it's not per nameserver.
* Peter van Dijk - Requirement 9 is a mistake. It should just go. As to signaling mechanism in Req 7, not sure we can discuss that today.
* Sara - 
* Tim - Asks about dkg noting he wants 2 cases
* dkg - Sees now two cases:
    * Opportunistic with probing
    * Strict mode
* dkg - Signaling is hard and there is a good bit of work to do. Thinks we will need signaling. But for right now should focus on probing and strict.
* Ben - Would like to see an experimental draft that explains in great detail opportunistic by probing on port 853.
* Paul - Thinks people are using "opportunistic" incorrectly. You try to authenticate, but then continue if you don't. Can't imagine that probing by port number is something people are going to want to do. Thinks TLSA records may work better. Thinks that transport paths are most important for both opportunistic and strict. Going back to Sara's question earlier, what do people who fail to authenticate do? We need detail about use cases and how things will fail.
* Alex Mayhofer - What if last authenticated server fails? Does resolver go back to unencrypted server? Do we identify dimensions? Do a hackathon to identify issues? Do both?



* Brian notes that Req 2 and Req 3 directly relate to whether we are scoping this "per zone" or "per server". Our decision will impact those.
* Tim - yes, we'll need to make this decision.

* Paul Hoffman - Draft authors started a year ago with very different assumptions. Let the authors take a look at what has been discussed and adjust the draft.
* Sara - Notes that several of the solutions have a requirement on DNSSEC. We need to be care to be sure that we want that coupling. That's another requirement that may feedback on here.
* Peter van Dijk - Agrees with Sara that we need to be careful about coupling.
* Wes Hardaker - We have one example of this working well. Start with RFC7672 - strict secure SMTP falling back to opportunistic secure SMTP falling back to insecure SMTP.
* Paul Hoffman - Notes that in those secure SMTP solutions, many SMTP servers are getting certs from CAs versus DNS. May be something considered here for secure DNS.
* Ben Schwartz - Agrees with Paul that we should be open-minded about different roots of trust. There are people in this ecosystem who won't touch CAs with a 10-foot pole, while others won't touch DNSSEC with a 10-foot pole. We need to get them all working together. Authentication is only interesting if you will terminate the connection and not deliver the query IF the authentication fails.
* Paul Hoffman - Has now been convinced by mailing list conversation to be more open about authentication scenarios.

* Brian - It seems that we have a set of requirements in the document that are obsolete or have gone by the wayside.  Shall we instead focus on how to look at a different set of requirements?
* Tim - As we hear all of this, I think we do need to step back and rethink what we are doing between recursive to authoritative?

* Puneet Sood - Whether the transport secure is really a server problem. Strict is a property of the zone. We don't have much secure transport at all right now. To jump to all strict is premature.
* Ben Schwartz - If the server signals it has a reliable connection, if the resolver receives insecure info, it should think of it as an attack. 
* Peter van Dijk - (agrees with Ben)
* Puneet - Scalability is an issue.

* Ray Bellis (in chat) - per-server flags advertise capability, per-zone flags indicate policy

* Tim - are people willing to lead some work on scenarios?
* Paul Hoffman - has provided some drafts to the list. Very open to seeing other people submit other drafts.

* Alex Mayrhofer - clarified that when he said "on paper", he didn't mean we shouldn't write code, but rather that some scenarios could be figured out in text.


* Sara - Different topic. Threat model in requirements has two cases. Third case is zone owners who are interested in privacy and trying to defeat enumeration. They may also be interested in encryption to prevent passive monitoring.
    * Paul Hoffman (in chat) -  same on the resolver side. Some resolver ops would be happy to not have passive observers "enumerate" what their clients are doing.

* Brian - Timing will be tight before IETF 110 in the beginning of March, but there should be room for interim meetings.

Meeting finished.
