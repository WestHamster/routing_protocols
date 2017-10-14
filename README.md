# routing_protocols
Routing Protocols
ROUTING PROTOCOL


ROUTING PROTOCOLS




STATIC				DYNAMIC			DEFAULT



DISTANCE VECTOR       LINK-STATE		HYBRID



RIP			IGRP	     OSPF			EIGRP
STATIC:
Static routing has several primary uses, including:
•	Providing ease of routing table maintenance in smaller networks that are not expected to grow significantly.
•	Routing to and from a stub network, which is a network with only one default route out and no knowledge of any remote networks.
•	Accessing a single default route (which is used to represent a path to any network that does not have a more specific match with another route in the routing table).
Static routing is easy to implement in a small network. Static routes stay the same, which makes them fairly easy to troubleshoot. Static routes do not send update messages and, therefore, require very little overhead.
The disadvantages of static routing include:
•	They are not easy to implement in a large network.
•	Managing the static configurations can become time consuming.
•	If a link fails, a static route cannot reroute traffic.
Advantages	Disadvantages
Easy to implement in a small network.	Suitable for simple topologies or for special purposes such as a default static route.
Very secure. No advertisements are sent, unlike with dynamic routing protocols.	Configuration complexity increases dramatically as the network grows. Managing the static configurations in large networks can become time consuming.
It is very predictable, as the route to the destination is always the same.	If a link fails, a static route cannot reroute traffic. Therefore, manual intervention is required to re-route traffic.
No routing algorithm or update mechanisms are required. Therefore, extra resources (CPU and memory) are not required.	




 
 
 
 



DEFAULT:

In computer networking, the default route is a setting on a computer that defines the packet forwarding rule to use when no specific route can be determined for a given Internet Protocol (IP) destination address. All packets for destinations not established in the routing table are sent via the default route.
The default route generally points to another router, which treats the packet the same way: if a route matches, the packet is forwarded accordingly, otherwise the packet is forwarded to the default route of that router. The route evaluation process in each router uses the longest prefix match method to obtain the most specific route. The network with the longest subnet mask that matches the destination IP address is the next-hop network gateway. The process repeats until a packet is delivered to the destination. Each router traversal counts as one hop in the distance calculation for the transmission path.
The device to which the default route points is often called the default gateway, and it often carries out other functions such as packet filtering, firewalling, or proxy server operations.
The default route in Internet Protocol Version 4 (IPv4) is designated as the zero-address 0.0.0.0/0 in CIDR notation,[1] often called the quad-zero route. The subnet mask is given as /0, which effectively specifies all networks, and is the shortest match possible. A route lookup that does not match any other route, falls back to this route. Similarly, in IPv6, the default route is specified by ::/0.
In the highest-level segment of a network, administrators generally point the default route for a given host towards the router that has a connection to a network service provider. Therefore, packets with destinations outside the organization's local area network, typically destinations on the Internet or a wide area network, are forwarded to the router with the connection to that provider.

A default route identifies the gateway IP address to which the ASA sends all IP packets for which it does not have a learned or static route. A default static route is simply a static route with 0.0.0.0/0 as the destination IP address. Routes that identify a specific destination take precedence over the default route.
You can define up to three equal cost default route entries per device. Defining more than one equal cost default route entry causes the traffic sent to the default route to be distributed among the specified gateways. When defining more than one default route, you must specify the same interface for each entry.
If you attempt to define more than three equal cost default routes, or if you attempt to define a default route with a different interface than a previously defined default route, you receive the following message:
"ERROR: Cannot add route entry, possible conflict with existing routes." 
You can define a separate default route for tunneled traffic along with the standard default route. When you create a default route with the tunneled option, all traffic from a tunnel terminating on the ASA that cannot be routed using learned or static routes, is sent to this route. For traffic emerging from a tunnel, this route overrides over any other configured or learned default routes.

Limitations on Configuring a Default Static Route
The following restrictions apply to default routes with the tunneled option:
• Do not enable unicast RPF (ip verify reverse-path) on the egress interface of tunneled route. Enabling Unicast RPF on the egress interface of a tunneled route causes the session to fail.
• Do not enable TCP intercept on the egress interface of the tunneled route. Doing so causes the session to fail.
• Do not use the VoIP inspection engines (CTIQBE, H.323, GTP, MGCP, RTSP, SIP, SKINNY), the DNS inspect engine, or the DCE RPC inspection engine with tunneled routes. These inspection engines ignore the tunneled route.
You cannot define more than one default route with the tunneled option; ECMP for tunneled traffic is not supported.

 

 

 

 


