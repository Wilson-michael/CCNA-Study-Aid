## Notes on IPv6 vs IPv4

# IPv4 Overview
    IPv4 was the standard for IP addresses created in 1983. It consists of four octets (a collection of eight binary bits) each ranging from 0 to 255. An example address would be 192.168.0.1 in dotted decimal, (which you may recognize as part of your home ip address, set up as a class c private address space). In order for it to be used by nodes, it must be converted to binary, 
    110000000.10101000.00000000.00000001. This method allows for  a large number of adresses (2^32, or 4,294,967,296, to be exact) to be used in the creation of the internet. However, due to several issues, not the least of which being the unforseen number of devices requiring an ip address, the IPv4 space is no longer sufficient. You can somewhat mitigate the issue by instituting private networks and subnetting, but it's not a permanent solution. Enter IPv6.

# IPv6 Overview
    Where IPv4 was limited to utilizing 256 in each of it's octets , IPv6 implemented the use of 32 hexadecimal characters consisting of 0-9 and A-F separated into 8 quartets to create it's addresses. Each of those characters represents a 4 bit binary value from 0000 (0) to 1111 (15, or F in Hexadecimal). This method of addressing gives a us 128 bits, meaning that there are a total of 2^128, or about 340 undecillion addresses that can be used for further expansion of the internet. There are some ranges that are reserved for specific purposes, which will be outlined later.

# Intro to IPv6 adressing
    