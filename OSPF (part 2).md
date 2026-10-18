(Day 26)

<u><span style="font-size: 1.2em">OSPF Cost:</span></u>
- OSPF's metric is called **cost**
- Automatically calculated based on bandwidth (speed) of the interface
	- ==🟢Reference bandwidth / interface's bandwidth==
- ==Default reference bandwidth = **100 mbps**==
> [!EXAMPLE] **Reference:** 100 mbps / **Interface:** 10 mbps = cost of 10<br>Reference: 100 mbps / Interface: 100 mbps = cost of 1<br>Reference: 100 mbps / Interface: 1000 mbps = cost of 1<br>Reference: 100 mbps / Interface: 10000 mbps = cost of 1
- ==All values less than 1 will be converted to 1==
- ==🔵FastEthernet, Gigabit Ethernet, 10Gig Ethernet, etc. are all equal and have a cost of 1 by default==