DYNAMIC:
1)	DISTANCE VECTOR:
 Each node constructs a one-dimensional array containing the "distances"(costs) to all other nodes and distributes that vector to its immediate neighbors.
1.	The starting assumption for distance-vector routing is that each node knows the cost of the link to each of its directly connected neighbors.
2.	A link that is down is assigned an infinite cost.
Example.
 
Information
Stored at Node	Distance to Reach Node
	A	B	C	D	E	F	G
A	1	1	0	1	�	�	�
D	�	�	1	0	�	�	1
E	1	�	�	�	0	�	�
F	1	�	�	�	�	0	1
G	�	�	�	1	�	1	0
Table 1. Initial distances stored at each node(global view).
�UNKNOWN
We can represent each node's knowledge about the distances to all other nodes as a table like the one given in Table 1.
Note that each node only knows the information in one row of the table.
1.	Every node sends a message to its directly connected neighbors containing its personal list of distance. ( for example, A sends its information to its neighbors B,C,E, and F. )
2.	If any of the recipients of the information from A find that A is advertising a path shorter than the one they currently know about, they update their list to give the new path length and note that they should send packets for that destination through A. ( node B learns from A that node E can be reached at a cost of 1; B also knows it can reach A at a cost of 1, so it adds these to get the cost of reaching E by means of A. B records that it can reach E at a cost of 2 by going through A.)
3.	After every node has exchanged a few updates with its directly connected neighbors, all nodes will know the least-cost path to all the other nodes.
4.	In addition to updating their list of distances when they receive updates, the nodes need to keep track of which node told them about the path that they used to calculate the cost, so that they can create their forwarding table. ( for example, B knows that it was A who said " I can reach E in one hop" and so B puts an entry in its table that says " To reach E, use the link to A.)
Information
Stored at Node	Distance to Reach Node
	A	B	C	D	E	F	G
A	0	1	1	2	1	1	2
B	1	0	1	2	2	2	3
C	1	1	0	1	2	2	2
D	2	2	1	0	3	2	1
E	1	2	2	3	0	2	3
F	1	2	2	2	2	0	1
G	2	3	2	1	3	1	0


Each router maintains a routing table. The routing table R can be modelled as a data structure that stores, for each known destination address d, the following attributes :
•	R[d].link is the outgoing link that the router uses to forward packets towards destination d
•	R[d].cost is the sum of the metrics of the links that compose the shortest path to reach destination d
•	R[d].time is the timestamp of the last distance vector containing destination d
A router that uses distance vector routing regularly sends its distance vector over all its interfaces.

RIP:
The Routing Information Protocol (RIP) defines a way for routers, which connect networks using the Internet Protocol (IP), to share information about how to route traffic among networks. RIP is classified by the Internet Engineering Task Force (IETF) as an Interior Gateway Protocol (IGP), one of several protocols for routers moving traffic around within a larger autonomous system network -- e.g., a single enterprise's network that may be comprised of many separate local area networks (LANs) linked through routers.
Although once the most widely used IGP, Open Shortest Path First (OSPF) routing has largely replaced RIP in corporate networks.
Each RIP router maintains a routing table, which is a list of all the destinations (networks) it knows how to reach, along with the distance to that destination. RIP uses a distance vector algorithm to decide which path to put a packet on to get to its destination. It stores in its routing table the distance for each network it knows how to reach, along with the address of the "next hop" router -- another router that is on one of the same networks -- through which a packet has to travel to get to that destination. If it receives an update on a route, and the new path is shorter, it will update its table entry with the length and next-hop address of the shorter path; if the new path is longer, it will wait through a "hold-down" period to see if later updates reflect the higher value as well, and only update the table entry if the new, longer path is stable.
Using RIP, each router sends its entire routing table to its closest neighbors every 30 seconds. (The neighbors are the other routers to which this router is connected directly -- that is, the other routers on the same network segments this router is on.) The neighbors in turn will pass the information on to their nearest neighbors, and so on, until all RIP hosts within the network have the same knowledge of routing paths, a state known as convergence.

If a router crashes or a network connection is severed, the network discovers this because that router stops sending updates to its neighbors, or stops sending and receiving updates along the severed connection. If a given route in the routing table isn't updated across six successive update cycles (that is, for 180 seconds) a RIP router will drop that route, letting the rest of the network know via its own updates about the problem and begin the process of reconverging on a new network topology.
RIP uses a modified hop count as a way to determine network distance. (Modifiedreflects the fact that network engineers can assign paths a higher cost.) By default, if a router's neighbor owns a destination network (i.e., if it can deliver packets directly to the destination network without using any other routers), that route has one hop, described as a cost of 1. RIP allows only 15 hops in a path. If a packet can't reach a destination in 15 hops, the destination is considered unreachable. Paths can be assigned a higher cost (as if they involved extra hops) if the enterprise wants to limit or discourage their use. For example, a satellite backup link might be assigned a cost of 10 to force traffic follow other routes when available.
RIP has been supplanted mainly due to its simplicity and its inability to scale to very large and complex networks. Other routing protocols push less information of their own onto the network, while RIP pushes its whole routing table every 30 seconds. As a result, other protocols can converge more quickly, use more sophisticated routing algorithms, include latency, packet loss, actual monetary cost and other link characteristics, as well as hop count with arbitrary weighting.     
 

IGRP:
 It has two purposes. One is to form an introduction to the IGRP technology, for those who are interested in using, evaluating, and possibly implementing it. The other is to give wider exposure to some interesting ideas and concepts that are embodied in IGRP. Refer to Configuring IGRP, The Cisco IGRP Implementation and IGRP Commands for information on how to configure IGRP.
The IGRP protocol allows a number of gateways to coordinate their routing. Its goals are the following:
•	Stable routing even in very large or complex networks. No routing loops should occur, even as transients.
•	Fast response to changes in network topology.
•	Low overhead. That is, IGRP itself should not use more bandwidth than what is actually needed for its task.
•	Splitting traffic among several parallel routes when they are of roughly equal desirability.
•	Taking into account error rates and level of traffic on different paths.
The current implementation of IGRP handles routing for TCP/IP. However, the basic design is intended to be able to handle a variety of protocols.
No one tool is going to solve all routing problems. Conventionally the routing problem is broken into several pieces. Protocols such as IGRP are called "internal gateway protocols" (IGPs). They are intended for use within a single set of networks, either under a single management or closely coordinated managements. Such sets of networks are connected by "external gateway protocols" (EGPs). An IGP is designed to keep track of a good deal of detail about network topology. Priority in designing an IGP is placed on producing optimal routes and responding quickly to changes. An EGP is intended to protect one system of networks against errors or intentional misrepresentation by other systems, BGP is one such Exterior gateway protocol.. Priority in designing an EGP is on stability and administrative controls. Often it is sufficient for an EGP to produce a reasonable route, rather than the optimal route.
IGRP has some similarities to older protocols such as Xerox's Routing Information Protocol, Berkeley's RIP, and Dave Mills' Hello. It differs from these protocols primarily in being designed for larger and more complex networks. See the Comparison with RIP section for a more detailed comparison with RIP, which is the most widely used of the older generation of protocols.
Like these older protocols, IGRP is a distance vector protocol. In such a protocol, gateways exchange routing information only with adjacent gateways. This routing information contains a summary of information about the rest of the network. It can be shown mathematically that all of the gateways taken together are solving an optimization problem by what amounts to a distributed algorithm. Each gateway only needs to solve part of the problem, and it only has to receive a portion of the total data.
The major alternative to IGRP is Enhanced IGRP (EIGRP) and a class of algorithms referred to as SPF (shortest- path first). OSPF uses this concept. To learn more about OSPF refer to OSPF Design Guide. OSPF These are is based on a flooding technique, where every gateway is kept up to date about the status of every interface on every other gateway. Each gateway independently solves the optimization problem from its point of view using data for the entire network. There are advantages to each approach. In some circumstances SPF may be able to respond to changes more quickly. In order to prevent routing loops, IGRP has to ignore new data for a few minutes after certain kinds of changes. Because SPF has information directly from each gateway, it is able to avoid these routing loops. Thus it can act on new information immediately. However, SPF has to deal with substantially more data than IGRP, both in internal data structures and in messages between gateways.

As mentioned above, IGRP is a protocol that allows gateways to build up their routing table by exchanging information with other gateways. A gateway starts out with entries for all of the networks that are directly connected to it. It gets information about other networks by exchanging routing updates with adjacent gateways. In the simplest case, the gateway will find one path that represents the best way to get to each network. A path is characterized by the next gateway to which packets should be sent, the network interface that should be used, and metric information. Metric information is a set of numbers that characterize how good the path is. This allows the gateway to compare paths that it has heard from various gateways and decide which one to use. There are often cases where it makes sense to split traffic between two or more paths. IGRP will do this whenever two or more paths are equally good. The user can also configure it to split traffic when paths are almost equally good. In this case more traffic will be sent along the path with the better metric. The intent is that traffic can be split between a 9600 bps line and a 19200 BPS line, and the 19200 line will get roughly twice as much traffic as the 9600 BPS line.
The metrics used by IGRP include the following:
•	Topological delay time
•	Bandwidth of the narrowest bandwidth segment of the path
•	Channel occupancy of the path
•	Reliability of the path
Topological delay time is the amount of time it would take to get to the destination along that path, assuming an unloaded network. Of course there is additional delay when the network is loaded. However, load is accounted for by using the channel occupancy figure, not by attempting to measure actual delays. The path bandwidth is simply the bandwidth in bits per second of the slowest link in the path. Channel occupancy indicates how much of that bandwidth is currently in use. It is measured, and will change with load. Reliability indicates the current error rate. It is the fraction of packets that arrive at the destination undamaged. It is measured.
Although they are not used as part of the metric, two addition pieces of information are passed with it: hop count and MTU. The hop count is simply the number of gateways that a packet will have to go through to get to the destination. MTU is the maximum packet size that can be sent along the entire path without fragmentation. (That is, it is the minimum of the MTUs of all the networks involved in the path.)
Based on the metric information, a single "composite metric" is calculated for the path. The composite metric combines the effect of the various metric components into a single number representing the "goodness" of that path. It is the composite metric that is actually used to decide on the best path.
Periodically each gateway broadcasts its entire routing table (with some censoring because of the split horizon rule) to all adjacent gateways. When a gateway gets this broadcast from another gateway, it compares the table with its existing table. Any new destinations and paths are added to the gateway's routing table. Paths in the broadcast are compared with existing paths. If a new path is better, it may replace the existing one. Information in the broadcast is also used to update channel occupancy and other information about existing paths. This general procedure is similar to that used by all distance vector protocols. It is referred to in the mathematical literature as the Bellman-Ford algorithm. Refer to RFC 1058   for a detailed development of the basic procedure, which describes RIP, an older distance vector protocol.
In IGRP, the general Bellman-Ford algorithm is modified in three critical aspects. First, instead of a simple metric, a vector of metrics is used to characterize paths. Second, instead of picking a single path with the smallest metric, traffic is split among several paths, whose metrics fall into a specified range. Third, several features are introduced to provide stability in situations where the topology is changing.
The best path is selected based on a composite metric:
[(K1 / Be) + (K2 * Dc)] r
Where K1, K2 = constants, Be = unloaded path bandwidth x (1 - channel occupancy), Dc = topological delay, and r = reliability.
The path having the smallest composite metric will be the best path. Where there are multiple paths to the same destination, the gateway can route the packets over more than one path. This is done in accordance with the composite metric for each data path. For instance, if one path has a composite metric of 1 and another path has a composite metric of 3, three times as many packets will be sent over the data path having the composite metric of 1.
There are two advantages to using a vector of metric information. The first is that it provides the ability to support multiple types of service from the same set of data. The second advantage is improved accuracy. When a single metric is used, it is normally treated as if it were a delay. Each link in the path is added to the total metric. If there is a link with a low bandwidth, it is normally represented by a large delay. However, bandwidth limitations don't really cumulate the way delays do. By treating bandwidth as a separate component, it can be handled correctly. Similarly, load can be handled by a separate channel occupancy number.
IGRP provides a system for interconnecting computer networks which can stably handle a general graph topology including loops. The system maintains full path metric information, i.e., it knows the path parameters to all other networks to which any gateway is connected. Traffic can be distributed over parallel paths and multiple path parameters can be simultaneously computed over the entire network.


2)	LINK STATE:
Link state routing is the second family of routing protocols. While distance vector routers use a distributed algorithm to compute their routing tables, link-state routers exchange messages to allow each router to learn the entire network topology. Based on this learned topology, each router is then able to compute its routing table by using a shortest path computation.
For link-state routing, a network is modelled as a directed weighted graph. Each router is a node, and the links between routers are the edges in the graph. A positive weight is associated to each directed edge and routers use the shortest path to reach each destination. In practice, different types of weight can be associated to each directed edge :
•	unit weight. If all links have a unit weight, shortest path routing prefers the paths with the least number of intermediate routers.
•	weight proportional to the propagation delay on the link. If all link weights are configured this way, shortest path routing uses the paths with the smallest propagation delay.
•	  where C is a constant larger than the highest link bandwidth in the network. If all link weights are configured this way, shortest path routing prefers higher bandwidth paths over lower bandwidth paths
Usually, the same weight is associated to the two directed edges that correspond to a physical link (i.e.   and  ). However, nothing in the link state protocols requires this. For example, if the weight is set in function of the link bandwidth, then an asymmetric ADSL link could have a different weight for the upstream and downstream directions. Other variants are possible. Some networks use optimisation algorithms to find the best set of weights to minimize congestion inside the network for a given traffic demand.

