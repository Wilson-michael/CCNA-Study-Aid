## IPv6 Study Guide Part 2

# Overview

    This study guide is part 2 of 3 for the IPv6 portion of the CCNA. It will cover more methods of configuring IPv6, as well as different IPv6 address types.

# Continued IPv6 Configuration

    This method of IPv6 address configuration is done through (Modified) Extended Unique Identifier (EUI) 64, a method of converting a 48-bit MAC address to a 64-bit interface identifier. This interface identifier can then become the host potion of a /64 IPv6 address.
    In order to convert the MAC address, follow these steps:
        1: Divide the MAC address in half, e.g. 1234 5678 90AB is 1234 56 | 78 90AB.
        2: Insert the hexadecimal value FFFE in the middle : 1234 56FF FE78 90AB
        3: Invert the 7th bit: 1(2)34 56FF FE78 90AB, the binary of 2 is 0010, and the bit that needs inversion is 1, which becomes 0, making the new interface ID 1034 56FF FE78 90AB, and will be added on to the network prefix to create the complete IPv6 address. 
    To configure an EUI-64 IPv6 address on a router, follow these steps:
        1: While in global config mode, choose the interface you wish to assign an IPv6 address to.
        2: Enter the command: "ipv6 address (network prefix, e.g 2001:db8::/64) eui-64"
        3: Enter the command: "no shutdown" 
        This tells the router to generate an IPv6 address using the EUI and network prefix. 
    Why is the 7th bit inverted?
        MAC addresses are divided into two types. One is the Universally Administered Address (UAA). It is uniquely assigned to the device by the manufacturer. The second is the Locally Administered Address (LAA). This is manually assigned by an admin or protocol, and doesn't have to be globally unique. An address can be identified as either a UAA or LAA by the 7th bit of the MAC address, known as the Universal/Local (U/L) bit. If the bit is set to 0 it's a UAA, and if it's set to 1 then it's an LAA. When used in the context of IPv6/EUI-64, however, the meaning is reversed. If the U/L bit is set to 0, then the MAC address that the EUI-64 interface ID was made from was an LAA, and vice-versa. The reason behind this inversion is to make the process of configuring local scope identifiers simpler for system admins when hardware tokens are not available.

# IPv6 Address types

    There are multiple different types of IPv6 addresses, with different purposes. Below is a breakdown.
        1: Global Unicast Addresses. These are public addresses that can be used over the internet. An enterprise must register to use them. Because they are public, they must be globally unique. Originally, these were defined as the 2000::/3 block of addresses (from 2000:: to 3FFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF). Now, they are all addresses which aren't reserved for other purposes. An example of a global unicast address and a breakdown of what each portion is is as follows : 2001:0DB8:8B00:0001:0000:0000:0000:0001. [2001:0DB8:8B00:]- this is the global routing prefix assigned by the ISP. [0001:]- this is the subnet identifier [0000:0000:0000:0001]- this is the interface identifier, or the host portion of the address.
        2: Unique local addresses. These are private adresses that cannot be used over the internet. You do not need to register to use them, and they can be used freely within internal networks. While they do not need to be globally unique, you should try to keep them unique for your use. They use the block FC00::7 (FC00:: to FDFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF), though a later update requires that the 8th bit be set to 1, so the first 2 digits must be FD. An example and breakdown follows.
        FD45:93AC:8A8F:0001:0000:0000:0000:0001/64.
        [FD]- this indicates a unique local address. [45:93AC:8A8F:]- this is the global id, which should be randomly generated in order to limit overlap between companies, in case of mergers. [0001:]- this is the subnet identifier, used to make various subnets. [0000:0000:0000:0001]- as in Global Unicast Addresses, this is the interface identifier, the host portion of the address.
        3: Link Local Addresses. These are IPv6 addresses that are automatically generated on IPv6 interfaces by using the command "ipv6 enable" when in interface config mode on a router. They technically use the FE80::/10 (FE80:: to FEBF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF) address block, however, the standard states that the 54 bits after FE80/10 need to be set to 0, so Link Local Addresses will never start with FE9, FEA or FEB, only FE8. The interface ID is generated using EUI-64 rules. Link-local means that these addresses are used for communication within a single link (subnet), meaning routers will not route packets with a link-local destination IPv6 address. Some uses for link-local addresses include: Routing protocol peerings (OSPFv3 uses link-local addresses for neighbor adjacencies), next-hop addresses for static routes, and Neighbor Discovery Protocol (NDP), which is IPv6's replacement for ARP. 
        4: Multicast addresses. These are used for communications from one source to multiple destinations that have joined the specific multicast group. IPv6 uses the range FF00::/8 (FF00:: to FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF) for its multicast. Unlike IPv4, IPv6 doesn't use broadcast, meaning there is no 'broadcast address' in IPv6. Below is a table with some important multicast addresses.

        | Purpose                                    | IPv6 Address | IPv4 Address |
        |--------------------------------------------|--------------|--------------|
        | All nodes\hosts (functions like broadcast) | FF02::1      | 224.0.0.1    |
        | All routers                                | FF02::2      | 224.0.0.2    |
        | All OSPF Routers                           | FF02::5      | 224.0.0.5    |
        | All OSPF DRs\BDRs                          | FF02::6      | 224.0.0.6    |
        | All RIP Routers                            | FF02::9      | 224.0.0.9    |
        | All EIGRP Routers                          | FF02::A      | 224.0.0.10   |

        Another aspect of multicast addresses to be aware of is the scope. IPv6 defines multiple scopes that indicate how far a packet should be forwarded. They are as follows.

        | Type               | Address | Boundaries                                                                                                            |
        |--------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
        | Interface-local    | FF01    | The packet doesn't leave the local device, and can be used to send traffic to a service within the local device.      |
        | Link-local         | FF02    | The packet remains in the local subnet, and routers will not route the packet between subnets                         |
        | Site-local         | FF05    | The packet can be forwarded by routers, and should be limited to a single physical location, not forwarded over a LAN |
        | Organization-local | FF08    | Wider in scope than site-local, covers an entire company or organization                                              |
        | Global             | FF0E    | No boundaries, and is possible to be routed over the internet                                                         |

        5: Anycast addresses. These are a new feature of IPv6, where transmission is one-to-one-of-many. Multiple routers are configured with the same IPv6 address, then use a routing protocol to advertise the address. When hosts sent packets to that address, routers will forward it to the nearest router (based on routing metric) configured with that IP address. There is no specified address range for anycast addresses. You can use a regular unicast address (global unicast, unique local) and specify it as an anycast address. An example would be "R1(config-if)# ipv6 address 2001:db8:1:1::99/128 anycast" (/128 is the same as /32 in IPv4)
        6: Unspecified IPv6 address. Denoted by a "::", it can be used when a device doesn't know its IPv6 address yet. IPv6 default routes are configured to ::/0. The IPv4 equivalent is 0.0.0.0.
        7: Loopback Address. This address is "::1". It's used to test the protocol stack on the local device. Messages went to this address are processed within the local device without being sent to other devices. As it only uses one address, it is much more efficient than the IPv4 equivalent of the 127.0.0.0/8 address range.
