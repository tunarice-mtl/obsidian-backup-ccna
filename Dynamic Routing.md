(Day 24)

Network Route: A route to a network/subnet (mask length < /32)
Host route: A route to a specific host (/32 mask)

- Routers form  'adjacencies' / 'neighbor relationships' / 'neighborships' with directly connected neighbors to exchange information
- ==Routers 'advertise' information using dynamic routing protocols about the routes they know to other routers==
	- They keep sharing with each other until all routers in the network are reachable
- If an interface goes down, other routers automatically remove the route from the routing tables
	- As opposed to static routing where routers will keep trying to reach the downed network
- ==If multiple routes to a destination are learned, router selects superior route based on the **lowest metric** to decide which to add to its routing table==
	- The unselected routes will not appear in the routing table, but will be used as backup and added to the routing table if something happens to the superior route

<span style="font-size: 1.2em">Types of Dynamic Routing Protocols:</span>
- <span style="color:LightCoral">IGP</span> (Interior Gateway Protocol)
	- Used to share routes ==within== a single *autonomous system* (AS), which is a single organization (ie. a company)
- <span style="color:MediumPurple">EGP</span> (Exterior Gateway Protocol):
	- Used to share routes ==between== different autonomous systems
![[Screenshot 2026-10-04 at 11.24.07 AM.png|center]]
<center>
<table>
<tr>
	<th>DRP</th>
	<th>Algorithm Type</th>
	<th>Protocol</th>
</tr>
<tr>
	<td rowspan="4" style="background-color:LightCoral">IGP</td>
	<td rowspan="2"   style="background-color:LightGreen">Distance Vector</td>
	<td   style="background-color:LightGreen">Routing Information Protocol (RIP)</td>
</tr>
	<tr>
		<td   style="background-color:LightGreen">Enhanced Interior Gateway Routing Protocol (EIGRP)</td>
	</tr>
<tr>
	<td rowspan="2"  style="background-color:LightGoldenRodYellow">Link State</td>
	<td style="background-color:LightGoldenRodYellow">Open Shortest Path First (OSPF)</td>
</tr>
<tr>
	<td style="background-color:LightGoldenRodYellow">Intermediate System to Intermediate System (IS-IS)</td>
</tr>
<tr>
	<td style="background-color:MediumPurple">EGP</td>
	<td style="background-color:LightCyan">Path Vector</td>
	<td style="background-color:LightCyan">Border Gateway Protocol (BGP)</td>
</tr>
</table>
</center>

<span style="font-size: 1.2em">Distance Vector Routing Protocols:</span>
- Invented before link state protocols (RIPv1, IGRP –> EIGRP)
- Operates by sending information to its directly connected neighbors
- 'routing by rumor'
	- ==Each route only knows what its neighbor tells it:==
		- *Its known destination networks*
		- *Its metric to reach its known destination networks*
- ==Routers only learn the 'distance' (metric) and the 'vector' (direction, next-hop router) of each route==
	- In other words, routers share their route tables with their neighbors

<span style="font-size: 1.2em">Link State Routing Protocols:</span>
- ==Each router creates its own map of the network which it uses to independently calculate the best routes to each destination==
	- Each router advertises info about its interfaces (connected networks) to its neighbors
	- Advertisements are passed along to other routers until all routers in the network develop the same map
- Uses more resources (CPU), however react to changes in the network faster than distance vector
- Protocols used today: OSPF, IS-IS

<span style="font-size: 1.2em">Dynamic Routing Protocol Metrics:</span>
- ==🔵Routing tables display the best route to each destination network it knows about==
- If there are multiple routes to the same destination, it uses **metric** to determine which is best
	- Lower metric = better
- ==Each protocol uses a different metric== to determine which route is the best
- **Equal Cost Multi-Path (ECMP):**
	- If two or more routes have the same **destination**, **routing protocol**, and **metric value**, they will both be added to the routing table and traffic will be load-balanced between them
		- Must be exactly the same destination: network address and prefix length
	- Can be configured with static or dynamic routes
		- Static  routes have a metric of zero \[AD/<span style="color:red">0</span>]

\[<span style="color:blue">Administrative Distance</span>/<span style="color:red">metric</span>]
```
O     192.168.4.0/24 [110/3] via 10.0.13.2, 00:00:09, GigabitEthernet1/0
					 [110/3] via 10.0.12.2, 00:00:09, GigabitEthernet0/0
```


<table>
	<tr>
		<th>IGP</th>
		<th>Metric</th>
		<th>Explanation</th>
	</tr>
	<tr>
		<td>RIP</td>
		<td>Hop count</td>
		<td>Each router in the path counts as one 'hop'. The total metric is the total number of hops to the destination. <b>Links of all speeds are equal.</b></td>
	</tr>
	<tr>
		<td>EIGRP</td>
		<td>Metric based on bandwidth & delay (by default)</td>
		<td>Formula with many values. By default, bandwidth of the <b>slowest link in the route</b> and total delay of all links in the route</td>
	</tr>
	<tr>
		<td>OSPF</td>
		<td>Cost</td>
		<td>The cost of each link is calculated based on bandwidth. The total metric is the total cost of each link in the route</td>
	</tr>
		<tr>
		<td>IS-IS</td>
		<td>Cost</td>
		<td>The total metric is the total cost of each link in the route. The total cost of each link is <b>not</b> automatically calculated by default. All links have a cost of 10 by default</td>
	</tr>
</table>

<span style="font-size: 1.2em">Administrative Distance (AD):</span>
- When multiple protocols are in use, ==AD is used to determine which routing protocol is preferred==
	- Most of the time a company will only use a single IGP– usually OSPF or EIGRP
	- Sometimes, they might use two. Ex. two companies using different routing protocols might connect their networks to share information
- Different routing protocols use different metrics, so they cannot be compared
- Lower AD = better, routing protocol is more 'trustworthy' (more likely to select good routes)

| Route Protocol/Type | AD  |
| ------------------- | --- |
| Directly connected  | 0   |
| Static              | 1   |
| External BGP (eBGP) | 20  |
| EIGRP               | 90  |
| IGRP                | 100 |
| OSPF                | 110 |
| IS-IS               | 115 |
| RIP                 | 120 |
| EIGRP (external)    | 170 |
| Internal BGP (iBGP) | 200 |
| Unusable route      | 255 |

==If the administrative distance is 255, the router does not believe the source of that route and does not install the route in the routing table.==

> [!question]- The following routes to the destination network 10.1.1.0/24 are learned: <br>next hop 192.168.1.1, learned via RIP, metric 5 <br>next hop 192.168.2.1, learned via RIP metric 3 <br>next hop 192.168.3.1, learned via OSPF metric 10 <br><br>Which route to 10.1.1.0/24 be added to the route table?
> next hop 192.168.3.1, learned via OSPF metric 10
> 
> Before comparing metrics, AD is used to select the best route. The OSPF route will alway s take precedence over the RIP routes because it has a lower AD.


You can manually set the AD of a routing protocol or static route:
```
R1(config)#ip route [target ip] [target ip mask] [next-hop] [distance metric]
```
<span style="font-size: 1.2em">Floating Static Route:</span>
- By setting the AD of the static route, you can make it less preferred than routes learned by a dynamic routing protocol to the same destination
	- (Make sure the AD is higher than the routing protocol's AD)
- The route will be inactive unless the route learned by the DRP is removed
	- Ex. Router stops advertising it, or interface failure causes an adjacency with a neighbor to be lost



[[CCNA]]