When a link-state router boots, it first needs to discover to which routers it is directly connected. For this, each router sends a HELLO message every N seconds on all of its interfaces. This message contains the router’s address. Each router has a unique address. As its neighbouring routers also send HELLO messages, the router automatically discovers to which neighbours it is connected. These HELLO messages are only sent to neighbours who are directly connected to a router, and a router never forwards the HELLO messages that they receive. HELLO messages are also used to detect link and router failures. A link is considered to have failed if no HELLO message has been received from the neighbouring router for a period of   seconds.

Once a router has discovered its neighbours, it must reliably distribute its local links to all routers in the network to allow them to compute their local view of the network topology. For this, each router builds a link-state packet (LSP) containing the following information :
•	LSP.Router : identification (address) of the sender of the LSP
•	LSP.age : age or remaining lifetime of the LSP
•	LSP.seq : sequence number of the LSP
•	LSP.Links[] : links advertised in the LSP. Each directed link is represented with the following information : - LSP.Links[i].Id : identification of the neighbour - LSP.Links[i].cost : cost of the link
These LSPs must be reliably distributed inside the network without using the router’s routing table since these tables can only be computed once the LSPs have been received. The Flooding algorithm is used to efficiently distribute the LSPs of all routers. Each router that implements flooding maintains a link state database(LSDB) containing the most recent LSP sent by each router. When a router receives an LSP, it first verifies whether this LSP is already stored inside its LSDB. If so, the router has already distributed the LSP earlier and it does not need to forward it. 

