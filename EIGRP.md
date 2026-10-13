(Day 25)
See [[RIP]].

<u><span style="font-size: 1.2em">EIGRP (Enhanced Interior Gateway Routing Protocol):</span></u>
- Used to be Cisco proprietary but now published openly. But OSPF is still used more
- 'advanced' / 'hybrid' distance vector routing protocol
- Faster reaction to changes than RIP and no 15 'hop-count' max
- Sends messages using ==multicast address 224.0.0.10==
- ==🔵The only IGP that can perform **unequal**-cost load-balancing (by default, ECMP load-balancing over 4 paths like RIP)==
	- Can be configured to load-balance over multiple paths that don't have equal cost. Will even ==load-balance in proportion to bandwidth== of individual paths (more traffic over lower metric paths and vice versa)

<span style="font-size: 1.2em">EIGRP Configuration:</span>
- You can have EIGRP and RIP configured on a router at the same time but why would you do that

==🟠**router eigrp \[AS #]** command:==
- AS (Autonomous System) number must match between routers, or they will not form an adjacency
```
R1(config)#router eigrp 1
```

auto-summary, passive-interface, and network commands function the same as in RIP however you can specify a ==*wildcard mask*==
```
R1(config-router)#network 172.16.1.0 0.0.0.15
```

<span style="font-size: 1.1em">Wildcard Masks:</span>
- EIGRP uses *wildcard masks* instead of regular subnet masks
- An 'inverted' subnet mask– flip all the bits

<u>Example of a /28 prefix:</u>
<center><table>
<tr>
	<th style="text-align:center">Subnetting</th>
	<th style="text-align:center">Wildcard</th>
</tr>
<tr>
	<td style="text-align:center">11111111.11111111.11111111. 11110000</td>
	<td style="text-align:center">00000000.00000000.00000000.00001111</td>
</tr>
<tr>
	<td style="text-align:center"> 255.255.255.240</td>
	<td style="text-align:center">0.0.0.15</td>
</tr>
</table></center>

Calculate without manually flipping bits:
255 - subnet mask = wildcard mask
255 - wildcard mask = subnet mask![[Screenshot 2026-10-11 at 5.28.50 PM.png | center|605]]
- A =='0'== in the wildcard mask = the ==bits must match== between the IP address and the EIGRP network command
- A =='1'== in the wildcard mask means the bits = ==don't have to match==

Example:
![[Screenshot 2026-10-12 at 11.00.32 AM.png|559]]
The last three bits of the mask are wildcards (1's). Which means the first 29 bits of the EIGRP network command and the interface IP must match (0's). They do match, so EIGRP will be enabled on the G2/0 interface. 

==🟠**show ip protocols** command:==
- Metric field:
	- EIGRP uses interface bandwidth and delay by default as metric (K1 and K3 values)
	- Metric = Bandwidth(slowest link in path) + sum(delayed values of all links in path)
	- Other K values = 0 by default
	- 90 internal and 170 external
- Router has unique Router-ID which identifies it in the AS
- ==🔴It is not actually an IP address, it is just a 32-bit number formatted like an ip address. Can be changed to any 32-bit number==
- Router ID order of priority:
	1) Manual configuration
	2) Highest IP address on a loopback interface
		- (Virtual interfaces inside the router)
	3) Highest IP address on a physical interface

<u>Configure EIGRP router ID:</u>
==🟠**eigrp router-id** command:==
```
R1(config-router)#eigrp router-id [router-id]
```

\ for space
> [!question]- R1's G1/0 interface has an IP address of 172.20.20.17 and its G2/0 interface has an IP address of 172.26.20.12. Which of the following network commands will activate EIGRP on both interfaces?<br>a) R1(config-router)# network 128.0.0.0  \ 127.255.255.255<br>b) R1(config-router)# network 172.16.0.0  \ 0.0.255.255<br>c) R1(config-router)# network 172.20.0.0  \  0.0.127.255<br>d) R1(config-router)# network 172.20.0.0 \ 0.3.255.255
> Answer: A) R1(config-router)# network 128.0.0.0  \ 127.255.255.255![[Screenshot 2026-10-12 at 1.07.10 PM.png]]only the first bit of the wildcard mask must match (is a 0). The rest are wildcards and don't have to match.
> 
> 









[[CCNA]] [[Dynamic Routing]] [[RIP]]