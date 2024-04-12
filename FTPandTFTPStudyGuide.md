## File Transfer and Trivial File Transfer Protocols Study Guide

# Overview

This will be a study guide covering the topics of File Transfer Protocol (FTP) and Trivial File Transfer Protocol (TFTP) for the purpose of the CCNA Exam. We will go over the [purpose of FTP and TFTP](#purpose-of-ftp-and-tftp), [the functions and differences between the two](#functions-and-differences-between-ftp-and-tftp), [IOS file systems](#ios-file-systems),and [how to use FTP and TFTP in Ciscos IOS](#Using-ftp-and-tftp-in-cisco-ios).

# Purpose of FTP and TFTP

FTP and TFTP are both industry standard protocols that are used to transfer files over a network, using a client-server model to do so. While these protocols are used for many types of file transfers, the most common use that a network engineer has for them is that of downloading an upgraded version of a network devices IOS. 

# Functions and differences between FTP and TFTP

Let's begin with TFTP. TFTP, the newer of the two, was first standardized in 1981, and was designated 'Trivial' due to the simplicity of its design. Its only ability is that of allowing a client to copy a file to or from a server. Because of this simplicity, it is not a replacement for FTP, but rather a tool for situations where functionality and security is less important than speed and minimal resource useage. There are no authentication measures or encryption methods used by TFTP, meaning that all data is sent in plain text, and servers will respond to all TFTP requests without the need for usernames or passwords. The best situation for TFTP use is in a controlled environment (LAN or VLAN) where the quick transfer of small files is important. TFTP uses UDP port 69, and while UDP doesn't provide reliability in transmissions, TFTP does have some features built into the protocol itself that help to ensure reliable transmissions, including Ack messages from both clients and servers and timers that are used for 'lock-step' communications (wherein both the client and the server will alternate sending messages and waiting for an Ack reply, resending the message if they don't receive the Ack within the specified timeframe). 

TFTP connections have three phases. 

The first phase is the "connection", where a clent sends a request to the server and the server responds, thereby initializing the connection. 

Next is the "data transfer" phase, where the client and the sever alternate with data and Ack messages. 

Finally is the 'connection termination' phase. After the last data message is sent from the server, a final Ack message is sent to terminate the connection.

One interesting aspect of TFTP is the Transfer Identifier (TID). As noted above, when a client begins the process of connection, it will send a request to UDP port 69, with a randomly generated ephemeral port as the source port. This is the client's TID. The server will receive the message on UDP port 69, and reply to the client's TID. However, rather than continuing to use UDP port 69 as the server's TID, it will choose its own ephemeral port as a TID, and all messages between the two from that point on will use the two TIDs. 

Now comes FTP. This protocol was first standardized in 1971, although it has received updates since then. It is a more secure protocol than FTFP, with authentication methods such as usernames and passwords. As with TFTP, however, there is no encryption with FTP, meaning that data, including passwords and usernames, is sent in plain text. An upgrade to FTP that deals with this issue is FTPS, where SSL and TLS can be used. SSH File Transfer Protocol (SFTP) is a new protcol (not just an upgrade) that adds encryption as well. FTP isn't limited to file transfers, but allows clients to navigate and edit file directories, list files and many other functions, all of which are performed using FTP commands sent from the client to the server. FTP uses two well known TCP ports, 20 and 21, to perform its functions. FTP uses TCP port 21 for an FTP control connection, sending FTP commands and replies. It uses TCP port 20 for the FTP data connection, where transfers of files and other data is performed. 

The FTP data connection has two different modes. 

The first mode is 'active', where the server initiates the TCP connection. This is the default mode of FTP connection.

The second mode is 'passive', and is where the client initiates the connection. This is often the mode used when the client is behind a firewall that could block the incoming connection from the FTP server.

# IOS file sytems

A file system is a way of controlling the storage and retrieval method of data. In the Cisco IOS, you can view the file system of a device using the Privileged Exec command _show file systems_. The table below shows some of the System types.

| Name    | Type/useage                                                                    |
|---------|--------------------------------------------------------------------------------|
| disk    | Storages devices such as flash memory                                          |
| opaque  | This is used for internal functions                                            |
| nvram   | The internal Non-Volatile Flash Memory where the startup-config file is stored |
| network | This represents external file systems such as FTP and TFTP servers             |

# Using FTP and TFTP in Cisco IOS

As mentioned above, the most common interaction a Network Engineer will have with FTP or TFTP is when an upgrade to the IOS of a Cisco network device is needed. Typically, the image will be downloaded from the external Cisco server to an internal server, then downloaded onto the local device.

To see the current version of IOS that is on the device, enter the Privileged Exec mode command _show version_. This will bring up the name and version of the IOS on the device. Remember to look for the K9 designation in the name, which indicates the ability to use cryptographic techniques such as SSH for security. _Show flash_ is another command that can be used to see the systems flash directory, which contains the IOS image.

To copy a file using TFTP, while in Privileged Exec mode, enter the command _copy (source, aka tftp): (destination, e.g. flash)_

The device will request the name or the IP addess of the remote host. Enter the TFTP server IP address.

The device will request the source filename. Since TFTP cannot access information on the server, you will have to know the filename ahead of time and enter it here.

The device will request the destination filename. You can either rename the file, or simply press 'enter' to accept the default.

To make the device use the new image as its IOS, go to Global Config mode and enter the command _boot sytem (filepath, aka flash: filename of the new IOS)_ If you don't enter the filepath, the device will just used the first IOS file it finds in flash rather than the update.

Enter the command _write memory_

Enter the command _reload_

After you have successfully updated the IOS, delete the old IOS using the Privileged Exec mode command _delete (filepath, aka flash:filename of old IOS)_. It's unneccessary and takes up space. 

There are only two additional steps to copying files using FTP, after which the commands are the same, just switch out FTP for TFTP.

First, while in Global Config Mode, enter the command _ip ftp username (username)_. Next, enter the command _ip ftp password (password)_. Finally, exit to the Privileged Exec mode and follow the steps from above.

This has covered FTP and TFTP for the purpose of the CCNA Exam.