OSPF:
OSPF is a standardized Link-State routing protocol, designed to scale
efficiently to support larger networks.
OSPF adheres to the following Link State characteristics:
• OSPF employs a hierarchical network design using Areas.
• OSPF will form neighbor relationships with adjacent routers in the
same Area.
• Instead of advertising the distance to connected networks, OSPF
advertises the status of directly connected links using Link-State
Advertisements (LSAs).
• OSPF sends updates (LSAs) when there is a change to one of its links,
and will only send the change in the update. LSAs are additionally
refreshed every 30 minutes.
• OSPF traffic is multicast either to address 224.0.0.5 (all OSPF
routers) or 224.0.0.6 (all Designated Routers).
• OSPF uses the Dijkstra Shortest Path First algorithm to determine
the shortest path.
• OSPF is a classless protocol, and thus supports VLSMs.
Other characteristics of OSPF include:
• OSPF supports only IP routing.
• OSPF routes have an administrative distance is 110.
• OSPF uses cost as its metric, which is computed based on the
bandwidth of the link. OSPF has no hop-count limit.
The OSPF process builds and maintains three separate tables:
• A neighbor table – contains a list of all neighboring routers.
• A topology table – contains a list of all possible routes to all known
networks within an area.
• A routing table – contains the best route for each known network.
OSPF Neighbors:
OSPF forms neighbor relationships, called adjacencies, with other routers in
the same Area by exchanging Hello packets to multicast address 224.0.0.5.
Only after an adjacency is formed can routers share routing information.
Each OSPF router is identified by a unique Router ID. The Router ID can
be determined in one of three ways:
• The Router ID can be manually specified.
• If not manually specified, the highest IP address configured on any
Loopback interface on the router will become the Router ID.
• If no loopback interface exists, the highest IP address configured on
any Physical interface will become the Router ID.
By default, Hello packets are sent out OSPF-enabled interfaces every 10
seconds for broadcast and point-to-point interfaces, and 30 seconds for non-broadcast
and point-to-multipoint interfaces.
OSPF also has a Dead Interval, which indicates how long a router will wait
without hearing any hellos before announcing a neighbor as “down.” Default
for the Dead Interval is 40 seconds for broadcast and point-to-point
interfaces, and 120 seconds for non-broadcast and point-to-multipoint
interfaces. Notice that, by default, the dead interval timer is four times the
Hello interval.

