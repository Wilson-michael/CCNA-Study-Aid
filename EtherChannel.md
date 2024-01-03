## EtherChannel Study guide

# What is EtherChannel used for
  While Spanning Tree Protocols are incredibly useful for preventing loops in your network, they can cause issues when it comes to dealing with oversubscription between Access Layer Switches (AS) and Distribution Layer Switches (DS0. No matter how many links you add between switches, when STP is in place, only one link will be functioning at a time. This is am important function, as there would eventually be a broadcast storm between the two, and all network functionality would cease. EtherChannel deals with this by combining all of the physical interfaces into one logical interface. STP will treat this as a single interface, meaning we can maintain physical redundancy while preventing broadcast storms and increasing available bandwidth. Other names for this logical interface are the Port Channel or the Link Aggregation Group (LAG)
  Traffic that uses the EtherChannel will be load balanced among the physical interfaces of the group. Which specific interface is used is algorithmically determined.

# How does EtherChannel Load Balance
  Load balancing over EtherChannel is based on communications between two nodes in a network, also referred to as a "flow".
  A brief example could be as follows. You have a network consisting of six PC's connected to AS1, which is has four physical links to DS1,  which is connected to a server and a printer. PC1 wants to communicate with the Server. It forwards several frames to AS1, which then forwards the frame over the Port Channel to DS1 using the servers MAC address. The switch determines that the third interface will be used to forward the frame. If PC1 continues to send frames to the server, these are considered to be in the same "Flow", and they will continue to be forwarded on the same physical interface, which balances the bandwidth usage and keeps frames in the correct order. Now, PC1 wants to communicate with the printer. AS1 will forward the frames along another physical interface, interface 1. Then, PC2 wants to print something. AS1 determines that using interface 2 is the best choice for this function. This balances traffic to and from the printer, reducing congestion on the network.
  Which interface is used for which flow is calculated using several different factors, which can be changed. They are as follows:
    1- Source MAC. All traffic with a set Source MAC address will use a single physical interface
    2- Destination MAC. The same as above, but for a set Destination MAC.
    3- Source and Destination MAC- Uses both MAC addresses to set a path.
    4- Source IP
    5- Destination IP
    6- Source and Destination IP

# Configuring an EtherChannel
  To see the current configuration, in privileged exec mode, enter the command _show etherchannel load-balance_. This will display both general and per-protocol configurations.
  To change the configuration, enter global configuration mode, then use the command _port-channel load-balance_, followed by the desired configuration, e.g. _src-dst-mac_
  Remember, etherchannel is the command used in the CISCO CLI to view the configuration, while port-channel is used to configure.

# Methods of EtherChannel configuration on CISCO switches
  There are three different methods of configuration 
    1- Port Aggrecation Protocol (PAgP): A CISCO proprietary protocol, it dynamically negotiates the creation and maintenance of EtherChannels between CISCO switches. Much like DTP does for trunking, it sends frames to the neighboring switch to determine whether or not it wants to form and EtherChannel.
    2- Link Aggregation Control Protocol (LACP): The industry standard protocol (IEEE 802.3ad), it performs the same function as PAgP, but can be used to be formed EtherChannels between switches from different manufacturers. This makes LACP the preferred method of EtherChannel configuration.
    3-Static EtherChannel: The switch doesn't use a protocol to determine if an EtherChannel should be formed, rather, interfaces are statically configured to form and EtherChannel. This is not typically used, because you want your switches to be able to dynamically maintain the EtherChannels in the case of interface failures.
  Up to 8 interfaces can be formed into a single EtherChannel (though when using LACP, up to 16 are allowed, with 8 active and 8 reserved in standby in case of active interface failure)
