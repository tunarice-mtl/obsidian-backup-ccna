(Day 25)

<span style="font-size: 1.2em">RIP:</span>
- Distance vector IGP ('routing by rumor')
- Hop count as a metric
- Maximum hop count is **15** (anything more is considered unreachable)
- Three versions:
	- IPv4: RIPv1, RIPv2
	- IPv6: RIPng (RIP Next Generation)
- Two message types:
	- Request: Asks RIP-enabled neighbor routers to send their routing table
	- Response: Sends the local router's routing table to neighboring routers
- By default, RIP routers share their routing table every 30 seconds
	- In large network, regular updates can clog the network
- 

RIPv1 (outdated):
- Only advertises *classful* addresses (Class A, Class B, Class C)
- Doesn't support VLSM, CIDR
- Doesn't include subnet mask info in advertisements (Response messages)
	- 10.1.1.0/24 will become 10.0.0.0 (Class A address, assumed /8)
	- 172.16.192.0/18 will become 172.16.0.0 (Class B address, assumed /16)
	- 192.168.1.4/30 will become 192.168.1.0 (Class C address, assumed /24)
- Messages are broadcast to 255.255.255.255 (all routers on local segment receive the messages)

RIPv2:
- Supports VLSM, CIDR
- Includes subnet mask information in advertisements
- Messages are **multicast** to 224.0.0.9 (Class D range)
	- Messages are delivered only to devices that have joined that specific *multicast group*

<span style="font-size: 1.2em">RIP Configuration:</span>
```
R1(config)#router rip
R1(config-router)#version 2
R1(config-router)#no auto-summary
R1(config-router)#network 10.0.0.0
R1(config-router)#network 172.16.0.0
```
auto-summary: converts networks router advertises to classful networks
- No need to enter network mask
	- network 10.0.12.0 will be converted into 10.0.0.0 (class A) automatically
- Network command
	- 


[[CCNA]] [[Dynamic Routing]]