OSPF routers will only become neighbors if the following parameters within
a Hello packet are identical on each router:
• Area ID
• Area Type (stub, NSSA, etc.)
• Prefix
• Subnet Mask
• Hello Interval
• Dead Interval
• Network Type (broadcast, point-to-point, etc.)
• Authentication
The Hello packets also serve as keep alive to allow routers to quickly
discover if a neighbor is down. Hello packets also contain a neighbor field
that lists the Router IDs of all neighbors the router is connected to.
A neighbor table is constructed from the OSPF Hello packets, which
includes the following information:
• The Router ID of each neighboring router
• The current “state” of each neighboring router
• The interface directly connecting to each neighbor
• The IP address of the remote interface of each neighbor

In multi-access networks such as
Ethernet, there is the possibility of
many neighbor relationships on the
same physical segment. In the above
example, four routers are connected
into the same multi-access segment.
Using the following formula (where
“n” is the number of routers):
n(n-1)/2
…it is apparent that 6 separate adjacencies are needed for a fully meshed
network. Increase the number of routers to five, and 10 separate adjacencies
would be required. This leads to a considerable amount of unnecessary Link
State Advertisement (LSA) traffic.
If a link off of Router A were to fail, it would flood this information to all
neighbors. Each neighbor, in turn, would then flood that same information to
all other neighbors. This is a waste of bandwidth and processor load.
To prevent this, OSPF will elect a Designated Router (DR) for each multi-access
networks, accessed via multicast address 224.0.0.6. For redundancy
purposes, a Backup Designated Router (BDR) is also elected.
OSPF routers will form adjacencies with the DR and BDR. If a change
occurs to a link, the update is forwarded only to the DR, which then
forwards it to all other routers. This greatly reduces the flooding of LSAs.
DR and BDR elections are determined by a router’s OSPF priority, which
is configured on a per-interface basis (a router can have interfaces in
multiple multi-access networks). The router with the highest priority
becomes the DR; second highest becomes the BDR. If there is a tie in
priority, whichever router has the highest Router ID will become the DR.
To change the priority on an interface:
Router(config-if)# ip ospf priority 125
Default priority on Cisco routers is 1. A priority of 0 will prevent the router
from being elected DR or BDR. Note: The DR election process is not
preemptive. Thus, if a router with a higher priority is added to the network, it
will not automatically supplant an existing DR. Thus, a router that should
never become the DR should always have its priority set to 0.

