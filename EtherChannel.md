## EtherChannel Study guide

# What is EtherChannel used for
  
While Spanning Tree Protocols are incredibly useful for preventing loops in your network, they can cause issues when it comes to dealing with oversubscription between Access Layer Switches (AS) and Distribution Layer Switches (DS). No matter how many links you add between switches, when STP is in place, only one link will be functioning at a time. This is am important function, as there would eventually be a broadcast storm between the two, and all network functionality would cease. EtherChannel deals with this by combining all of the physical interfaces into one logical interface. STP will treat this as a single interface, meaning we can maintain physical redundancy while preventing broadcast storms and increasing available bandwidth. Other names for this logical interface are the Port Channel or the Link Aggregation Group (LAG)
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
  
To see the current configuration,use the following commands.
  
1. In privileged exec mode,  _show etherchannel load-balance_. (This will display both general and per-protocol configurations.)
2. To change the configuration, enter global configuration mode, then use the command _port-channel load-balance_, followed by the desired configuration, e.g. _src-dst-mac_
    
Remember, etherchannel is the command used in the CISCO CLI to view the configuration, while port-channel is used to configure.

# Methods of EtherChannel configuration on Layer 2 CISCO switches
  
There are three different methods of configuration 
  
1- Port Aggrecation Protocol (PAgP): A CISCO proprietary protocol, it dynamically negotiates the creation and maintenance of EtherChannels between CISCO switches. Much like DTP does for trunking, it sends frames to the neighboring switch to determine whether or not it wants to form and EtherChannel.
2- Link Aggregation Control Protocol (LACP): The industry standard protocol (IEEE 802.3ad), it performs the same function as PAgP, but can be used to be formed EtherChannels between switches from different manufacturers. This makes LACP the preferred method of EtherChannel configuration.
3-Static EtherChannel: The switch doesn't use a protocol to determine if an EtherChannel should be formed, rather, interfaces are statically configured to form and EtherChannel. This is not typically used, because you want your switches to be able to dynamically maintain the EtherChannels in the case of interface failures.
    
Up to 8 interfaces can be formed into a single EtherChannel (though when using LACP, up to 16 are allowed, with 8 active and 8 reserved in standby in case of active interface failure)
Steps for Configuration
  
1. Enter global config mode
2. Enter command _Interface range_ followed by the range you want to set as an EtherChannel, e.g. g0/0 - 3
3. Enter command _channel-group_ followed by the number you want to assign to the group (must match for member interfaces on the same switch, does not need to match the channel group number on the other switch), followed by one of the following commands:
a. active- enables LACP unconditionally. Will form an EtherChannel with any device set to either active or passive.
b. auto- enables PAgP only if a PAgP device is detected. Two devices with auto mode will not form an EtherChannel.
c. desirable- enables PAgP unconditionally. Will form an EtherChannel with any device set to either auto or desirable.
d. on- EtherChannel only. Static EtherChannel configuration, only works with on mode.
e. passive- enables LACP only if another LACP device is detected. Two devices with passive mode will not form an EtherChannel
You can also use the command _protocol_ to manually configure the negotiation protocol to either LACP or PAgP. You must then configure the group to the appropriate mode.   This is typically not something you would do, as autonegotiation is much more efficient.
    
After you have finished configuring the channel group, you configure it to trunk mode as if it were a single interface, with the following commands.
  
1. In global configuration mode, _interface port-channel (number)_
2. _switchport trunk encapsulation (encapsulation type)_
3. _switchport mode trunk_
    
  In addition to configuring the port channel, this will also configure the individual physical interfaces in the group the same way. This is crucial, as all interfaces in the group must have matching configurations, including not only the same switchport mode, but the same duplex, the same speed and the same native and allowed VLAN range. If one or more of the physical interfaces configurations do not match the group, they will be set to "suspended", and will not function until the configuration mismatch is fixed.

# Notes on Layer 3 switches
  
Modern networks are shifting towards using layer three switches as the default, due to the fact that STP is not an issue between Layer 3 switches (layer 3 switches do not forward layer 2 frames, so there can be no layer 2 loop created). 
To configure a Layer 3 EtherChannel, use the following commands.
  
1. In global configuration mode, _int range (interface range you want to be in the group)_
2. _no switchport_ (thus making them layer 3 routed interfaces)
3. _channel-group (number) mode active_
4. _int po(group number assigned above)_
5. _ip address (ip address and subnet mask)_

This has covered the basic overview of configuring EtherChannels on both 
