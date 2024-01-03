## CCNA Interfaces and Connections Study Guide 
    
    ## Overview
       This is to provide overview and some mnemonic devices on the Interfaces and Connections utilized in Networking

## Types of connections used and their respective speeds
       
| Type            | Speed    | Maximum Distance |
|-----------------|----------|------------------|
| Ethernet        | 10 Mbps  | 100 Meters       |
| Fast Ethernet   | 100 Mbps | 100 Meters       |
| Gig Ethernet    | 1 Gbps   | 100 Meters       |
| 10 Gig Ethernet | 10 Gbps  | 100 Meters       |

All Ethernet Cables utilize RJ45 Connectors. Ethernet UTP (Unshielded Twisted Pair) is best utilized in LAN connections where the distance is typically under 100 meters, as it is far less expensive than fiber, and the cable can be cut to length and an RJ45 connector attached at the cut end. It is composed of 4 pairs of copper wire. They emit a slight electrical signal outside of the cable which can be detected and copied, and while the twisting of the pairs does help, they can be affected by EMI (Electromagnetic Interference). 
In the Ethernet and Fast Ethernet Connection (10 and 100 Base), pins 3 and 6 on the UTP Cable recieve data, while 1 and 2 transmit. In 1 and 10 Gig Ethernet (1000 and 10G Base), all four pairs are used bi-directionally, the pairs being 1/2, 3/6/, 4/5, and 7/8. 
Full Duplex connections allow a host to send and receive data at the same time, while half duplexes limit them to one or the other at one time.
While PCs, Routers and Firewalls use Pins 1 and 2 to transmit data and 3 and 4 to receive, switches use the opposite, as they must transmit between end hosts. AutoMDI-X (Automatic Medium Dependent Interface Crossover) Enables end hosts to automatically adjust which pins they use to receive and transmit data, meaning you can use a straight-through cable to connect multiple switches without issues communicating. If AutoMDI-X is not enabled, you must use a crossover cable, which reverses the receiving and transmitting pins on one end of the cable. This is also the case when connecting two like end hosts.
Fiber cables are either glass or plastic cables manufactured to exacting standards, utilize SFFP (Small Form Factor Pluggable) tranceivers to connect to end hosts, and are prefabricated at a set length. They cannot be cut or bent at sharp angles,making them slightly more difficult to install. There are two types of Fiber Optic Cable used in LANs, Multimode and Single Mode. Multimode Fiber is the cheaper of the two, as it is slightly larger, making it cheaper to manufacture. It allows for light to bounce through the cable at multiple angles, and has a range of 550 Meters, where Single Mode has a range of 5 km and only allows the light to bounce at a single angle. These both use the 1000BaseLX Protocol, 802.3z. Since Fiber Cables use light pulses, rather than electrical, they are not susceptible to EMI 

## Ethernet Protocols
Ethernet Protocols were first set out by the IEEE in 1985. These are the common protocols for Modern Networking

| Name    | Type       | Speed    | Distance                 | Mnemonic                                    |
|---------|------------|----------|--------------------------|---------------------------------------------|
| 802.3i  | 10BaseT    | 10Mbps   | 100 Meters               | I get 10                                    |
| 802.3u  | 100BaseT   | 100Mbps  | 100 Meters               | YO(U) get 100                               |
| 802.3ab | 1000BaseT  | 1Gbps    | 100 Meters               | ABGB                                        |
| 802.3an | 10GBaseT   | 10Gbps   | 100 Meters               | (AN)d there's 10Gb                          |
| 802.3z  | 1000BaseLX | 1000Mbps | 550 Meters MMF, 5 Km SMF | (Z)ee LX gives you the distance! 550 or 5 K |
| 802.3ae | 10GBase-SR | 10Gbps   | 400 Meters               | SR means Short Range of 400 Meters          |
| 802.3ae | 10Gbase-LR | 10Gbps   | 10km                     | LR means Long Range of 10km                 |
| 802.3ae | 1000BaseER | 1000Mbps | 30 Km                    | ER means extended range of 30km             |

