## Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) Study Guide

# Overview
This will be a basic study guide for the principles of TCP and UDP, and how they compare. 

# Layer 4 Basics
Layer 4 (Transport) provides transparent transfer of data between end hosts, and either provides or doesn't provide various services to applications, listed as follows: 
1. Reliable Data Transfer
2. Error Recovery
3. Data Sequencing
4. Flow Control
It also provides layer 4 addressing with port numbers. Note that these are not the physical interfaces/ports on network devices. They identify the application protocol used and provides session multiplexing.
The Port Numbers used have been designated by the Internet Assigned Numbers Authority (IANA). They are as follows:
Well-known:0-1023. These are strictly regulated.
Registered: 1024-49151. These are not as strictly regulated, but are still very controlled.
Ephemeral/private/dynamic: 49152-65535. These are used for random source ports.

# TCP
    
TCP is a connection-oriented protocol, meaning that before actually sending data to the destination host, the two hosts communicate to establish a connection, called a "three-way handshake". 
A host will send a TCP segment with the SYN flag set to 1. 

The receiving host will then reply with a TCP segment with the SYN and ACK flags set to 1. 

The first host will send a final segment with the ACK flag set to 1, completing the 3-way handshake. 

Once the connection is established, the data exchange begins. 

When the host wants to terminate a TCP connection, it does so through a "Four-way handshake". 

It sends the second host a TCP segment with the FIN flag set to 1. 

The second host replies with a segment with the ACK flag set to 1, then another segment with the FIN flag set to 1.  

The first host finalizes the handshake by sending a segment with the ACK flag set to 1.

What are the benefits of using TCP?

It provides reliable communication, as the destination host must acknowledge that it received each TCP segment. If it doesn't, the segment must be sent again.
It provides sequencing, with the TCP header containing sequence numbers that allow destination hosts to put segments in the correct order even if they arrive out of order.
Finally, it provides flow control, using the window size field to allow the destination host to tell the source host to increase or decrease the rate at which the data is sent.
    
# UDP

UDP is not connection-oriented, therefore it does not establish a connection  between hosts before sending data, it simply sends it.
It doesn't provide reliable communication. Acknowledgements are not sent for received segments, and if any are lost, UDP doesn't retransmit. Segments are sent "best-effort".
It does not provide sequencing. If segments arrive out of order, UDP has no mechanism to put them back in order.
It does not provide flow control, as it has nothing like the window size mechanism to control the flow of data.

# Use cases for each protocol

When are TCP and UDP used?
TCP provides more features than UDP, but has a higher overhead cost. This makes it useful for applications that require reliable communications, like downloading a file.
UDP is preferred for applications like real-time voice and video or games, where small amounts of lost data don't really affect the usefulness of the application, the added cost of TCP would slow it down, and retransmission of lost data would actually cause confusion. There are some applications that use UDP but provide reliability within the application itself.
Some applications, such as DNS, use both TCP and UDP.

# Useful Port Numbers

| TCP            | UDP            | TCP & UDP |
|----------------|----------------|-----------|
| FTP data:20    | DHCP server:67 | DNS:53    |
| FTP control:21 | DHCP client:68 |           |
|SSH:22|TFTP:69|||
|Telnet:23|SNMP agent:161||
|SMTP:25|SNMP manager:162||
|HTTP:80|Syslog:514||
|POP3:110|||
|HTTPS:443|||

