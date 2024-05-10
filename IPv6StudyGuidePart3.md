## IPv6 Study Guide Part 3

# Overview

This is the final portion of the IPv6 study guide for the CCNA Exam. It will cover the [IPv6 Heaader](#ipv6-header), [Solicited-Node Multicast Address (SNMA)](#Solicited-node-multicast-address), [Neighbor Discovery Protocol (NDP)](#ndp), [SLAAC](#stateless-address-auto-configuration-(slaac)), and [IPv6 Static Routing](#ipv6-static-routing).

# IPv6 Header

Compared to an IPv4 header, the header fo IPv6 is much simpler. For one thing, rather than the size of the header being variable, IPv6 has a fixed header format with a size of 40 bytes, and a field for the payload length, but no header length field like in IPv4. The processing of IPv6 headers is easier for routers, meaning that performance is generally improved. First, as in the IPv4 header, is the version field, which has a fixed value of 6 (0b0110) that indicates IPv6. Next is the traffic class field. It consistsof 8 bits, and is used for Quality of Service (QoS) to indicate high-priority traffic, such as IP phone calls and live video calls, which will have a higher Traffic Class value than other traffic. Then comes the Flow Label field,made up of 20 bits and used to identify specific traffic flows, or communications between specific sources and destinations. Next is the Payload length field. With a length of 16 bits, it indicates the length of the encapsulated layer 4 segment (the payload) in bytes. The length of the header itself isn't included, as it is always 40 bytes. After that is the 'next header' portion, made up of 8 bits. It indicates the type of the 'next header' of the encapsulated segment, e.g. TCP or UDP. It serves the same purpose as the IPv4 Protocol field. The next field is the Hop limit field. It is made up of 8 bits, and the value in the field is decremented by 1 by each router that forwards it, and if it reaches 0, the packet will be discarded. It has the same function of the TTL field in the IPv4 header. Lastly, the source and destination address fields. They are 128 bits in length each.

# Solicited-Node Multicast Address

An IPv6 SNMA is calculated from a unicast address. It begins with a fixed prefix of ff02:0000:0000:0000:0000:0001:ff (properly shortened to ff02::1:ff) followed by the last 6 hex digits of the unicast address that is generating the solicited node address. Below is an example.
2001:0db8:0000:0001:0f2a:4fff:fea3:00b1. a3:00b1 would be used to create the SNMA: ff02:0000:0000:0000:0000:0001:ffa3:00b1, or ff02::1:ffa3:b1.

# NDP

NDP is a protocol used with IPv6 in place of ARP from IPv4. It uses ICMPv6 and SNMAs to learn the MAC addresses of other hosts, rather than the broadcast message used by ARP. As SNMA messages are directed towards a specific host, rather than broadcasted to all hosts, it is much more efficient. There are two types of messages used. First off is the Neighbor Solicitation (NS), or ICMPv6 Type 135. It is the equivalent of an ARP request. The second message is ICMPv6 Type 136, the Neighbor Advertisement (NA), which replaces the ARP Reply message. Say you wanted to ping another host on your network. Your router needs to create an ethernet header, and to do so, it needs the MAC address of the router it will be forwarding to. By using NDP, the first router would calculate the SNMA of the second router bu taking the last six hex characters of the IPv6 address and adding them to ff02::1:ff, then it would send an NS message to that address, containing the source IP interface, the destination IP (R2s SNMA), the MAC address of the source interface, and the destination MAC, which is the Multicast MAC based on R2's Solicited-Node address. Router 2 then replies with it's Neighbor Advertisement message, which contains the source IP of the interface sending the message, the destination IP of the interface on R1 that sent the NS message, The source MAC of R2s interface and the destination MAC of R1s interface.
Whereas IPv4 creates an ARP table through requests and replies, IPv6 creates an IPv6 neighbor table, which can be seen using the command "show ipv6 neighbor". This process is industry standard across all devices, Cisco or otherwise. 
Another function of NDP is allowing hosts to automatically discover any routers on the local network. This is done through the use of two types of messages: Router Solicitation (RS), aka ICMPv6 Type 133 and Router Advertisment (RA), or ICMP Type 134. RS messages are sent out whenever an IPv6 interface is enabled or an IPv6 host is connected to the network. They go to the multicast address of FF02::2, the all routers address, and ask all routers on the local link to ID themselves. 
All routers that receive the RS message then send out an RA. This message is sent to multicast address FF02::1, the all nodes address. With this message, the router announces its presence, along with other information about the link. While the reception of an RS will cause a router to send an RA, they will periodically send them out unprompted.

# Stateless Address Auto-configuration (SLAAC)

This is another way to configure IPv6, this time automatically. Whereas when you manually configure the IPv6 address using EUI-64 you have to input the prefix and prefix length yourself, by using the command "ipv6 address autoconfig" you allow the device to generate the IPv6 address from the RS/RA messages used in NDP using either EUI-64 or random generation, depending on the device.

# Duplicate Address Detection (DAD)

This function of NDP allows hosts to see if other devices on the local link are using the same IPv6 address. DAD is performed any time an IPv6 interface initializes using the "no shutdown" command, or an IPv6 addreess is configured on an interface by any method. DAD uses NS and NA. It sends an NS to its own IPv6 address, and if it doesn't get a reply, it knows that the address is unique. If, however, it does receive an RA, it means another host on the network is already using the address.

# IPv6 Static Routing

IPv6 routing works the same as IPv4 routing. A router receives a packet on an interface, it looks up the destination address in its routing table, then forwards it according to the most specific match in the table. However, IPv4 and IPv6 routing processes and tables are separate on the router. IPv4 is enabled by default, while IPv6 is disabled by default, and must be enabled with the command "ipv6 unicast-routing". If it is not enabled, the router will be able to send and receive IPv6 traffic, but not route it (forward it between networks). There are multiple IPv6 routes in the routing table, listed here.

1: Network Route. These are automatically added for each connected network.

2: Host Route. These are automatically added for each address configured on the router.

3: Static Routes: There are multiple types of IPv6 static routes, and they are determined by how you configure them, as shown below.

# Configuring IPv6 Static Routes

Ciscos command for configuring a static IPv6 route is as follows:

"ipv6 route (destination/prefix length){next-hop | interface [next-hop]}[ad]"

In Cisco documentation {} indicates a required choice, in this instance either a next-hop address or an exit-interface with an optional next-hop address, optional being indicated by the []. This means that the AD is optional too.
The type of static route configured is determined by what you choose to input. 

First is the directly attached static route, where only the exit interface is specified. Note, for Cisco routers, this command cannot be used if the interface is an Ethernet interface.

Here is an example of this configuration : "ipv6 route 2001:db8:0:3::/64 g0/0"

Second is a Recursive static route, where only the next-hop is specified. The reason for the designation recursive is due to the fact that during this process, the router must look up its routing table multiple times (a recursive lookup) to find the destination, then the next hop. 

Here is an example Recursive static route configuration command: "ipv6 route 2001:db8:0:3::/64 2001:db8:0:12::2"

Third is the Fully Specified static route, where both the exit interface and the next-hop are specified.

Here is an example config command: "ipv6 route 2001:db8:0:3::/64 g0/0 2001:db8:0:12::2"

Finally, the floating static route. This is configured by raising the AD to be higher than the AD of the routing protocol used in the network. Always set the AD to higher than the main route.

If you want to use a link-local address as the next-hop, you must specify the exit interface, aka a fully specified static route. This is due to the fact that a router cannot figure out on its own which interface a next-hop link local address is connected to.
