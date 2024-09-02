## Spanning Tree Protocol (STP) Study Guide, Part 1

# Overview

This is a study guide on the topic of Spanning Tree Protoco (STP) for the purpose of the CCNA exam. It will cover[redundancy in networks](#redundancy_in_networks) and [Spanning Tree Protocol](#STP).

# Redundancy in networks

Redundancy in network design is critical in the modern world, as networks are expected to run 24/7 with no downtime. If a network goes down, even for a short time, it can prove disastrous. In order to ensure constant network availability, the implementation of redundancy at every possible failure point in the network is a must. One means of ensuring redundancy is having multiple routes between switches and routers on in and between networks, ensuring that if one piece of equipment fails, there is still a path for traffic to move through.

When you have layer 2 redundancies in your network, namely multiple switches connected together in with different paths, you can cause a major issue known as a _"Broadcast Storm"_. Say SW1 sends a broadcast frame ARP request out of f0/1 and f0/2 to SW2 and SW3, respectively. Each of those switches will flood the broadcast frame out of all ports except the one it was recieved on. Since we have redundancies built into our network, SW2 and SW3 are connected through their f0/3 ports, and will relay the broadcast frame through that port to each other. They will then flood the frame through all of their ports (except the one they received it on), including the ports connected to SW1. As layer 2 headers don't have a Time To Live (TTL) field, this will continue indefinitely. As ARP requests continue to be sent, the number of broadcast frames on the network will continue to grow, eventually congesting the network to a point where it is unable to allow any more traffic.

This isn't the only problem that comes from this scenario. Every time a frame arrives on a switchport, the switch uses the source MAC address of the frame to "learn" the MAC address and updates its MAC address table. Since frames with the same source MAC address will continue to arrive on different interfaces, the switch will continuously be updating the interface in its MAC address table, known as _"MAC Address Flapping"_. This increases the demand on the CPU, possibly overloading it, and can cause the switch to become "confused" about where data needs to go, resulting in dropped packets.

This is where STP comes in.

# STP

_Classic STP_, IEEE 802.1d, is the industry standard which all vendors run by default. It prevents Layer 2 loops by placing redundant ports in  the _blocking state_, making them a backup, disbled until an active (forwarding) port fails and they are needed to forward traffic.

When a port is in a forwarding state, it behaves normally, sending and receiving all normal traffic. When it is in a blocking state, it only sends and receives STP messages known as _Bridge Protocol Data Units (BPDUs)_

Note, the term _Bridge_ refers to a technology stage between Hubs and switches. These devices are not used in modern networks, but STP still uses the term as the name for a switch.

Switches enabled with STP send and receive Hello BPDUs out of all interfaces every two seconds by default. If a switch receives one of these messages on an interface, it knows that that interface is connected to another switch, as no other network devices use STP or BPDUs. 

In order to determine which interfaces are forwarding or blocked, switches use the _Bridge ID_ field of the BPDU to first determine the _root bridge (switch)_ for the network. This will be the switch with the lowest Bridge ID. The original Bridge ID field is made up of the 16 bit Bridge Priority (32768 by default) and the 48 bit MAC address. Since all switches have the same bridge priority, the lowest MAC address is the determining factor for the root bridge. The updated version of the Bridge ID field is slightly different, as the Bridge Priority portion is made up of a 4 bit Bridge Priority and a 12 bit Extended System ID (the VLAN ID). THis is because Cisco switches use a version of STP called _Per-VLAN Spanning Tree (PVST)_, which runs a separate STP 'instance' for each VLAN, meaning that in each VLAN different interfaces can be in forwarding/blocking states (e.g. f0/1 is in both VLAN 100 and 200, and is forwarding in 100 but blocked in  200). All ports on this switch will considered "designated" and placed in a forwarding state, and all other switches in the network topology must have a path to reach this switch.

If you want to change the Bridge ID in order to choose which switch is the Root Bridge, the only way to do so is to adjust the bridge priority in units of 4096, which is the value of the least significant bit of the bridge priority (0, 4096, 8192, 12288, 16384, 20480, 24576, 28672, 32768, 36864, 40960,45056, 49152, 53248, 57344, or 61440), which will then have the VLAN ID added to it to make the total bridge priority. 

When a switch is powered on, it will automatically assume that it is the root bridge, and will only give this status up when it receives a 'superior' BDU with a lower Bridge ID. Once the topology has converged and all of the switches agree on the root bridge, only the root bridge will send BPDUs, which will be forwarded by all other switches on the network. 

Now that we've gone through all of that, it's time to actually go through the process of STP.

1. The root bridge is determined through the lowest bridge ID, and all ports on this switch are set as designated ports.

2. Each of the remaining switches will select one of its interfaces to be its _root port_ by determining which interface has the lowest _root cost_. This port will be in a forwarding state. Root cost is determined based on the speed of a port (10 Mbps has an STP cost of 100, 100 Mbps is 19, 1 Gbps is 4 and 10 Gbps is 2) and the cost of the connected interface. Say that SW1, SW2 and SW3 are all connected through 1 Gigabit interfaces. SW1 has the lowest Bridge ID, thus is the Root bridge. All of its ports have a root cost of 0. The interfaces connecting SW1/SW2 and SW1/SW3 all have a root cost of 4 (0+4), whereas the interface connecting SW2 and SW3 has a root cost of 8 (4+4). Both SW2 and SW3 will designate the the SW1 connected interface as their root port. 

If a switch has multiple ports with the same root cost, then the root port will be selected using the lowest neighbor Bridge ID. 

If both the Root Cost and the neighbor Bridge ID are equal, then the lowest Neighbor Port ID will be used. This is determined by adding the port priority (128 by default) and the port number, meaning that the lowest NEIGHBOR port number will dictate the port used.

Note that any ports connected to another switches root port must be designated, as it is the path to the root bridge and must not be blocked.

3. Determine which unused ports in any collision domain are to be 'designated' in case of port failure.

The first means of determination is the switch with the lowest root cost will make its ports designated.

If the root cost of both switches is the same, then the switch with the lowest bridge ID will make its ports designated.

The other switch wiil then make its ports 'non-designated' (blocked).

This completes the first portion of the STP study guide.

