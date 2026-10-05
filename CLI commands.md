
<table>
<tr>
    <th colspan="2">ROUTING</th>
</tr>
<tr>
	<th>Command</th>
	<th>Description</th>
</tr>
  <tr>
	<td>ip route [target-ip address] [target-ip netmask] [next-hop ip]</td>
	<td>Create route between networks</td>
  </tr>
    <tr>
	  <td>ip route 0.0.0.0 0.0.0.0 [net-hop ip]</td>
	  <td>Create default gateway at to next-hop</td>
  </tr>
  <tr>
	  <td>show ip route</td>
	  <td>Show ip routes on router</td>
  </tr>
</table>

<table>
<tr>
    <th colspan="2">VLAN (Switches)</th>
</tr>
<tr>
	<th>Command</th>
	<th>Description</th>
</tr>
  <tr>
	<td>switchport access vlan [vlan-id]</td>
	<td>assign vlan to access interface</td>
  </tr>
    <tr>
	  <td>switchport trunk allowed vlan [vlan-id(s)]</td>
	  <td>Enable vlans on trunk interface</td>
  </tr>
  <tr>
	  <td>switchport trunk allowed vlan [add/remove] [vlan-id]</td>
	  <td>Adding/removing vlans in TRUNK interfaces</td>
  </tr>
  <tr>
	  <td>switchport trunk native vlan [vlan-id]</td>
	  <td>Set native vlan on an trunk (The access port vlan is the "native vlan" so you only set native vlan for trunks)</td>
  </tr>
</table>

<table>
  <tr>
    <th colspan="2">VLAN (Routers)</th>
  </tr>
<tr>
	<th>Command</th>
	<th>Description</th>
