## Dynamic Routing Study Guide Part 2

# Overview
This study guide will focus on the RIP and EIGRP aspects of dynamic routing
    
# General guide to the protocols
RIP- Routing Information Protocol is an industry standard Distance Vecotor protocol. It uses Hop count as it's metric. The maximum hop count is 15, meaning it is useless for large networks. The three versions of RIP are RIPv1 and RIPv2, used for IPv4, and RIPng (RIP Next Generation), used for IPv6. It uses two message types. 
1: Request- to ask RIP enabled neighbor routers to send their routing table
2: Response- To send their routing table to neighboring connected routers

RIPv1 vs RIPv2. RIP1 only advertises Classful addresses, meaning it doesn't support VLSM or CIDR, and doesn't include subnet mask information, so it is not useful in modern networking. All messages are broadcast to 255.255.255.255. RIPv2 supports VLSM and CIDR, as well as including the subent information in advertisments. All messages are multicast to 224.0.0.9, meaning that messages are only delivered to devices that have joined that specific multicast group. RIPv2 is what you will always use, IF you are using RIP, which is not the norm.
EIGRP- Enhanced Interior Gateway Routing Protocol was a CISCO proprietary standard, but they have since published it openly to be used on other vendors equipment. It's considered and advanced distance vector protocol, being much faster than RIP in reacting to changes in the network and eliminating the 15 hop-count limit of RIP, making it much more suitabel for large network applications. It is the only IGP that can perform unequal-cost load balancing, meaning it will distribute traffic based on the bandwith of the interface, rather than just equally distributing it across all of the paths (ECMP load balancing) 

# RIP Configuration
Commands are as follows
1. In global cofig mode, enter the command "router rip"
2. Enter the command "version 2"
3. Enter command "no auto-summary" (auto summary aoutomatically configures networks as classful, which is not what we want)
4. Enter command "network (network ip address, e.g. 10.0.0.0)" 
A note on the command "network". It is classful, and will automatically convert to classful networks, in this case looking for networks that fall under 10.0.0.0/8 meaning that adresses such as 10.0.12.1/30 and 10.0.13.1/30 will be treated as if they are in the same class A netork, since only the first 8 bits need to match. It tells the router to look for interfaces with an IP address that is within the specified range, activate RIP on those interfaces, form adjacencies with connected RIP neighbors and advertise the network prefix of the interface. Since we are using version 2, the protocol tells the router to advertise the network prefixes of the aforementioned interfaces,not the classful address specified by the "network" command This command funtions the same way across OSPF and EIGRP protocols.
5. Repeat this command for any other networks on the router.
If there are networks on the router that are not connected to any other routers, they should be configured as a passive interface by using the command "passive-interface (interface number)". This tells the routher to stop sending RIP advertisements out of that interface. The router will, however, continue to advertise those addresses to its RIP neighbors. As before, this command functions the same across OSPF and EIRGP.
One additional function of RIP is as follows. If you have an interface configured to connect to the internet on a router, e.g. "ip route 0.0.0.0 0.0.0.0 203.0.113.2", the following command allow the router to advertise that interface to its neighbors. 
1. While in RIP config ("router rip"), enter the command "default-information originate". This means the router will share the route to its RIP enabled neighbors, which will share it to theirs, creating a route to the internet on all connected routers.

Another useful command is "show ip protocols" while in priveleged exec mode. This displays which protocols are enabled on the router, some of the timers used to operate the protocol, information on the version used, the maximum path on ECMP load balancing (which can be changed), which networks are being routed on the device, if there are any passive interfaces, the gateways used and the AD. 

# EIGRP configuration
1. In global config mode, enter the command "router eigrp (Autonomous System Number, e.g.1. This number must match across all routers, otherwise they won't form adjacencies and share their information)
2. Enter command "no auto-summary"
3. Configure any passive interfaces using the command "passive-interface (interface number)
4. Configure interfaces using the command "network (network ip address). You can either specify a mask or leave it to be assumed as a classful network. To specify a mask, enter the command as follows: "network (network ip address, e.g. 172.16.1.0) (wildcard mask, e.g. 0.0.0.15)"
A wildcard mask is basically an inverted subnet mask, indicating how many bits from each octet are host bits, rather than network bits indicated in a standard subent mask. The above example is a /28 network, binary 11111111.11111111.11111111.11110000. The wildcard mask binary for this is 00000000.00000000.00000000.00001111. An easy shortcut for conversion is to subtract the value of each octet from 255. When you have a 0 in the wildcard mask, it means that the bits must mactch between the EIGRP network address and the interfaces ip address. When you have a 1, they do not. Below is an example.
R1 G/10 IP address: 172.16.1.14 (10101100.00010000.00000001.00001110)
EIGRP network command: 172.16.1.0 (10101100.00010000.00000001.00000000) 0.0.0.15 (00000000.00000000.00000000.00001111)
Since all the 0 bits in the EIGRP address are matched with 0 bits in the interface IP address, EIGRP can be activated on the interface.
Wildcard masks are also used by OSPF.

When the "show ip protocols" command is used in EIGRP, there are a few differences in the information shown. In addition to displaying most of the same information as RIP (protocol used, auto-summarization, maximum path, routing for networks, passive interfaces  routing information sources and internal/eternal ADs), it also displays the metric weights (which can be changed), the Router ID which is used to identify the router to other routers on the AS. There are three ways that the Router ID is determined, prioritized as follows: 
1- A manually configured Router ID. 
2- The highest IP address on a loopback interface within the router. 
3- The highest IP address on one of the routers physical interfaces. 
While this number looks like an IP address, it is not. It can be any 32 bit number.
    
This covers the basics of RIP and EIGRP