Neighbor adjacencies will progress through several states, including:
Down – indicates that no Hellos have been heard from the neighboring
router.
Init – indicates a Hello packet has been heard from the neighbor, but two way
communication has not yet been initialized.
2-Way – indicates that bidirectional communication has been established.
Recall that Hello packets contain a neighbor field. Thus, communication is
considered 2-Way once a router sees its own Router ID in its neighbor’s
Hello Packet. Designated and Backup Designated Routers are elected at
this stage.
ExStart – indicates that the routers are preparing to share link state
information. Master/slave relationships are formed between routers to
determine who will begin the exchange.
Exchange – indicates that the routers are exchanging Database Descriptors
(DBDs). DBDs contain a description of the router’s Topology Database. A
router will examine a neighbor’s DBD to determine if it has information to
share.
Loading – indicates the routers are finally exchanging Link State
Advertisements, containing information about all links connected to each
router. Essentially, routers are sharing their topology tables with each other.
Full – indicates that the routers are fully synchronized. The topology table of
all routers in the area should now be identical. Depending on the “role” of
the neighbor, the state may appear as:
• Full/DR – indicating that the neighbor is a Designated Router (DR)
• Full/BDR – indicating that the neighbor is a Backup Designated
Router (BDR)
• Full/DROther – indicating that the neighbor is neither the DR or
BDR
On a multi-access network, OSPF routers will only form Full adjacencies
with DRs and BDRs. Non-DRs and non-BDRs will still form adjacencies,
but will remain in a 2-Way State. This is normal OSPF behavior.
OSPF Network Types:
OSPF’s functionality is different across several different network topology
types. OSPF’s interaction with Frame Relay will be explained in another
section
Broadcast Multi-Access – indicates a topology where broadcast occurs.
• Examples include Ethernet, Token Ring, and ATM.
• OSPF will elect DRs and BDRs.
• Traffic to DRs and BDRs is multicast to 224.0.0.6. Traffic from
DRs and BDRs to other routers is multicast to 224.0.0.5.
• Neighbors do not need to be manually specified.
Point-to-Point – indicates a topology where two routers are directly
connected.
• An example would be a point-to-point T1.
• OSPF will not elect DRs and BDRs.
• All OSPF traffic is multicast to 224.0.0.5.
• Neighbors do not need to be manually specified.
Point-to-Multipoint – indicates a topology where one interface can connect
to multiple destinations. Each connection between a source and destination
is treated as a point-to-point link.
• An example would be Point-to-Multipoint Frame Relay.
• OSPF will not elect DRs and BDRs.
• All OSPF traffic is multicast to 224.0.0.5.
• Neighbors do not need to be manually specified.
Non-broadcast Multi-access Network (NBMA) – indicates a topology
where one interface can connect to multiple destinations; however,
broadcasts cannot be sent across a NBMA network.
• An example would be Frame Relay.
• OSPF will elect DRs and BDRs.
• OSPF neighbors must be manually defined, thus All OSPF traffic is unicast instead of multicast. Remember: on non-broadcast networks, neighbors must be manually specified, a multicast Hello’ s are not allowed.
  

 

 


 

 


