# Dynamic Host Configuration Protocol (DHCP) Study Guide

# Overview

This will cover some of the basics of DHCP for the purpose of the CCNA exam. It will include information on the purpose of DHCP, its basic functions, and how to configure it on Cisco IOS.

# What is the purpose of DHCP

DCHP is crucial in modern computer networks, allowing hosts to automatically and dynamically learn about their network, including their IP address, subnet mask, the default gateway and DNS server without needing these things to be manually or statically configured. Typically, it's used on 'client devices' (cell phones, PCs, laptops, tablets, etc.), as routers, servers, switches, etc. are usually manually configured. In smaller networks, such as home or small home office networks, the router usually acts as the DHCP server for the hosts in the LAN. As you get into larger networks, there is usually a Windows or a Linux server that deals with DHCP. While manual configuration of hosts in smaller networks is possible (and can be engaging), it's not really practical when dealing with large networks, and DHCP is a vital tool.

# DHCP client configuration

A few things about DHCP and address assignment. If you access the ipconfig /all section of you computers CLI, you will see whether or not you have DHCP enabled, and what IP address has been assigned to you. It may also show "preferred", meaning that your device has been assigned that IP address by the DHCP server before, and it was requested and available this session. You will also see two lines showing when the lease of the IP address was obtained and when it will expire. You can configure an IP address to be permanently assigned to a device, but this is not usually the best choice; it will remove that address from rotation until the assigned device releases it, and you will lose space in your network for new devices. 

If you have a device that you are removing from a network, and you want the address assigned to it to be available again, you can release it from the CLI with the command _ipconfig /release_. This sends a DHCP release message to the DHCP server, letting it know that the IP address currently assigned to this device is no longer needed and can be leased to a new client.

Once you are ready to get a new IP address from the DHCP server, enter the command _ipconfig /renew_. The PC will then contact the server, and will possibly be given the same address (if available) or a new one. This process involves four messages, listed below

| Name                 | Type                 | Purpose                                                                                        |
|----------------------|----------------------|------------------------------------------------------------------------------------------------|
| DHCP Discover        | Broadcast            | Asks if there are any DCP servers in the local network, and states that it needs an IP address |
| DHCP Offer           | Unicast or Broadcast | Sends an available IP address, along with information such as the DHCP and DNS servers         |
| DHCP Request         | Broadcast            | Accepts the IP address offered in the DHCP Offer                                               |
| DCHP Acknowledgement | Unicast or Broadcast | Lets the host know it can use the assigned IP address                                          |
A mnemonic device to remember these messages is the acronym DORA

The port numbers associated with DHCP are UDP 67 for servers and UDP 68 for clients.

# DHCP configuration in Cisco IOS

In large enterprise networks, a centralized DHCP server is generally used for the entire network. Because it is outside of the subnets making up the network, it will not receive the DHCP clients' broadcast DHCP messages, as they will not leave their local subnets. In order to deal with this, a router can be configured to act as a DHCP relay agent that will forward broadcast messages to the DHCP server as unicast messages. The commands for configuring a DHCP on a Cisco router follow.

Configuring a router as a DHCP server:

While in global config mode, enter the command _ip dhcp excluded-address (address range not to be given to DHCP clients, e.g 192.168.1.1 192.168.1.25)_: excludes a given range of IP addresses from being assigned to DHCP clients (typically, these are what will be assigned to network devices).

_ip dhcp pool (name)_: creates a designation for the pool of available addresses you are creating for DHCP. A different DHCP pool should be created for each router that the device will be a server for.

_network (network address)(CIDR notation or subnet mask)_: Assigns the subnet of addresses to be assigned to clients, without the excluded addresses from the first command.

_dns-server (IP address of desired DNS server, e.g. 8.8.8.8)_: Specifies the DNS server that should be used by DHCP clients.

_domain-name (desired domain name, e.g. michaelwilson.tech)_: Specifes the domain name of the network.

_default-router (IP address for the networks default gateway)_

_lease (days)(hours)(minutes)(or infinite)_: Specifies the length of the lease time for clients, infinite means that the leases will not end until released by the clients.

If you want to see what addresses have been assigned via DHCP, enter the privileged exec mode command _show ip dhcp bindings_.

Configuring a router as a DHCP relay agent:

Choose the interface connected to the subnet of client devices that need to connect to the DHCP server and enter interface configuration mode.

_ip helper-address (IP address of the DHCP server interface the relay agent is connected to)_

Configuring a router as a DHCP clent:

Choose the interface you want to configure as a DHCP client and enter interface configuration mode.

_ip address dhcp_: This enables the interface to have its IP address dynamically configured by DHCP. While this is not the typical means of address configuration on a network device, it is still useful to know how to do it.

This has covered the basics of DHCP for the purposes of the CCNA exam.