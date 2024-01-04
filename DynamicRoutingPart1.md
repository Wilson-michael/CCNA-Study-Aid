## First Study Guide for the Dynamic Routing portion of the CCNA

# Overview
  This will provide a basic look at the process of and reasoning behind dynamic routing in a network

# Dynamic vs Static Routing
  Static routing is performed by the networking professional, manually assigning dedicated routes between end-points on a network. If anything happens to a path to a network, routers will be unaware, and will continue forwarding packets along that path, and they will no longer reach that network.
  Dynamic routing, however, is done by configuring a dynamic routing protocol (DRP) on the router, allowing the router itself to determine the best routes to the destination network. If a new LAN is added, routers will automatically let each other know about how to get to that network. If one path to a network becomes unavailable, the routers will switch to the next best available path. This option does require redundancy in connections between routers.

# Network Routes vs Host Routes
  When referring to a "Network Route", all that is meant is a route to a network or subnet which has a mask length less than /32. 192.168.1.0/24 is a Network Route. If you have a route that is something like 10.0.12.1/32, it is what is known as a "Host Route". This is a route to a specific host, a single address with a /32 mask. 
  To configure a host route, use the command _ip route (host ip address) 255.255.255.255_

# Overview of Dynamic Routing
  In order to determine which routes are preferred, routers use DRP's to advertise the information they know about their routes to other routers by forming 'adjacencies' / 'neighbor relationships' / 'neighborships' with adjacent routers that they are directly connected to. They then use a similar process to STP, basing decisions on route costs.
  There are two main categories of DRP's. The first is an Interior Gateway Protocol (IGP). The second is Exterior Gateway protocol (EGP)
  IGP- IGP's are used within a single Autonomous System (AS), which is a single organization, such as a company or a hospital. There are two different types of algorithms that can be used with IGP's, listed below.
    Distance Vector- These algorithms were invented in the early eighties. They work by sharing their known destination networks and their metrics to those networks with their directly connected neighbors, also known as "Routing by Rumor", because each router only knows it's own information and that of its neighbors. The two DVP's are 1: Routing Information Protocol (RIP) and 2: Enhanced Interior Gateway Routing Protocol (EIGRP).
    Link State- When using a link state routing protocol, every router creates a connectivity map of the network. To make this happen, each router advertises infomation about its connected networks to it's neighbors, with this info being passed along to each router in the network until they all have developed the same "Map". Each router then uses their map to independently calculate the best route to each destination. This process does use more resources on the router, but tend to be faster than distance vector protocols in reacting to changes on the network. The two types of LSP's are 1: Open Shortest Path First (OSPF) and 2: Intermediate System to Intermediate System (IS-IS).
  EGP- EGP's are used to share routes between different AS's. Where IGP's use multiple types of algorithms,EGP's use not only one type (Path Vector), but only one algorithm at all is used in modern networks: Border Gateway Protocol (BGP) 
# Metrics
  Much like STP, DRP's use metrics to determine which connections have the lowest cost. The table below lays out how each protocol determines cost.
  
  |IGP|Metric|Explanation|
  |---|------|--------|
  |RIP|Hop Count|Each router in the path counts as one hop. The total number of hops is the cost. All link speeds are equal costs|
  |EIGRP|Bandwidth and Delay|Comprised of a complex formula that can take into account many values, the default uses the bandwidth of the slowest link and the total delay of all the links in the route|
  |OSPF|Cost|Calculated off of bandwidth. The total metric is the sum of all costs in the route. THe higher the bandwidth of the link, the lower the cost|
  |IS-IS|Cost|The total metric is the sum of all link costs in the route. The cost of each link is not automatically calculated by default, and all links have a cost of 10 by default.|
  
  
  If redundant routes between devices have the same cost, the exact same destination, the same network address and the same prefix length, they will both be added to the routing table, and traffic will be load-balanced over both routes. This is called Equal Cost Multi-Path (ECMP) Load-Balancing. 
  In most cases, an entity will only use one IGP (usually OSPF) for their network. However, on the rare occasion that they might use two, such as when two companies connect their networks to share information and they each use a different protocol, the metrics cannot be compared. In this case, they use the Administrative Distance (AD) to determine which protocol is preferred. The lower the AD, the more likely the protocol is to select good routes. The table below displays the AD of most common protocols.

  |Route protocol/type|AD|
  |-------------------|--|
  |Directly Connected|0|
  |Static|1|
  |External BGP (eBGP)|20|
  |EIGRP|90|
  |IGRP|100|
  |OSPF|110|
  |IS-IS|115|
  |RIP|120|
  |EIGRP (external|170|
  |Internal BGP (iBGP)|200|
  |Unusable Route|255|

  You can change the AD of a routing protocol, say, if you want the router to prefer one protocol over another, OSPF over EIGRP, for example. You can also change the AD of a static route by using the global config command _ip route (destination ip address, subnet mask, next hop) (1-255, setting the AD for this route)_ YOu would do this to make it less preferred than a dynamic route. This is called a "Floating Route", and it will be inactive unless the route learned by the DRP is removed.
