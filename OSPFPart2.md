## Study Guide for OSPF part 2

# Overview
    
This study guide will go more in-depth on the subject of Open Shortest Path First dynamic routing protocol. It will cover the OSPF metric and how to configure it, becoming OSPF neighbors, and some additional OSPF configurations

# OSPF Metric

The OSPF metric is called "Cost". It is automatically calculated using the bandwidth of the interface. The formula for the calculation is the value of the reference bandwidth divided by the interfaces bandwidth. The default value of the reference bandwidth is 100 mbps. As in the other routing protocols, the lower the cost of the interface, the higher the preference for routing that way.
    Here is a table on the default costs for different interface speeds

## OSPF Cost Table
| Reference Speed | Interface Speed | Cost |
|-----------------|-----------------|------|
| 100 mbps        | 10 mbps         | 10   |
| 100 mpbs        | 100 mbps        | 1    |
| 100 mbps        | 1000 mbps       | 1    |
| 100 mbps        | 10000 mbps      | 1    |

As you can see, in OSPF, all values below 1 are automatically rounded up to 1.Obviously this is slightly confusing and less than ideal. You can and should change the reference bandwidth using this command.
1. While in OSPF router config mode, enter the command "auto-cost reference-bandwidth (megabits per second)" Below is a table showing costs when the reference value is set to 100000 megabits per second

## Adjusted OSPF Cost Table
| Reference Speed | Interface Speed | Cost  |
|-----------------|-----------------|-------|
| 100000 mbps     | 10 mbps         | 10000 |
| 100000 mpbs     | 100 mbps        | 1000  |
| 100000 mbps     | 1000 mbps       | 100   |
| 100000 mbps     | 10000 mbps      | 10    |

All routers in the area should have the same reference badwidth to ensure proper routing.
You should always configure your reference bandwidth to be greater than the fastest links in your network to allow for future upgrades.
A note on loopback interfaces: they always have a cost of one.

To manually configure the cost of a specific interface, use the following commands:
1. While in global config mode, enter "interface (interface id)"
2. Enter command "ip ospf cost (value 1-65535, e.g. 1000)"

Another option to change the OSPF cost of the interface is to change the bandwidth of the interface with the "bandwidth" command: in interface configuration mode "bandwidth (value 1-10000000 kilobits per second)". This does not change the speed at which the interface operates, it just changes the value used by the OSPF or EIGRP formulas. To change the speed at which an interface operates, use the "speed" command. Changing the bandwidth value of an interface for use in altering the OSPF/EIGRP interface cost is not recommended, as this value is used in other calculations.

To find the cost of each interface on the router, while in priveleged exec mode, use th command "show ip ospf brief".

# OSPF Neighbors

The main task in configuring and troubleshooting OSPF is making sure that routers can become OSPF neighbors, after which they will automatically do the work of sharing network information, calculating the best routes, etc. 
When OSPF is activated on an interface, the router sends "hello" messages out of the interface at regular intervals determined by the "hello timer", which is set to 10 seconds on an Ethernet connection by default. These messages are multicast to 224.0.0.5, which is the multicast address for all OSPF routers.
OSPF messages are encapsulated in the protocol field of an IP header, with a value of 89.

For OSPF routers to become neighbors, they have to go through various states, listed as follows.

1. R1 (10.0.12.1) has OSPF enabled on its G0/0 interface, and sends a "hello" message containing its RID (1.1.1.1 in this instance) and any neighbor RIDs to R2 (10.0.12.2). At this point, R1 knows no neighbors, so neighbor RID will be 0.0.0.0, and its current neighbor state is "down".
2. R2 receives the "hello" message from R1 and adds its RID to its neighbor table. Its relationship with R1 is now in the "init" state. The "Init" state means that while R2 knows R1s RID, R1 doesn't know R2s.
3. R2 sends a "hello" message containing its RID (2.2.2.2) and its neighbor RIDs (1.1.1.1) to R1. This will cause R1 to insert R2 into its OSPF neighbor table in a "2-way" state, meaning the router has received a message containing its own RID.
4. R1 then sends another "hello" containing its RID (1.1.1.1) and neighbor RID (2.2.2.2) to R2, which then converts R1s neighbor status to the "2-way" state. 
Once both routers have reached the 2-way state, it means that the conditions have been met to become OSPF neighbors, and are ready to share LSAs to build their LSDBs. In certain network types, a Designated Router (DR) and Designated Backup Router (DBR) are selected. This will be covered in the next OSPF Study Guide.
5. The routers will then determine which of the two will initiate the exchange of information about their LSDBs while in the "Exstart" state. One of the routers will be designated the "Master" and the other the "Slave". This designation will only be used in this process. These designations are determined by exchanging Database Description (DBD) packets. These packets show which router has the higher RID, in this case R2.
6. The routers then enter the "Exchange" state, where they exchange DBDs that contain a list of all the LSA's in their LSDBs. They don't contain any detailed information about the LSA's, just the basics. The routers then compare the data from the DBD to the information in their own LSDB to determine which LSAs they need their neighbor to share.
    7. The "Loading" state means that the routers will send Link State Request (LSR) messages to their neighbors for any LSAs they don't have in their LSDB. Any requested LSAs are then sent in a Link State Update (LSU) message. Then the routers reply with Link State Acknowledged (LSAck) to acknowledge that they recieved the LSA's. 
    8. The final state of OSPF Neighbors is the "Full" state. This is when the routers have a full OSPF adjacency and identical LSDBs. The routers maintain the neighbor adjacency by continuing the process of sending and listening for "hello" messages every 10 seconds. Every time a "hello" packet is received, the "Dead timer", set at 40 seconds by default, is reset. If the Dead timer reaches 0 without receiving a "hello", the neighbor is removed. If the neighbors remain "up", they will continue to share LSAs with any changes to the network to ensure that each routers LSDB is complete and accurate.

To give a simplified overview, below is a table with each state and its meaning/fuction.

## OSPF State Table
| State    | Function                                                                                           |
|----------|----------------------------------------------------------------------------------------------------|
| Down     | No neighbors in neighbor table, hello message is sent                                              |
| Init     | One router has received a hello, but its RID is not in the message                                 |
| 2-way    | Both routers have receive hellos with both their own RID and their neighbors                       |
| Exstart  | Routers determine which router will intitiate the exchange of LSAs                                 |
| Exchange | Routers exchange DBDs that contain a basic list of the LSAs in their LSDBs                         |
| Loading  | Routers send requests for missing LSAs, reply with LSUs containing LSAs, and end by sending LSAcks |
| Full     | Routers have full OSPF adjacency and identical LSDBs. They maintain this with Hellos and new LSAs  |

# More OSPF Configurations

Enabling OSPF on an interface directly:
1. From global config mode, enter the command "int (the interface you wish to configure OSPF on)
2. Enter the command "ip ospf (process-id) area (area number)"
3. Repeat with all interfaces you want configured with OSPF

Configuring Passive Interfaces
1. While in interface config mode, enter the command "router OSPF (process-id)"
2. Enter the command "passive-interface default"
3. To choose which interfaces you want to be active, enter the command "no passive-interface (interface id)", and repeat for all desired interfaces


    
