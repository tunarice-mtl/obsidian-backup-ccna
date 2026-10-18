(Day 26)
<br>
<span style="font-size: 1.2em">OSPF (Open Shortest Path First):</span>
> [!INFO]- OSFP is a *link state* dynamic routing protocol
> ==Each router creates its own map of the network which it uses to independently calculate the best routes to each destination.== Each router advertises info about its interfaces (connected networks) to its neighbors. See [[Dynamic Routing]].
- Shortest path first algorithm (Dijkstra's algorithm)
- Three versions:
	- OSPFv1 (1989): OLD, not used anymore
	- OSPFv2 (1998): Used for IPv4 (covered in CCNA)
	- OSFPv3 (2008): Used for IPv6 (can also be used for IPv4, but usually v2 is used)
- Routers store information about the network in **LSAs** (Link State Advertisements)
	- Organized in a structure called **LSDB** (Link State Database)
- ==Routers **flood** LSAs== until all routers in the OSPF ==**area**== develop the same ==map of the network (LSDB)==

OSPF Process:
1) **Become neighbors** with other routers connected to the same segment
2) **Exchange LSAs** with neighbor routers
3) **Calculate the best routes** to each destination, and insert them into the routing table
<br>

<span style="font-size: 1.1em">LSA Flooding:</span>
- OSPF is enabled on R4's G1/0 interface
- R4 creates an LSA to tell its neighbors about the network on G1/0
- LSA is flooded throughout the network until all routers have received it, resulting in all routers sharing the same LSDB
	- LSDBs contain all the the LSAs in the network
- Each router then uses SPF algorithm to calculate its best route to 192.168.4.0/24
- LSAs:
	- LSA aging timer = 30 min by default. LSA is flooded again after the timer expires
	- 4.4.4.4 is either the manually configured address or the loopback address
![[Screenshot 2026-10-12 at 2.29.05 PM 5.png|363]]![[Screenshot 2026-10-12 at 2.50.31 PM.png]]
<br>

<span style="font-size: 1.1em">OSPF Areas:</span>[^1]
- ==🔵A set of routers and links that share the same LSDB, used to divide up the network==
- ==**Backbone area (area 0)**:== area that all other areas must <u>directly</u> connect to
- **==Internal routers:==** routers with all interfaces in the same area
- **==Area border routers (ABRs):==** Routers with interfaces in multiple areas
	- ABRs have a separate LSDB for each area they are connected to (recommended max 2)
- **==Backbone routers:==** routers connected to the backbone area
- **==Intra-area route:==** route to a destination inside the same OSPF area
- **==Inter-area route:==** route to a destination in a different OSPF area![[Screenshot 2026-10-12 at 3.35.43 PM.png | center|627]]
- Small networks can be a single-area without any negative effects on performance
- ==🔴Large networks should be divided into several areas==
	- SPF takes more time to calculate + processing power in larger areas
	- Larger LSDB takes up more memory
	- Any change in the network causes every router to flood LSAs and run SPF algorithm again

<span style="font-size: 1.1em">OSPF Rules:</span>
1) One area should be *contiguous*– all connected together and not split up like you can do with VLANs
2) All areas must have at least one ABR directly connected to the Backbone Area
3) Interfaces in the same subnet must be in the same area, otherwise they will not become neighbors

<span style="font-size: 1.2em">OSPF Configurations:</span>
**==🟠Entering ospf config mode:==**
```
R1(config)#router ospf [process id]
R1(config)#router ospf 1
```
- You can have different OSPF processes running at the same time, but you typically only use one
	- OSPF *process-ID* is **locally significant.** Routers with different process IDs can become OSPF neighbors
		- ==🔵In other words, which process you use has no effect on whether routers can be neighbors.== Unlike EIGRP AS, which must match between routers
	- Totally unrelated to area

```
R1(config-router)#network 10.0.12.0 0.0.0.3 area 1
R1(config-router)#network [ip address] [wildcard mask] area [area #]
```
- For CCNA you only need to configure single-area OSPF (usually area 0)

```
R1(config-router)#passive-interface [interface]
```
- Covered in [[RIP]] and [[EIGRP]]
	- Tells the router to stop sending OSPF 'hello' messages out of the interface
- Router will continue to send LSAs informing neighbors about the subnet configured on the interface

```
R1(config-router)#default-information originate
```
- In OSPF, causes router to create a new LSA and flood it

Before manually setting a router-id, you must first use **==🟠clear ip ospf process==** command. ==🔴Do not do in live network== 
**Autonomous system boundary router (ASBR):** an OSPF router that connects the OSPF network to an external network
- R1 is connected to the internet. By using the **==🟠default-information originate==** command, R1 becomes the ASBR
Doesn't support UCLB, does support ECMP
**==🟠maximum-paths==** command


[[CCNA]]


[^1]: CCNA only covers single area
