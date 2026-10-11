(Day 25)

<span style="font-size: 1.2em">RIP (Routing Information Protocol):</span>
- Distance vector IGP ('routing by rumor')
- ==🟡Hop count as a metric,== ==🟢max hop count = **15** ==(anything more considered unreachable)
- Three versions
	- IPv4: RIPv1, RIPv2
	- IPv6: RIPng (RIP Next Generation)
- Two message types:
	- Request: Asks RIP-enabled neighbor routers to send their routing table
	- Response: Sends the local router's routing table to neighboring routers
- By default, ==🟢RIP routers share their routing table every 30 seconds==
	- In large network, regular updates can clog the network

RIPv1 (outdated):
- ==Only advertises *classful* addresses (Class A, Class B, Class C)==
- Doesn't support VLSM, CIDR
- Doesn't include subnet mask info in advertisements (Response messages)
	- 10.1.1.0/24 will become 10.0.0.0 (Class A address, assumed /8)
	- 172.16.192.0/18 will become 172.16.0.0 (Class B address, assumed /16)
	- 192.168.1.4/30 will become 192.168.1.0 (Class C address, assumed /24)
- ==Messages are **broadcast** to 255.255.255.255== (all routers on local segment receive the messages)

RIPv2:
- Supports VLSM, CIDR
- Includes subnet mask information in advertisements
- ==Messages are **multicast** to 224.0.0.9== (Class D range)
	- Messages are delivered only to devices that have joined that specific ==🟡*multicast group*==

<span style="font-size: 1.2em">RIP Configuration:</span>
==🟠**router rip** command==
==🟠**auto-summary** command: converts networks router advertises to classful networks==
```
R1(config)#router rip
R1(config-router)#version 2
R1(config-router)#no auto-summary
```
- No need to enter network mask
	- Network 10.0.12.0 will be converted into 10.0.0.0 (class A) automatically

==🟠**network** command:==
	- Look for interfaces with an IP address that is in the specified range
	- Activate RIP on that fall in the range and form adjacencies with connected RIP neighbors
	- ==🔵Advertise **the network prefix of the interface** (NOT the prefix in the **network** command)==
	- OSPF and EIGRP network commands operate in the same way

<u>Example with the following network:</u>![[Screenshot 2026-10-10 at 12.12.19 PM.png | center]]
```
R1(config-router)#network 10.0.0.0
```
- Because the **network** command is classful, 10.0.0.0 is assumed to be 10.0.0.0/8
- R1 will look for any interfaces with an IP address that matches 10.0.0.0/8 (/8, so only needs to match the first 8 bits)
- 10.0.12.1 and 10.0.13.1 match, so RIP is activated in G0/0 and G1/0
	- R1 forms adjacencies with its neighbors R2 and R3 through those interfaces
- ==🔵Network command tells router which **interfaces** to enable RIP on. Then **advertises the network prefix** of those interfaces. It does **NOT** tell the router which networks to advertise==
	- R1 advertises 10.0.12.0/30 and 10.0.13.0/30 to its RIP neighbors, NOT 10.0.0.0/8

```
R1(config-router)#network 172.16.0.0
```
- 172.16.0.0 assumed to be /16
	- 172.16.1.14/28 matches, so RIP is enabled on G2/0
	- There are no RIP neighbors so no adjacencies are formed. However, it will continue sending RIP advertisements out of G2/0 causing unnecessary traffic
		- As such, G2/0 should be configured as a ==**passive interface**==

==🟠**passive-interface** command:==
```
R1(config-router)#passive-interface g2/0
```
- tells the router to stop sending RIP advertisements out of the specified interface
- However, router continues to advertise the network prefix to its RIP neighbors (R2, R3)
- ==🔴Always enable this on interfaces that don't have any adjacencies==

==🟠**default-information** command:==
```
R1(config-router)#default-information originate
```
- ==🔵Share default gateway info into RIP==
- Route will be passed along, advertised to all routers in the network
- In this case, if default-information is issued on R1, R4 will receive two routes. One from R3 and the other from R2
	- Since its RIP, the two paths will have the same hop count and traffic will be load balanced between them
- OSPF and EIGRP have the same default-info command

**==🟠show ip protocols==** command:
```
R1#show ip protocols
```
- Displays information about protocol, timers, version in use, auto-summary
- Maximum-paths: edit using ==🟠maximum-paths \[# max paths]== command
- Which interfaces to activate RIP on (displays as the classful address)
- Passive interfaces, rip neighbors
- Administrative distance: edit using ==🟠distance \[administrative distance (1 - 255)]== command

<span style="font-size: 1.2em">EIGRP (Enhanced Interior Gateway Routing Protocol):</span>
- Used to be Cisco proprietary but now published openly. But OSPF is still used more
- 'advanced' / 'hybrid' distance vector routing protocol
- Faster reaction to changes than RIP and no 15 'hop-count' max
- Sends messages using ==multicast address 224.0.0.10==
- ==🔵The only IGP that can perform **unequal**-cost load-balancing (by default, ECMP load-balancing over 4 paths like RIP)==
	- Can be configured to load-balance over multiple paths that don't have equal cost. Will even ==load-balance in proportion to bandwidth== of individual paths (more traffic over lower metric paths and vice versa)

<span style="font-size: 1.2em">EIGRP Configuration:</span>
- You can have EIGRP and RIP configured on a router at the same time but why would you do that
```
R1(config)#router eigrp 1
R1(config-router)#no auto-summary
```


[[CCNA]] [[Dynamic Routing]]