3)	HYBRID:
Hybrid Routing Protocol (HRP) is a network routing protocol that combines Distance Vector Routing Protocol (DVRP) and Link State Routing Protocol (LSRP) features. HRP is used to determine optimal network destination routes and report network topology data modifications.
HRP features are as follows: 
•	Requires less memory and processing power than LSRP
•	Integrates reactive and proactive routing advantages
•	Serves activated nodes via reactive flooding
Proactive HRPs are as follows: 
•	Enhanced Interior Gateway Routing Protocol (EIGRP): Employs LSRP mechanisms
•	Core Extraction Distributed Ad Hoc Routing (CEDAR): Establishes a data transmission network via reactive core node routing
•	Zone Routing Protocol (ZRP): Segments networks into local neighborhoods (known as zones)
•	Zone-Based Hierarchical Link State (ZHLS): Peer-to-peer (P2P) protocol based on node and zone identification
Reactive HRPs with efficient flooding mechanisms are as follows:
•	Preferred Link-Based Routing (PLBR): Reactive routing protocol, where each node maintains a neighbor table (NT) and neighbor’s neighbor table (NNT)
•	Neighbor Degree-Based Preferred Link (NDPL) and Weight-Based Preferred Link (WBPL) Subset: Preferred list (PL) routing request messages forwarded by PLs only
•	Optimized Link State Routing (OLSR): Proactive routing protocol based on the link state algorithm


HRP is also known as Balanced Hybrid Routing (BHR).

EIGRP:
EIGRP (Enhanced Interior Gateway Routing Protocol) is a network protocol that lets routers exchange information more efficiently than with earlier network protocols. EIGRP evolved from IGRP (Interior Gateway Routing Protocol) and routers using either EIGRP and IGRP can interoperate because the metric (criteria used for selecting a route) used with one protocol can be translated into the metrics of the other protocol. EIGRP can be used not only for Internet Protocol (IP) networks but also for AppleTalkand Novell NetWare networks.
Using EIGRP, a router keeps a copy of its neighbor's routing tables. If it can't find a route to a destination in one of these tables, it queries its neighbors for a route and they in turn query their neighbors until a route is found. When a routing table entry changes in one of the routers, it notifies its neighbors of the change only (some earlier protocols require sending the entire table). To keep all routers aware of the state of neighbors, each router sends out a periodic "hello" packet. A router from which no "hello" packet has been received in a certain period of time is assumed to be inoperative.
EIGRP uses the Diffusing-Update Algorithm (DUAL) to determine the most efficient (least cost) route to a destination. A DUAL finite state machine contains decision information used by the algorithm to determine the least-cost route (which considers distance and whether a destination path is loop-free).

 

 


 

 

