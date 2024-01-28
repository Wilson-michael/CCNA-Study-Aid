## First Hop Redundancy Protocols

# Overview 
    
    This will be a basic overview of First Hop Redundancy Protocols (FHRPs).

# What is the purpose of FHRPs and how are they used

    An FHRP is useful in the event of a breakdown in the connectivity of the network default gateway. It is designed to allow two or more routers to provide backup to the address; if the active router fails, the backup router will take over the address, typically within a few seconds.
    This is done through the sharing of a virtual IP address (VIP). Say you have two routers, one with the address of 172.16.0.254 and the other .253. A VIP that you could use would be .252, and all end hosts on the network would have that set as their default gateway. The routers would then negotiate their roles with each other through multicast "Hello" messages. One would be the "Active" and the other the "Standby" (though these labels depend on the FHRP being used).
    PCs on the network learn the MAC address of the VIP through broadcast ARP requests. The active router will reply with a unicast ARP reply containing a virtual MAC.
    If the active router goes down, the backup router will recognize it hasn't recieved any hellos from the active router and designate itself the active router with the VIP. It will force the switches in the network to update their MAC address tables by sending "Gratuitous ARP Replies". These are ARP replies sent without being requested. They are also broadcast to F.F.F.F rather than unicast, meaning all switches on the network recieve the frame and update their MAC address tables. 
    Like DRs and BDRs, FHRPs are non-preemptive, so even if the original active router comes back online, it will become the standby router. You can configure preemption to allow the router to take on its previous active role.

# Different types of FHRPs

    Hot Standby Router Protocol (HSRP): A Cisco proprietary protocol in which active and standby routers are elected. There are two versions, 1 and 2. Version 2 adds IPv6 support and increases the number of groups that can be configured.
    V1 multicast IPv4 address: 224.0.0.2
    V2 multicast IPv4 address: 224.0.0.102
    V1 Virtual MAC address: 000.0c07.acXX (XX = HSRP group number)
    V2 Virtual MAC address: 000.0c9f.fXXX (XXX = HSRP group number)
    If you have a situation with multiple subnets/VLANs, you can configure a different active router in each to allow load balancing.

    Virtual Router Redundancy Protocol (VRRP): An open standard protocol where a "Master" and "Backup router are elected. The multicast address is 224.0.0.18, and the virtual MAC address is 000.5e00.01XX (Again, XX is the group number). As in HSRP, while you cannot have multiple master routers within a single subnet or VLAN, you can have a different master and backup in separate ones. 

    Gateway Load Balancing Protocol (GLBP): A Cisco proprietary protocol that load balances among multiple routers within a single subnet. In this protocol. a single Active Virtual Gateway (AVG) is elected, and up to four Active Virtual Forwarders (AVFs) are assigned by the AVG, which can itself be an AVF. Each of the AVFs acts as the default gateway for a potion of the hosts in the subnet. The multicast address for GLBP is 224.0.0.102, the same as HSRP V2. The virtual MAC address is 0007.b400.XXYY (XX is the GLBP group number and YY is the AVF number)

    Below is a table with the basic necessary information of each of these FHRPs

    | Name | Terminology    | Multicast IP                 | Virtual MAC                          | Cisco Proprietary |
    |------|----------------|------------------------------|--------------------------------------|-------------------|
    | HSRP | Active/Standby | V1:224.0.0.2 V2: 224.0.0.102 | V1: 0000.0c07.acXX V2: 000.0c9f.fXXX | Yes               |
    | VRRP | Master/Backup  | 224.0.0.18                   | 0000.5e00.01XX                       | No                |
    | GLBP | AVG/AVF        | 224.0.0.102                  | 0007.b400.XXYY                       | Yes               |

# Configuring HSRP 

    1. While in global config mode, specify the interface you wish to configure, the one acting as the gateway to the Network it is connected to.
    2. Enter the code "standby (value 0-255 in V1)" or "standby version 2 (value 0-4095 in V2). This specifies the group the interface is in, such as the VLAN or Subnet. This value and the version used must match on both routers.
    3. Enter the code "standby (the group number chosen above) ip(the address you want to be asssigned as the VIP).
    4. Enter the code "standby (group number) priority (value 0-255)" Priority determines which router will be the active router. The default is 100. If both routers have the same value, then the router with the highest IP address will be designated as the active router. 
    5. Enter the code "standby (group number) preempt". Preempt only needs to be configured on the router you wish to be the active router, as it allows it to take over as the active router in teh case of a shutdown and restart.
    6. To see the configuration of the FHRP on each router, while in priveleged exec mode enter the code "show standby". 