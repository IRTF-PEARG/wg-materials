Pearg notes
=====
Administrivia (5 minutes)
==

Blue sheets / scribe selection / NOTE WELL
==
* Agenda Bash

# Draft updates (20 minutes)
* IP address privacy draft missed the cutoff, came from interim, pretty skeletal
* More content comming, throw some in. Particularly usecase. Lots in interim. If interested let chairs and authors know.

## Safe measurement update
Mallory Knodell
* A number of prior contributions, a few small changes
* Fills major need
* Goal of draft: describe for academia and industry guidelines on measurements that don't violate user privacy
* Interesting things around scope. Important. Strengthened in last version
* Not substitute for ethics review. Complements, not replaces
* Tries to define better what Internet measurement scope means. Interested in definition.
* Identifies user and who it is safe for.
* In three parts
* Consent
* Safety
    * Isolate risk with dedicated testbed
    * Respect other infrastructure
    * Data minimization
    * Masking
* Risk analysis
* Alas TOC not coming out of XML yet!
* Changes since -04 : disclosure issues
* Recent research, IP address
* Safety != Ethics?
* Since -05 nits
* Things still open in github
    * Responsible disclosure
    * Availability
    * Ip addresses
    * Future computing capability
    * Look at CADIA
    * Want to bring in learnings, add to table of contents.
    * Please open issues, better yet PRs
* Very hard to explain what consent would mean in lower layers
### QA:
* Stephen (from chat): two things on consent 1) be good to include examples of when that was handled well and when badly (or controversially) and 2) I think this document might end up being a model for other IETF docs that mention consent so it should be done carefully
* Mallory: Good idea on adding examples, depends on carefully. User consent shouldn't give free reign. Not clear this goes beyond what this context. Internet traffic is different.

## IP address privacy update
# New Work / Presentations (1hr 35 mins)
## Website Fingerprinting in the Age of QUIC - Jean-Pierre Smith (20 mins)
* From PETS two weeks ago
* In early days if adversary wanted to see content could use Wireshark
* Now things are encrypted. But certs, SNI reveal what user is viewing.
* Users use PETs like Tor and VPNs
* Most information no longer there
* In ideal world end of story
* But... packet sizes, timings, directions.
* Creates fingerprinting
* Attacker sees features from websites, constructs model
* Usually interested in particular set of pages vs. not
* Quite a lot of work on Tor and websites
* Mostly TCP focused
* QUIC has multiple streams, vs. one flow per connection
* Traces can mix TCP and QUIC for the same navigation
* Collect large dataset by scanning webpages
* 100 pages are the target, 16000 the rest
* Number of classifiers trained, many deep learning. Identify webpage!
* Identify recall and precision
* TCP trained identifying QUIC?
* Doesn't work: TCP trained worked well on TCP, not QUIC
* Now train on QUIC and test on QUIC
* They work just as well QUIC-QUIC and TCP-TCP
* Some classifiers a bit better on QUIC than on TCP
* Could be due to QUIC server variability being limited, so variation across web pages different. Reduced middlebox interference?
* Mixed classification/Split ensamble
* Mixed: do both at once
* Split: detect QUIC or TCP, send to dedicated classifier for case
* Mixed: slight decrease in performance
* Split: Very simple trace distinguishing, 99% accuracy
* Due to handshake differences
* QUIC initial client hello quite large vs. SYN-ACK handshake
* Real easy!
* Ensamble: QUIC vs TCP provides weight to predictions of TCP and QUIC
* Not as good as Mixed for same sample budget
* Conclusions: QUIC not more difficult than TCP, some problems when TCP only classifier
* Joint possible, cost to adversary
### QA
* Antoine: How large is training vs test? Is there drift over time with pages, or is all the data gathered at once?
* A: Split was 90% training to 10% holdout.
* A: Didn't evaluate drift. Has been evaluated before by a number of people. Realistic Website Fingerprinting. Running collection of the webpages could maintain high precision and recall. Quite a number of recent works. Triplet fingerprinting: use 10 new samples to get it back up to high precision despite very old samples.
* Q: any other research in area?
* A: Four or five other groups doing it, expect more stuff coming out over the months and year

