## Overview

    A Study guide on the basics of Open Shortest Path First (OSPF)protocol

# What is OPSF

    OSPF is the most common Link State IGRP used in networking. Link State Routing Protocols create a "connectivity map" of its network, passing that map along to each of it's neighbors until all routers in the network have the same complete map. They each use this map to determine the best paths in the network. OSPF uses the "Shortest Path First" algorithm, also known as "Dijkstra's Algorithm", named for it's creator, Edsger Dijkstra. 
    There are three versions of OSPF. OSPF1 was created in 1989, and is not in use anymore. OSPF2 was released in 1998, and is the version typically used for IPv4. OSPF2 is also the version covered in the CCNA Exam. OSPF3 was released in 2008, and can be used for both IPv6 and IPv4, though OSPF2 is more common.
        Basic Operations of OSPF
        Routers store information about the network in "Link State Advertisements" (LSA's), which are organized in structures called "Link State Databases" (LSDB). 
        Routers will flood LSDA's until all routers in the OSPF area have the same LSD. If a new network is discovered on a routers interface and OSPF is enabled, it will send out an LSDA containing information about the network and interface to its neighbors, such as its Router ID (RID), its IP address, and its cost. Each LSA has an "Aging timer" set to 30 minues by default, which tells the router to re-flood the LSA after the timer has ended. 
        
        OSPF Areas
        OSPF uses "areas" (routers and links that share the same LSDB) to divide up a network. In smaller networks, a single area isn't an issue. 
        Large Networks using a Single Area, however, can create problems. For example, the algorithm requires more time and exponentially more computing power on each router, as well as needing more memory for the LSDB, which has to be refreshed each time the aging timer expires. If you divide the network into separate areas, you avoid these problems. There are some requirements to implement separate areas on a network. First, all of the areas must connect to area "0", or the "backbone area". When routers share the same area on all interfaces, they are called "internal routers". Routers that have interfaces that are on different areas are called "Area Border Routers" (ABR's). Each ABR contains a separate LSDB for each area it is connected to, so in order to not overburden the router, it is recommended that you only have a maximum of 2 areas connected to a router. "Backbone routers" are routers that are connected to the backbone area. An "intra-area route" is a route to a destination in the same area, whereas an "interarea route" is a route to a destination in a separate area.
        Rules for OSPF Areas;
        1. Areas must be contiguous, meaning that each individual areas should be connected, not split up.
        2. All areas must have at least one ABR connected to the Backbone Area.
        3. All OSPF interfaces on the same subnet must be in the same area.

# Basic OSPF Configuration Commands

    Enabling OSPF on specified interfaces:
        1. In global configuration mode, enter the command "router ospf" followed by a process id. 
            While routers can use multiple processes, the typical configuration is to use just one, meaning that the simplest command is usually "router ospf 1". Routers can become OSPF neighbors with different process IDs, unlike EIGRP which requires matching AS numbers.
        2.Enter command "network (ip address)(wildcard mask)(area id)" e.g "network 10.0.12.0 0.0.0.3 area 0". 
            For the CCNA, only single area configurations are used, and it's considered best practice to use "area 0". Just like in RIP and EIGRP, the "network" command tells OSPF to look for any interfaces in the specified IP address range and activate OSPF on the interface in the specified area. The router then tries to become OSPF neighbors with other OSPF activated neighbor routers.

    Passive interface commands:
        1. In order to prevent interfaces unnecessarily sending "hello" messages, while in router configuration mode enter the command: "passive_interface (interface id).
            Even though the router will no longer send "hello" messages out of that interface, it will continue to send LSA's about the network connected to that interface to other routers.

    Commands to advertise a default route into OSPF
        1.After configuring a default gateway on a router, while in router configuration mode enter the command "default-information originate". This will cause the router to create an LSA containing the information for the default route to neighboring routers.

    Configuring a manual router id
        Just as in EIGRP, the router id is set by order of priority (manual id, highest ip on a loopback interface and higher ip on a physical interface). 
        1. To manually configure an interface id, while in router config mode, enter "router-id (a.b.c.d, e.g. 1.1.1.1)
        2. In order to make this take effect, either reboot the router, or, while in priveleged exec mode, enter the command (clear ip ospf process), then type "yes". This resets the OSPF on the router, which is not what you want to do on an actual network, as it will mean that the router cannot communicate with its neighbors until it repopulates its LSDB. 


# Information on OSPF enabled routers

    1. An "Autonomous System Boundary Router" (ASBR) is an OSPF router that connects the OSPF network to the internet.
    2. Unlike EIGRP, OSPF doesn't support Unequal cost load balancing. It does, however, support equal cost multi path load balancing over a maximum of 4 paths by default. You can change the maximum number of paths by using the "maximum-paths" command in router config mode.
    3. The default AD for OSPF is 110. If you want to change it, use the command "distance (1-255)" in router config mode. This is useful when you want to prioritize OSPF over something like EIGRP.

This is a basic overview of OSPF. More will follow.
