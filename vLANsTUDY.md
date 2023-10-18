## VLAN Study Guide for CCNA

# Overview

This is a basic overview of what I feel are important aspects to keep in mind when studying VLAN's for a CCNA Certification

# VLAN's
   Purpose
      To help ensure intra-network security and reduce unnecessary traffic 

   # Basics
      VLAN's are typically configured in two separate formats: Access and Trunk
         Trunk Ports allow VLAN traffic to use single network device access points, reducing the physical space needed on Routers and Switches to create large numbers of VLAN's. This is done by the use of "Tags", typically based in the 802.1Q Protocol from the IEEE (The Institute of Electrical and Electronics Engineers). The tags are located in the ethernet frame, between the source field and type field of the ethernet header. It is 4 bytes (or 32 bits) of data, which I will break down below
            The 802.1q, or "Dot1Q" tag is made up of two main fields: the Tag Protocol Identifier, or "TPID", and the Tag Control Information, or "TCI".
               The TPID is 16 bits in length, and is always set to 0x8100. This value lets the switch know that this frame is Dot1Q Tagged.
               The TCI field contains three subfields, the first of which is the Priority Code Point, or the PCP. It is 3 bits in length, and it is used for Class of Service, which prioritizes traffic on a congested network.
               The second TCI field is the Drop Eligible Indicator (DEI), containing a single bit, and its purpose is to let the switch know whether or not this frame can be dropped in a congested network situation.
               The last TCI subfield is the VLAN ID,or VID. It is 12 bits in length, and identifies which VLAN the frame belongs to, which is the essential function of the Tag.
               The 12 bits in binary give us a total of 4096 VLANs, from 0-4095. Keep in mind, 0 and 4095 are both reserved, so the total number of VLAN's available to us are 4094.
        The 4094 available VLAN's are divided into two separate ranges, 1-1005 and 1006-4094. 1-1005 are designated as "Normal VLAN's", while 1006-4094 are "Extended VLAN's"
    
        Another feature of the Dot1Q tag is the "Native VLAN". The default Native VLAN is 1. However, it can and should be switched to an unused VLAN. Untagged frames will automatically be forwarded on the Native VLAN, which can lead to failed transmissions between clients. Something to remember is that the Native VLAN has to be configured separately on all Trunk Ports, and it is important to ensure that the same Native VLAN is configured on all Trunk Ports. If a frame is sent from a client on the NVLAN, the switch will not tag it with the VLAN it is sent from, and any other switches en route will forward it along the NVLAN configured on that switch. Say that you have a network where VLAN 10 is divided between two separate switches, and one switch has a NVLAN of 10 while the second has a NVLAN of 30. A client in VLAN10 attempts to send a frame to another client on VLAN10 through both switches. The first Switch sees that the client is in VLAN10, its NVLAN. It doesn't need to tag that frame with the Dot1Q  tag that indicates VLAN10. The second switch receives the frame, sees that there is no Dot1Q tag, and assumes that it needs to be sent to a client on its NVLAN, which is 30. At this point it will recognize that the destination is in VLAN10, not VLAN30, and will not forward the Frame, as Switches cannot perform Inter VLAN communication without a Router.
        Alternately, say that the same client wants to send a frame to a client in VLAN30. The frame will be sent from the first switch with a tag indicating that it needs to go to VLAN30. The second switch will receive the frame, but since its NVLAN is 30, frames to be sent to that VLAN are not to be tagged, so it will discard the frame. Thus, the simplest way to avoid these crossups is to make sure that the NVLAN across all Level 2 devices on the network are set to the same unused VLAN.

# Configuring Trunkports on Device Interfaces

    On Cisco Switches, begin by entering global config mode (conf t) and specifying which interface you want to configure (eg. g0/0). At this point, rather than using the access port command, enter "switchport mode trunk".