## ShorTor: Improving Tor Network Latency through Multi-Hop Overlay Routing - Kyle Hogan (20 mins)
* Presented last meeting, excited to be back
* Work in progress
* Questions can be answered as going
* Short Tor is an overlay that reduces latency between relays by making better routing
* Design, Evaluation, Integration, Security
* We don't change the client
* Interest in feedback
* Overlay routing: sometimes fastest path between A to B goes through a server C.
* Tor already has lots of forwarding
* Can we take advantage of it?
* Go via another relay if faster
* To evaluate need latencies between things in Tor
* Ethics interlude: we've worked with Tor, let operators opt out, our relays are pretty restrictive, Don't record any connection we didn't make ourselves.
* Q: Since whole path unkown, per hop? 
* A: right now additional control plane to indicate that this route should go Via.
* Only thing skipped is Onion encryption. Queuing not skipped.
* Currently focus on top consensus weights
* Top 125K relays
* Big, enduring, most circuits, stay up
* Challenging to get small relays in all-pairs dataset
* Future: all*
* Churn will complicate: relay stops existing midway through
* Graph gets presented!
* Simulated circuits through selection (limited to measured relays) see what the latency would be one way or the other
* Some ridiculous round trip delays
* High round trip times seem to be low BW relays, but not selected
* Circuit selection is unchanged. Via looks at latency. No via should ever accept traffic in excess of BW
* Using scheduling to make via lower priority then direct.
* If via slows down, stop using. Doesn't impact circuit.
* Integration: as Tor relays adopt can be used.
* Get good speedups even with small number supporting.
* Can get 1500ms speedups sometimes, with just a few hundred big relays supporting
* MATors framework and network traffic share based security analysis
* All circuit selections supported, including AS diversity
* Next steps: finish analysis: only have 1 M, not 50 M pairs
* Finish security analysis with representative dataset
* Dataset touchy subject
* Tor latency find exit relay that was chosen
* Client will never see it, relays decide
* Don't want clients to change behavior
* Q: Watson: Sounds pretty intrusive
* A: Yeah, need two new fields so intermediate will be able to learn how to forward, next relay will use previous relay instead of connection to disambiguate circuit ID
* Q: Antonine: does latency deanonymize?
* A: Yes, possibly: fast=short, so selecting relays on speed means close relays. We have less correlation because circuit is the same. But reducing latency closer to geodistance

## Private Relay - Tommy Pauly (20 mins)
* Given a lot of talk about IP privacy, let's look at a deployment.
* Love to hear feedback on how this could evolve
* Private relay several pieces of IETF
* Separate IP addresses from origin servers accessed
* Not full Tor threat model, seemed very common linkage used by many parties for tracking and hurting privacy
* MASQUE, Oblivious DoH
* QUIC TLS 1.3 to connect to proxies
* To access using RSA blind signatures
* scope iOS15 and Mac OS Monterray (both beta)
    * All Safari browsing
    * All DNS traffic
    * All unencrypted HTTP traffic
    * Highest vuln traffic without all of it
    * Underlying tech to protect against pixel trackers in mail
* Privacy goals
    * No entity can connect who you are and what you are looking at
    * Performance good enough for generic web browsing
    * Left on, not flipped on and off
* Two hop minimum!
    * Ingress and Egress proxy siting between access network and origin
    * Ingress forwards encryped connection to Egress
    * Operated by differen entities
* Clients control which, nested encrypion
* Gets manifest with hops and how to combine
* In order to track would need collusion. Policy enforced contractually
* Q: Jonathan: (more of a comment) Global passive adversary can identify through both hops. Impossible to prevent
* A: Yup
* Privacy not slow
* Aggressively use QUIC and MASQUE features to accelerate. Lots has to do with deployment and routing and global coverage
* Lots of fast open. Proxying at stream level
* If talking to normal TLS/TCP origin forwarding QUIC dgram through ingress that is request to egress+TLS client hello. Egress does the rest, without waiting
* Fast open, QUIC on last mile regardless of server IPv6 everywhere, Web on par, sometimes faster
* No v4 recapsulate in middle
* Break as little as possible
    * No impact on local routes
    * Failover for private hostnames and address
    * Off if over VPN or proxy is being used
