(Day 5)

<center><table>
<tr>
	<th colspan="5">Header</th>
	<th>Payload (packet)</th>
	<th>Trailer</th>
</tr>
<tr>
    <th>Preamble</th>
	<th>SFD</th>
	<th>Destination MAC</th>
	<th>Source MAC</th>
	<th>Type/Length</th>
	<th>Packet</th>
	<th>FCS</th>
</tr>
<tr>
	<td>7 bytes</td>
	<td>1 byte</td>
	<td>6 bytes</td>
	<td>6 bytes</td>
	<td>2 bytes</td>
	<td>min 46 bytes, max 1500</td>
	<td>4 bytes</td>
</tr>

</table>
</center>

(Preamble and SFD often excluded when counting length of header. Including Preamble and SFD, Header + Trailer = 26 bytes. Excluding: 18 bytes.)

<span style="font-size: 1.2em">Preamble</span>
- Bit sequence: 10101010 x 7
- Allows devices to synchronize their receiver clocks

<span style="font-size: 1.2em">Start Frame Delimiter (SFD):</span>
- Bit sequence: 10101011
- Marks end of the preamble, and beginning of the rest of the frame

<span style="font-size: 1.2em">Destination & Source:</span>
- Indicate the devices sending and receiving the frame using their MAC address

<span style="font-size: 1.2em">Type/Length:</span>
- A value ≤ 1500 indicates the length of the encapsulated packet (in bytes)
- A value ≥ 1536 indicates the type of the encapsulated packet (usually IPv4 or IPv6)
<table>
<tr>
	<th>IPv4</th>
	<th>IPv6</th>
	<th>ARP</th>
</tr>
<tr>
	<th>0x800</th>
	<th>0x86DD</th>
	<th>0x0806</th>
</tr>
</table>

<span style="font-size: 1.2em">Frame Check Sequence (FCS):</span>
- Detects corrupted data by running a CRC (Cyclic Redundancy Check) algorithm over the received data

<span style="font-size: 1.2em">MAC Addresses (AKA BIA):</span>
- Written as 12 hexadecimal characters
- Assigned to the device when it is made, globally unique
- First 3 bytes are the OUI (Organizationally Unique Identifier) which is assigned to the company making the device
- Last 3 bytes are unique to the device

---
<span style="font-size: 1.2em">Example</span>
- PC1 sends data to PC2: 
	- This is the first time the switch is receiving something from PC1, so it logs the addresses in its MAC Address Table along with the associated interface (in this case interface F0/1)
		- Dynamically learned MAC Address
- Ex I'm lazy


- Unicast = message sent to one person– as opposed to broadcast, sent to multiple people
- The switch FORWARDS a KNOWN unicast frame
- On Cisco switches, MAC addresses age from the MAC address table after 5 minutes of inactivity

- Switches use source address to fill in the MAC address table. The interface doesn't mean the device is actually connected to that interface

wordsaa
and more words

[[CCNA]]