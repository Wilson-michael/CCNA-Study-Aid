
## OSPF Study Guide part 3

# Overview

This will be the final portion of the study guide for OSPF. It will cover Loopback interfaces, OSPF network types, neighbor and adjacency requirements, and LSA types

# Loopback interfaces

A loopback interface is a virtual interface in the router. It is always up/up unless manually configured otherwise. It is not dependent on a physical interface, so it provides a consistent IP address that can be used to reach and identify a router. It is useful if a physical interface on a destination router goes down, as the path to the router is not dependent on a physical interface being available.

# OSPF Network Types

The term "network type", when in the context of OSPF, refers to the type of connection between OSPF neighbors (Ethernet, etc).
There are three main OSPF Network types, shown in the table below.

## OSPF Network Type Table
| Type           | Description                                                                                            |
|----------------|--------------------------------------------------------------------------------------------------------|
| Broadcast      | Enabled by default on Ethernet and Fiber Distributed Data Interface (FDDI) interfaces                  |
| Point-to-point | Enabled by default on Point-to-Point Protocol (PPP) and High-Level Data Link Control (HDLC) interfaces |
| Non-Broadcast  | Endabled by default on Frame Relay and X.25 interfaces.                                                |

# Broadcast Network Type
    
When using this type of network a Designated Router (DR) and Backup Designated Router (BDR) must be elected on each subnet, unless there are no OSPF neighbors, in which case there is only a DR. Routers in a subnet that aren't a DR or BDR become DROthers. 
To elect a DR and BDR, there is an order of priority.
1. Highest OSPF interface priority. 
2. By default, all OSPF interfaces have a priority of 1, so without manual adjustment the router with the highest Router ID becomes the DR.
If you want to change the OSPF priority of an interface, use the following commands: While in global config mode, pick the interface you want to change, then enter the command "ip ospf priority (value 0-255)". If you set the OSPF interface priority to 0, the router cannot be the DR or BDR for the subnet.
However, the designation of a router as either DR or BDR is non-preemptive, and will not change until either OSPF is reset or the interface fails or is shut down, etc.
When a DR goes down, the BDR then becomes the new DR, and an election is held for the next BDR.
In this network type, routers will only form a full OSPF adjacency with the DR and BDR of the segment, meaning that routers only exchange LSAs with the DR and the BDR. ALl routers will still have the same LSDB, but the amount of LSAs flooding the network will be reduced, reducing congestion on the network. Messages to the DR/BDR are multicast using 224.0.0.6, rather than the OSPF "all routers" multicast address of 224.0.0.5.

# Point-to-point Network Type

This type of network is enabled on serial interfaces using the PPP or HDLC encapsutlations by default. Both the PPP and HDLC are Layer 2 encapsulations, similar to Ethernet.            
Like the Broadcast Network type, routers dynamically discover neighbors using OSPF Hello messages on 224.0.0.5. Unlike the broadcast network, however, a DR and BDR are not elected. This is because as a Point-to Point connection, the two routers will form a full adjacency with each other. 
This type of network is the only instance on the CCNA where Serial Interfaces will be covered.
Basics of Serial Interface Configurations:
There are two ends of a serial connection, one of which functions as Data Communications Equipment (DCE), the other as Data Terminal Equipment (DTE). The DCE specifies the "Clock Rate" of the connection. 
1. In order to determine which interface is the DCE and which is the DTE, while in privileged exec mode, enter the command "show controllers (the specific interface you wish to check)".
2. In Global config mode, specify the serial interface you wish to configure. Then enter the command "clock rate (the desired value, the available choices for which can be found by entering '?' after clock rate)
3. By default, the encapsulation on a serial interface is set to HDLC. To change it, select the desired interface and use the command "encapsulation ppp". This encapsulation must match on both ends, or the interface will go down, as it would be like they were speaking two separate languages.

Note that you can manually configure the OSPF Network Type on an interface with the command "ip ospf network (type)". This is useful in certain instances, such as when  two routers are directly connected with an Ethernet links, eliminating the need for a DR/BDR. Also, not all link types can support all network types, e.g. a serial link and a broadcast network type (serial links don't support Layer 2 Frames).

# OSPF Neighbor Requirements

1. Area must match.
2. Interfaces must be in the same subnet.
3. OSPF process must not be shutdown.
4. OSPF Router IDs must be unique.
5. Hello and Dead timers must match.
To configure the timers, while in interface configuration mode, use the command "ip ospf hello-interval (value in seconds 1-65535)" and "ip ospf dead-interval (value in seconds 1-65535)"
6. Authentication settings must match.
To configure an authentication key for each interface, while in interface config mode, use the command "ip ospf authentication-key (desired password)" then enable authentication using the command "ip ospf authentication"
7*. IP Maximum Transmission Units (MTU) settings must match. The default is 1500 bytes, but this can be manually configured with the command "ip mtu (bytes value 68-1500)".
8*. OSPF Network type must match.

*If these settings don't match, the routers can become neighbors, but OSPF will not function properly.

# LSA Types
    
There are 11 types of LSAs, but only three are used for the CCNA, listed as follows:
Type 1 (Router LSA): Every OSPF Router generates this type of LSA. It identifies the router using its RID, and lists networks attached to its OSPF-activated interfaces.
Type 2 (Network LSA): This LSA is generated by the DR of each multi-access network, and lists the routers attached to said network.
Type 5 (AS External LSA): This type of LSA is generated by ASBRs to describe routes to destinations outside of the AS (OSPF domain).

This is the final portion of the OSPF Study Guide.


    