* Rough GeoIP preserved
* Hint to egress to use particular geolocationd data
* Long term need to move away from this use of IP addresses.
* More standards on geolocation and how that's shared and fraud prevention with IP privacy
* Future
    * Expand MASQUE
    * Open interoperable network
    * Ingress into carrier networks
    * Egress within content providers
    * Client selection of policies, route selection
### Questions:
* Victor: Authentication of origin still end to end?
* A: yes. TCP proxied, TLS not, QUIC origins fully.
* Q: Stephen:- how would you characterise the longer term complexity trade-offs between this approach and trying to eventually move to something simpler and more generic but harder to get deployed like "all over Tor" or an equivalent?
* A: Would like to see consensus around deployment. MASQUE proxies good start. Tor compat interesting
* Q: Matthew: DNS for key management? Hard coded relays?
* A: Right now public keys are all coming from iCloud control plane. Short term decision for this feature. Long term more open and discoverable and extensible.
* Q: Andrew: Using the CFRG draft for RSA?
* A: yes, why we want that draft
## FLoC - Josh Karlin (remaining time)
* Tech Team lead on Privacy Sandbox
* Trial, chewing on feedback, thinking on next
* Web partioning on top level track
* Lots of companies observe browsing. Would like to stop it
* First build walls: break third party cookies. Same origin policy
* Partition everything...
* Important use cases: SSO
* Personalized avertising fraud, logouts with federated login
* Lots of work here to provide them
* FLOC focused on interest based advertising
* Target array of interests not just context on page
* Goals to support interest adds, hard to track individuals
* Today script runs on broswers
* Backend takes contextual queues to user, sends profile back
* With FLOC ad-tech given cohort of similarity, predictive models find ads
* History as group, adtech backend few changes
* API rejects for reasons: sensitive cohort, incognito, history cleared
* Cohort: client side
* No new data.
* Only part used domains; no path or contents
* Thousands of users, no sensitive info, no fingerprinting service
* Encode user history by taking domains, hash into 64 bits. Sparse vector
* Random projection onto 50 dimensional space
* Apply grouping from Chrome server to 16 bits capturing thousands in each group
* Pages eligable: only pages using API, only without private IP and not oped out
* Origin trial concerned not representative for early adopters
* Now used sites with ads on them
* k-anonymous: 2,000 chrome sync users per cohort
* Prevent transmission of cohorts with sensitive sites in them via revoking if correlated
* Dropped 4% of cohorts
* Origin trial: Page and user opt outs, bunch of other things in slides
* Feedback:
* got lots
* Especially Mozilla and privacy analysis
* Improvements
    * No auto-opt in: Done
    * Cohorts hard to understand for users: use topics to make clear what is revealed
    * Topics would be curated, have curated lists
    * Users can understand what they are indicating
    * Users opt in or out
    * Ad Topics Hint also related
    * New fingerprinting surface
    * Reduce it? Can we use 8 bits? Privacy sandbox tackles all the tracking
    * Random topics with some probability?
    * Give sites different topics?
    * Taken together can drop cross-site fingerprinting issues
* Sensitivity
    * Human curation and t-topic analysis
* Scope
    * Right now global browsing history
    * Per third party topic based on where third party is
    * 100% subsetting
    * Disadvantages if multiple parties can work on once
* Q: What happens when off?
* A: Nothing vs random we're thinking about. Training easier if we understand cohorts have meaning. 5% random right now. Cohort fairly high
* Q watson: User justification for participation
* A: Sites get money, particularly tail sites. Big money from personalization
* Q followup: Need to see data, open and free doesn't mean making money from slimy people
* Q: Matthew: Recomputation of map?
* A: Yes. Has to be done regularly as web changes. Sensitivities change. Full intention of sharing
* Q: Wes: Do not track vs. this?
* A: Not sure question follows. What we offer is privacy sandbox. Still offering third party blocking and customization. Users still get to set up the barriers.
