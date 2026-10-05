(Day 10)
<table>
<tr>
    <th>Version</th>
	<th>IHL</th>
	<th>DSCP</th>
	<th>ECN</th>
	<th>Total Length</th>
	<th>Identification</th>
	<th>Flags</th>
	<th>Fragment Offset</th>
	<th>TTL</th>
	<th>Protocol</th>
	<th>Checksum</th>
	<th>Source</th>
	<th>Destination</th>
	<th>Options</th>
</tr>
<tr>
	<td>4 bits</td>
	<td>4 bits</td>
	<td>6 bits</td>
	<td>2 bits</td>
	<td>16 bits</td>
	<td>16 bits</td>
	<td>3 bits</td>
	<td>13 bits</td>
	<td>8 bits</td>
	<td>8 bits</td>
	<td>16 bits</td>
	<td>32 bits</td>
	<td>32 bits</td>
	<td>0 – 320 bits</td>
</tr>

</table>

<span style="font-size: 1.2em">Version:</span>
- Identifies version of IP used
- IPv4 = 0100
- IPv6 = 0110 (IPv6 has different header format after version field)

<span style="font-size: 1.2em">Internet Header Length (IHL):</span>
- Indicates the total length of the header because the final field, options, is variable in length
- Identifies length of header in 4 byte increments
- Minimum value is 5 (=20 bytes) (empty options field)
- Maximum value is 15 (=60 bytes)

<span style="font-size: 1.2em">Differentiated Services Code Point (DSCP):</span>
- Used for Quality of Service (QOS)



| f   | f   | f   |
| --- | --- | --- |
|     |     | f   |



[[CCNA]]