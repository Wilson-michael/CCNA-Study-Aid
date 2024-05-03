## NAT Study Guide, Part 2

# Overview

This will be the second portion of the CCNA Exam study guide for Network Address Translation. It will focus on [Dynamic NAT](#dynamic-nat) and [Dynamic PAT](#dynamic-pat)

# Dynamic NAT

Whereas Static NAT relies on constant one-to-one mapping of inside local to inside global addresses, Dynamic NAT dynamically assigns them as needed, helping to ease the shortage of globally unique IPv4 addresses as those addresses won't be in constant use. An ACL is used to identify traffic that needs to be translated, allowing permitted source IP addresses to be translated, while denied IP addresses won't be. Note that because this ACL won't be applied to an interface, the traffic will not be dropped as it would be with the typical application of an ACL. 

After the application of the ACL, a NAT pool is created for defining the available inside global addresses.

You can configure Dynamic NAT using the following commands:

From Global Config Mode, choose the inside interface on which Dynamic NAT is to be configured, e.g. int g0/1

Enter the command _ip nat inside_

Choose the outside interface on which Dynamic NAT is to be configured, e.g. int g0/0

Enter the command _ip nat outside_

Enter the command _exit_

Enter the command _access-list (the number you want to be assigned, e.g. 1) permit (the ip address and wildcard mask for the inside local address range you want to allow, e.g. 192.168.0.0 0.0.0.255)_

Enter the command _ip nat pool (Name of Pool, e.g. Pool1) (the range of inside global ip addresses you want to allow the pool to access, e.g. 100.0.0.0 100.0.0.255) prefix-length (the CIDR notation of the address range, e.g. 24) OR netmask (the subnet mask of the address range, e.g. 255.255.255.0)_

Enter the command _ip nat inside source list (the number of the previously assigned ACL, in this case 1) pool (the name of the previously assigned NAT pool, in this case Pool1)_

There are some limitations to the use of Dynamic NAT. Even though inside global addresses are dynamically assigned, they are still one-to-one mapped addresses, and the problem of NAT Pool Exhaustion can arise. This is when there aren't enough Inside Global IP addresses in the NAT pool for all of the hosts that require NAT at one time. Packets from an inside host that needs NAT will be dropped because there are no available addresses. The host will not be able to access outside networks until an inside global address becomes available, either through an automatic timeout (24 hours, by default) or a manual clearing process. This issue can be resolved through the use of Port Address Translation (PAT).

# Dynamic PAT 

PAT, also known as NAT overload, translates both the IP address and the port number of a host if necessary. The use of a unique port number for each communication flow means that a single public IP address can be used by multiple internal hosts at the same time. Each TCP and UDP port number is 16 bits, meaning that there are 65,536 different available port numbers. Note that even if multiple inside hosts happen to have the same port number attached to their inside local address, a router utilizing PAT will make adjustments to the port number attached to the inside global address to be able to track the correct sources and destinations. This process makes PAT the most useful type of Address Translation when it comes to the preservation of public IPv4 addresses around the world., and thus the most widely used.

Configuring PAT is basically the exact same process as that of Dynamic NAT, with the only differences being the ability to define a smaller range of addresses to be assigned to a NAT pool (for example, the address range of 10.0.0.0 to 10.0.0.3 gives you over 196,000,000 available addresses), and the addition of the keyword _overload_ to the end of the final command, e.g. _R1(config)#ip nat inside source list 1 pool POOL1 overload_

Another useful, and more common, method of configuring PAT is the use of the routers outside interface address. The commands are as follows.

Configure NAT on the desired inside and outside interfaces as before, along with an ACL for the inside local addresses to be translated. 

This is where the major difference comes in. Rather that defining a pool of addresses to be assigned, you will map the ACL to the routers outside interface by entering the command _ip nat inside source list 1 interface (the routers outside interface, e.g. g0/0) overload_

This configuration allows the router to use its own public IP address when translating the source IP of the packets.

This has covered Dynamic NAT and PAT for the purpose of the CCNA Exam.
