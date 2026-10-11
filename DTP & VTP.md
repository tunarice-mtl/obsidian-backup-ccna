(Day 19)

<u><span style="font-size: 1.2em">Dynamic Trunking Protocol (DTP):</span></u>
- Cisco proprietary protocol— ==enabled by default on all Cisco switches==
- Allows Cisco switches to dynamically determine their interface status (access or trunk) automatically
- ==For security purposes, DTP should be disabled on all switchports== (instead, you should manually configure interface status)
<u>Disable DTP negotiation on an interface:</u>
```
SW1(config-if)#switchport nonegotiate
SW1(config-if)#switchport mode access
```
==*Switches configured in trunk mode will continue to send DTP frames unless you issue the nonegotiate command*==

DTP Modes:
<center>
<table>
<tr style="background-color:LightSkyBlue; text-align:center;">
	<th style="background-color:LightSkyBlue; text-align:center;">Administrative mode</th>
	<th>Trunk</th>
	<th>Dynamic Desirable</th>
	<th>Access</th>
	<th>Dynamic Auto</th>
</tr>
<tr>
	<th style="background-color:LightSkyBlue; text-align:center;">Trunk</th>
	<td>Trunk</td>
	<td style="text-align:center">Trunk</td>
	<td  style="text-align:center">X</td>
	<td style="text-align:center">Trunk</td>
</tr>
<tr>
	<th style="background-color:LightSkyBlue; text-align:center;">Dynamic Desirable</th>
	<td style="text-align:center;">Trunk</td>
	<td style="text-align:center">Trunk</td>
	<td>Access</td>
	<td style="text-align:center">Trunk</td>
</tr>
<tr>
	<th style="background-color:LightSkyBlue; text-align:center;">Access</th>
	<td style="text-align:center">X</td>
	<td style="text-align:center">Access</td>
	<td>Access</td>
	<td style="text-align:center">Access</td>
</tr>
<tr>
	<th style="background-color:LightSkyBlue; text-align:center;">Dynamic Auto</th>
	<td>Trunk</td>
	<td style="text-align:center">Trunk</td>
	<td>Access</td>
	<td style="text-align:center">Access</td>
</tr>
</table>
</center>
Older switches default administrative mode: Dynamic desirable
Newer switches default administrative mode: Dynamic auto

For switches that support 802.1Q and ISL, the default trunk encapsulation mode is negotiate
```
SW1(config-if)#switchport trunk encapsulation negotiate
```
- If you want to manually configure a trunk interface on a switch that supports both 802.1Q and ISL, you must first change the encapsulation mode to one or the other. Cannot be in negotiate mode.
- ISL is favored over 802.1Q— if both switches support it, ISL will be selected
- DTP frames sent in VLAN1 when using ISL
- DTP frames sent in the native VLAN when using 802.1Q
	- 802.1Q default native VLAN is VLAN1
```
SW1(config-if)#switchport mode dynamic desirable
SW1(config-if)#do show interfaces g0/0 switchport
Name: Gi0/0
Switchport: Enabled
Administrative Mode: dynamic desirable
Operational Mode: trunk
Administrative Trunking Encapsulation: negotiate
Operational Trunking Encapsulation: isl
Negotiation of Trunking: On
```
If the interface is in access mode or switchport nonegotiate command, "Negotiation of Trunking" field will be "Off"

<u><span style="font-size: 1.2em">VLAN Trunking Protocol (VTP):</span></u>
- Allows you to configure VLANs on a central VTP server switch, which other VTP client witches will synchronize their VLAN database to
- Designed for large networks with many VLANS
- ==Rarely used, recommended that you do not use it==
- VTP versions v1, v2, and v3
	- VTPv2 has support for Token Ring VLANs. If you aren't using them, no reason to use VTPv2 over VTPv1
- Three VTP modes: **server**, **client**, and **transparent**
<span style="font-size: 1.1em">VTP Servers:</span>
- Can add/modify/delete VLANs
- ==Switches operate in VTP server mode by default==
- Store the VLAN database in non-volatile RAM (NVRAM)
	- Database is saved even if the switch is turned 
- **Revision number** keeps track of edits to VLANs
- Advertise the latest version of the VLAN database for VTP clients to sync to
- VTP servers function as VTP clients to VTP servers with a higher revision number
	- i.e. They will sync to VTP servers with higher revision number
<span style="font-size: 1.1em">VTP Clients:</span>
- Cannot add/modify/delete VLANs
- Do not store the VLAN database in NVRAM (VTPv3 does store in NVRAM)
- ==Will synchronize their VLAN database to the server with the highest revision number in the VTP domain==
- Advertise their VLAN database and forward VTP advertisements to other clients over their trunk ports
```
SW1#show vtp status
VTP Version capable               : 1 to 3
VTP Version running               : 1
VTP Domain Name                   :
VTP Pruning Mode                  : Disabled
VTP Traps Generation              : Disabled
Device ID                         : 0c09.f956.1300
Configuration last modified by [time info here]
Local updater ID is 0.0.0.0 (no valid interface found)

Feature VLAN:
- - - - - - - -
VTP Operating Mode                : Server
Maximum VLANs supported locally   : 1005
Number of existing VLANs          : 5
Configuration Revision            : 0
MD5 digest                        : [other info here]
```
- If you want VTP to synchronize devices, they must all have the same domain name
	- *NULL* (blank) by default
- VTPv1/v2 do not support the extended VLAN range (1006 - 4094)
- 5 existing VLANs on the switch by default: 1, 1002, 1003, 1004, 1005
Change domain name:
```
SW1(config)#vtp domain cisco
```
- If a switch with no VTP domain name receives a VTP advertisement with a VTP domain name, it will automatically join that VTP domain
- If switch receives a VTP advertisement in the same VTP domain with a higher revision number, it will update it's VLAN database to match
	- <mark style="background: LightCoral">DANGER: connecting an old switch with a higher revision number and same VTP domain name to your network, all switches in the domain will sync their VLAN database to that switch</mark>
<span style="font-size: 1.1em">VTP Transparent Mode:</span>
- Does not sync its VLAN database to the VLAN database
- Changes to its own database won't be advertised to other switches
- Will <mark style="background: #FFF3A3A6;">*forward* VTP advertisements</mark> to switches that are in the <mark style="background: #FFF3A3A6;">same domain</mark> as it
- ==Changing the VTP domain to an unused domain, or setting VTP mode to transparent will reset the revision number to 0==
![[Screenshot 2026-10-08 at 5.22.36 PM.png]]

- If SW3 is in a different domain as the rest of the switches, changes made to SW1 will not reach SW4 and vice versa. SW3 only forwards advertisements to switches in the same domain group as it
- Changing SW3's domain to match the rest of the switches, it will forward the changes to SW4

```
SW1(config)#vtp version [*version number*]
```
- Changing the version number also changes the revision number, and advertisements with new revision number will be sent