</tr>
  <tr>
	<td>interface g0/0.10</td>
	<td>Create/edit subinterface .10 on g0/0 interface</td>
  </tr>
    <tr>
	  <td>encapsulate dot1q [vlan-id]</td>
	  <td>Assign vlan to the subinterface (don't forget to assign a ip address to each subinterface)</td>
  </tr>
</table>

<table>
<tr>
    <th colspan="2">MULTILAYER SWITCHES</th>
</tr>
<tr>
	<th>Command</th>
	<th>Description</th>
</tr>
<tr>
	<td>ip routing</td>
	<td>Enable Layer 3 routing on a multilayer switch (affects all ports)</td>
</tr>
<tr>
	<td>no switchport</td>
	<td>Turn interface from Layer 2 switchport to Layer 3 routed port (effects one port)</td>
</tr>
<tr>
	<td># interface vlan [vlan-id]
# ip address [ip address] [netmask]</td>
	<td>Create/edit a SVI
Assign an ip address to the SVI</td>
</tr>
<tr>
	<td># default interface [interface]</td>
	<td>(Routers) Reset interface to its default settings</td>
</tr>
<tr>
	<td colspan="2">note: No ROAS. Router and Multilayer switch are connected via a point-to-point network. The multilayer switch still has vlans and trunk/access ports configured the same(?) it's just that each vlan now also has an associated ip address(?)</td>
</tr>
</table>

<table>
<tr>
    <th colspan="2">DTP/VTP</th>
</tr>
<tr>
	<th>Command</th>
	<th>Description</th>
</tr>
  <tr>
	<td>switchport mode dynamic [negotiation mode (desireable/auto)]</td>
	<td>Set negotiation mode</td>
  </tr>
    <tr>
	  <td>show interfaces [interface] switchport</td>
	  <td>Show interface including negotiation mode, administrative/operating mode</td>
  </tr>
  <tr>
	  <td>switchport nonegotiate</td>
	  <td>Disable DTP negotiation on an interface</td>
  </tr>
  <tr>
	  <td>show vtp status</td>
	  <td>Show vtp status</td>
  </tr>
  <tr>
	  <td>vtp domain [name]</td>
	  <td>Create vtp domain with specified name</td>
  </tr>
<tr>
	<td>vtp mode [mode]</td>
	<td>Change vtp mode (client, transparent, server)</td>
</tr>
</table>

<table>
<tr>
    <th colspan="2">STP (Spanning-tree protocol)</th>
</tr>
<tr>
	<th>Command</th>
	<th>Description</th>
</tr>
<tr>
	<td>show spanning-tree</td>
	<td>Show spanning tree info
(Cost, role, status, priority number)
(Alternate = non-designated)
(Cost is only cost of that interface, not total cost)</td>
</tr>
<tr>
	  <td>show spanning-tree vlan [vlan-id]</td>
	  <td>Filter by vlan number</td>
</tr>
<tr>
	  <td>show spanning-tree detail</td>
	  <td>Show spanning tree info detailed
(Also shows total root cost)</td>
</tr>
<tr>
	  <td>show spanning-tree interface [interface] detail</td>
	  <td>Show detailed spanning-tree info for a specific interface</td>
</tr>
<tr>
	  <td>show spanning-tree summary</td>
	  <td>Show spanning tree info summary</td>
</tr>
<tr>
	  <td>spanning-tree portfast</td>
	  <td>Enable portfast on a port. Only works on access ports</td>
</tr>
<tr>
	  <td>spanning-tree portfast default</td>
	  <td>Enable portfast on all access ports on the switch</td>
</tr>
<tr>
	  <td>spanning-tree portfast trunk</td>
	  <td>Enable portfast on trunk link (Ex. ROAS, Virtualization server with VM)</td>
</tr>
<tr>
	  <td>show spanning-tree portfast disable</td>
	  <td>Disable portfast on a specific access port</td>
</tr>
<tr>
	  <td>spanning-tree bpduguard enable</td>
	  <td>Enable BPDU guard on specific interface directly</td>
</tr>
<tr>
	  <td>spanning-tree portfast bpduguard default</td>
	  <td>Enable BPDU guard on all portfast-enabled interfaces</td>
</tr>
<tr>
	  <td>shutdown
no shutdown</td>
	  <td>Re-enable port disabled by BPDU guard</td>
</tr>
<tr>
	  <td>spanning-tree mode [mode, (MST/PVST/rapid-pvst)]</td>
	  <td>Configure spanning tree mode (modern switches use rapid-pvst)</td>
</tr>
<tr>
	  <td>spanning-tree vlan [vlan #] root primary</td>
	  <td>manually configure primary root bridge</td>
</tr>
<tr>
	  <td>spanning-tree vlan [vlan #] root secondary</td>
	  <td>manually configure secondary root bridge</td>
</tr>
<tr>
	  <td>spanning-tree vlan [vlan #] priority 28672</td>
	  <td>You can also manually configure bridge priority to set primary/secondary
switch, but the root command just makes it easier so you don't have to remember numbers</td>
</tr>
<tr>
	  <td>spanning-tree vlan [vlan #] [cost/port-priority]</td>
	  <td>Manually configure root cost or port-priority</td>
</tr>
</table>

<table>
  <tr>
    <th colspan="2">RSTP</th>
  </tr>
<tr>
	<th>Command</th>
	<th>Description</th>
</tr>
  <tr>
	<td>spanning-tree portfast</td>
	<td>Enable Edge port (portfast) in RSTP</td>
  </tr>
    <tr>
	  <td>spanning-tree link-type point-to-point </td>
	  <td>Assign vlan to the subinterface (don't forget to assign a ip address to each subinterface)</td>
  </tr>
</table>


clients cannot make any configuration
You can make configurations on servers and the changes will be sent to clients
mode: Transparent switches can inherit the configurations of the other switches but then also make their own changes without affecting anything else in the network


<table>
  <tr>
    <th colspan="2">EtherChannel</th>
  </tr>
<tr>
	<th>Command</th>
	<th>Description</th>
</tr>
  <tr>
	<td>show etherchannel load-balance</td>
	<td>See current load-balancing method</td>
  </tr>
<tr>
	  <td>(config)# port-channel load-balance [method, ? to view available methods]</td>
	  <td>Set load-balance method (Ex. src-dst-mac)</td>
  </tr>
<tr>
	<td>(config-if)# channel-group [virtual int #] mode [mode]</td>
	 <td>Create/add interfaces to Port-channel</td>
</tr>
<tr>
	<th colspan="2">Note: channel-group number has to match for member interfaces on the same switch, but doesn't have to match the channel-group number on the other switch. i.e. Channel-group 1 on ASW1 can form an Ether Channel with channel-group 2 on DSW1</th>
</tr>
<tr>
	<td>channel-protocol ?</td>
	 <td>Manually configures EtherChannel negotiation protocol that member interfaces should use (PAgP, LACP). Although it should happen automatically</td>
</tr>
<tr>
	<td style="color:red">show etherchannel summary</td>
	 <td>Check EtherChannel flags for matching configurations</td>
</tr>
<tr>
	<td>show etherchannel port-channel</td>
	 <td>Show other information such as EtherChannel state, protocol, # of ports</td>
</tr>
</table>

no switchport: Layer 2 switch –> Layer 3 switch

port-priority is the tiebreaker in determining root port





%%at what age do you think parents should stop looking through their kids messages? (Texts, Insta DMs etc)%%

[[CCNA]]