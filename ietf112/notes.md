# Privacy Enhancements and Assessments Research Group (PEARG) Agenda - IETF 112


## Presentations

* "State of the art on privacy-preserving techniques used at the network layer" - Antoine Fressancourt  (20 mins)
* (lots of things in the slides look at them)
* lots of consumer demand for privacy, IP should be obscured
* Various threat models
* Over top application layer vs in network
* Can avoid third parties but avoid destination based routing
* Proposal: have each AS work with an onion, use source routing
* Sphinx mixnet
* Conclusion: should look at SR to address privacy
* Q&A: lots of technologies, deployablity, different attack models, characterization

## Drafts

* [Privacy Improvements for DNS Resolution with Confidential Computing](https://datatracker.ietf.org/doc/html/draft-arkko-dns-confidential) - Jari Arko (20 mins)

https://datatracker.ietf.org/meeting/112/materials/slides-112-pearg-using-confidential-computing-to-protect-dns-resolution-00

Jari: DNS as an example, metadata is still mostly available on the wire, but expect it to be encrypted more often.
... but even then, resolvers can still collect the user's browsing history, which makes them tempting targets for attack
... RFC 8932 for practices for running a resolver, and Mozilla has their own requirements

Jari: what is the next step? we should protect user data in flight, at rest and in use.
... what if run the DNS resolver process inside a trusted execution environment and don't provide any user-specific data outside the encrypted process
... should complicate things for people who want to collect this data
... TEE: run code without tampering, and data cannot be accessed externally
... attestation: a node can confirm to external parties that it's running particular code with particular configuration, relying on trusted someone like the creator of the CPU

Jari: experiment had clients using DoH or DoT, with encryption terminated inside the TEE, and so resolution of the names stays inside the TEE
... you can't do DNS entirely on your own, sometimes you don't have something cached and need more info
... timing obfuscation as some protection against correlating those queries

Jari: it is possible to hide all user-related information and we SHOULD.
... could be an incentive for adoption that you can market to your users that you can show a certain level of privacy and not re-use of data
... could still collect aggregated data

Jari: downsides include impacts on operational practices, scaling, monitoring, debugging
... dependencies become more specific, requiring particular hardware
... performance varies

Jari: attacks remain, including CPU problems, but it's a pretty big hurdle for the attacker
Jari: active development ongoing on hardware, etc.

Jari: we should pay more attention to the data held by servers, reduce and protect that data. ready for some deployments. interesting, even if there's interesting problems remaining.

Ben Schwartz: oblivious RAM? 
Jari: we used Intel SGX
Ben: memory access patterns even if the contents are encrypted.

dkg: is this running somewhere where it can be queried/tested? replacement or complementary?
Jari: definitely complementary to other techniques. could be a hackathon project for testing, but not currently available for testing.

ekr: past enthusiasm for enclaves as MPC alternatives, but so much work on attacks makes me think current TEE technology is insufficient. threat model attacker includes an extremely sophisticated attacker since they are running the data center.
Jari: there clearly are attacks, but still think it's a barrier.
waiting for the next generation

* [WIP Draft update - IP Address Privacy update](https://github.com/ShivanKaul/draft-ip-address-privacy) - Matthew Finkel (15mins)

https://datatracker.ietf.org/meeting/112/materials/slides-112-pearg-ip-address-privacy-draft-01

Matt Finkel: draft on IP address privacy considerations, what we need to take into account as we deploy more IP privacy.
... interim earlier this year
... how IP addresses are used and why privacy of IP address is so difficult
... reputation a particularly important use case, replacement for reputation is especially ambitious

finkel: holistic view of IP address usage, categorize the use cases
... fruitful conversations with platforms and other providers
... use cases on anti-abuse and geo-ip
... privacy legislation in different jurisdictions

finkel: what blockers there are for services that are currently blocking users of IP address privacy tech
... following efforts at IETF and W3C

finkel: scope of the draft, question of mandatory signals

wood: interested in a call for adoption of this draft for pearg. please read and ask questions on the list.

* [Gap Analysis in Internet Addressing](https://datatracker.ietf.org/doc/draft-jia-intarea-internet-addressing-gap-analysis/) - Luigi Iannone (5 mins) *if time permits*
