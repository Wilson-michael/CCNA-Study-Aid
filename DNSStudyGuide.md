## Domain Name System Study Guide

# Overview

This will cover the topic of Domain Name System (DNS) for the purpose of the CCNA Exam. It will explain the role of DNS of within the network, its basic functions, and the proper methods to configure DNS in Cisco IOS.

# What is the purpose of DNS

As humans, we're not great at remembering the long strings of numbers that make up different IP addresses, but names are something that we are able to hold on to. Conversely, computers can't use human-readable names to access resources on the internet, requiring numerical data like IPv4 and IPv6 to navigate. In order to resolve this conflict, DNS was created. When a user types the name of a website into their web browser, the computer will make a request to a DNS server for the associated IP address, either by using Dynamic Host Configuration Protocol (DHCP) or a manually configured DNS server. It will receive a type A address (IPv4), a type AAAA address (IPv6), and the CNAME address (the canonical name on the record of the DNS server). As the device receives information from a DNS server, it will save its responses to a local DNS cache, meaning that it won't have to ask the server every time the want to access a certain destination, saving a lot of traffic on the network. Note that this will not be stored permanently on the device, but has a TTL of either 43200 (12 hours) or 86400 (24 hours). 

# DNS in Cisco IOS

If you want the hosts in your network to use DNS, it's not necessary to configure DNS on the routers. They can forward the DNS messages just as they would with any other packets. However, they can be configured as a client and (rarely) a server. Commands to configure DNS on a router are as follows:

To configure a router as a DNS server, while in global config mode, enter the following commands:
_ip dns server_: This configures the router to act as a DNS server

_ip host (Name) (IP address)_: This command enters the specified device name and IP address into the routers DNS host table. 

_ip name-server (ip address of external name server, e.g. 8.8.8.8, the DNS server for Google)_: This gives the router a database to query if the record that's being asked for isn't in its host table.

_ip domain(-)lookup_: This enables the router to perform DNS queries. Typically, this is enabled by default and you won't need to configure it, but it's useful to know it anyways. The hyphen is the old command, and will be recognized by newer devices, so it's good to know both.

Note that any manually configured hosts will be permanent in the host table, while those learned through DNS are temporary.

To configure a router as a DNS client, enter the following commands from global config mode:

_ip name-server (ip address of the DNS server you want to access)_

_ip domain(-)lookup_

_ip domain name (name of the default domain name, e.g. michaelwilson.tech)_: This is an optional command to configure the default domain name. This will cause the name to be automatically appended to any hostnames without a specified domain, e.g. PC1 becomes PC1.michaelwilson.tech. This command will be useful when configuring SSH.

This has covered some of the basic aspects of DNS for the purpose of the CCNA exam.