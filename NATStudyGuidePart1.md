## Network Address Translation (NAT) Study Guide, Part 1

# Overview

This is a study guide on the topic of Network Address Translation (NAT) for the purpose of the CCNA Exam. It will cover [private IPv4 addresses](#private-ipv4-addresses), then [introduce NAT](#NAT-introduction), as well as [static NAT](#static-nat) and [how to configure it](#configuring-static-nat).

# Private IPv4 addresses

As discusssed in previous portions of this study guide, IPv4 has run out of addresses. The switch to IPv6 is the long term solution to this problem, but there have been some short term fixes implemented in the meantime, namely CIDR, Private IPv4 address spaces, and NAT. CIDR has been in constant use throughout this study guide, so there's not much of a need to go over it. Private IPv4 addresses have been used, but this is a more in depth view. Request For Comment (RFC) 1918 specifies several ranges of IPv4 addresses as private, listed below.

Class A addresses are in the 10.0.0.0/8 range, and include 10.0.0.0 to 10.255.255.255.

Class B addresses are in the 172.16.0.0/12 range, and include 172.16.0.0 to 172.31.255.255.

Class C addresses (the most familiar) are in the 192.168.0.0/16 range, and include 192.168.0.0 to 192.168.255.255.

These addresses don't have to be globally unique, and can be freely used in networks. However, they cannot be used over the internet, and ISPs will drop traffic from these addresses. This is where NAT comes in. It allows devices on your LAN to borrow the Globally Unique IPv4 address assigned by your ISP in order to access the Internet.

# NAT Introduction

NAT is used to modify the source and/or destination IP addresses of packets. The most common reason to use NAT is to allow hosts with private IP addresses to communicate with other hosts over the internet as discussed above. The CCNA requires an understanding of source NAT and its configuration on Cisco routers. Source NAT is when a router uses NAT to convert the source IP of a packet (the device IP) to its ISP assigned address, and the reverse for packets recieved from external sources.

# Static NAT

Static NAT is the process of statically configuring one-to-one mappings of private to public IP addresses, where  an inside local IP address is mapped to an inside global IP address. An inside local address is that of the inside host from the perspective of the local network, typically the private address that is actually configured on the inside host. An inside global access is the IP address of the inside host from the perspective of outside hosts, typically the public address configured through NAT. 
Note that with Static NAT, each host requires its own inside global address. This means that while it does allow hosts to communicate over the internet, it does nothing to help preserve addresses, meaning it's not the most useful form of NAT that can be implemented for our purposes.

# Configuring Static NAT

To configure Static NAT on a Cisco device, use the following global config mode commands:

_int (Inside interface on which to configure NAT)_

_ip nat inside_

These commands define the inside interface connected to the internal network

_int (Outside interface on which to configure NAT)_

_ip nat outside_

These commands define the outside interface connected to the external network.

_exit_

_ip nat inside source static (inside local ip, e.g. 192.168.0.155)(inside global ip, e.g 100.0.0.1)_

Repeat this command for all hosts in the network that require NAT. Note that the inside global addresses must be registered to you or your company, not just ones that you decided to use.

To view the NAT translations on your device, enter the command _show ip nat translations_ in Priveleged Exec mode. This will display protocols used, Inside and Outside local and global addresses and port numbers.

Outside local and global addresses are the IP addresses of the outside host from the local and outside networks. Unless destination NAT is used, these addresses will be the same.

_clear ip nat translation_ is another useful Priveleged Exec mode command that will clear dynamic NAT translation entries from the NAT translation table.

_show ip nat statistics_ is a Priveleged Exec mode command that allows you to view detailed information on the devices NAT useage.

This has covered the first portion of NAT for the purpose of the CCNA Exam.