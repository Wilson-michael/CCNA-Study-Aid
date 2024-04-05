## Secure Shell Study Guide

# Overview

This is a study guide on the subject of Secure Shell (SSH) for the purposes of the CCNA exam. It will cover multiple topics, including [Console Port Security](#console-port-security), [use and configuration of Layer 2 Management IP addresses](#management-ip-addresses), [Telnet useage](#telnet-useage), and [SSH configuration and useage](#ssh).

# Console Port Security

Login as a security measure. By default, on Cisco devices, a password is not required for logging into the console. Anyone can connect directly to the device amd start configuring. In order to configure a password for console access, use the following commands in Global Config mode:_

_line console 0_: There is only a single console line, so the line number is always 0. This is to ensure that only one user at a time is able to configure the device.

_password (password)_: This sets the console lines password.

_login_: This tells the device that a user is required to input the passord in order to access the CLI via the console port.

Another option for securing logins is the implementation of usernames and associated passwords for local login. The commands for configuration are as follows:

_username (username) secret (secret password)_: This creates the unique username and password combination for local login.

_line console 0_: This enters configuration mode for the console port.

_login local_: This tells the device that a username and password are required to access the CLI locally on this device.

Another security measure that should be utilized is the exec-timeout configuration, in which a set time of inactivity logs the user out of the console, requiring a new login session.

# Management IP addresses

Since Layer 2 switches don't perform packet routing or build router tables, they are not IP routing aware, making remote access using Telnet or SSH impossible without configuring a Management IP address on the Switches Virtual Interface (SVI). Below are the commands used in Global Confif mode to configure the Management IP address.

_int (the interface you want to configure the IP address on, e.g. Vlan1)_

_ip adress (the IP address and subnet mask you want to set as the Management IP)_

_no shutdown_ (If necessary)

_exit_

_ip default-gateway (the default gateway of the LAN containing the switch)_: This enables communication with the witch from outside the LAN.

# Telnet useage

Created in 1969, the Teletype Network (Telnet) protocol is used to remotely access the CLI of a remote host. However, Telnet is an insecure method of remote access, sending data in plain text rather than encrypting it. It has mostly been replaced by the much more secure SSH. Telnet traffic uses TCP port 23 for the Telnet server.

Telnet configuration is done in Global Config mode, using the following commands:

_enable secret (secret password)_: If you don't configure an enable password or secret, you won't be able to access Priveleged Exec mode via Telnet.

_username (username) secret (password)_: as in the console line login, you need to configure a usename and password combo for accessing Global Config mode via Telnet.

_access-list (number) permit host (host IP address): This tells the device which IP addresses are allowed to access the Virtual Teletype (VTY) lines of the device. 

_line vty 0 15_: Telnet and SSH are configured on the VTY lines. There are 16 lines available, meaning up to 16 users can be connected at once. This specific command means that you are configuring all VTY lines at once, which is the recommended method.

_login local_

_exec-timeout (minutes)(seconds)_: Configures the specified time of inactivity that will log a user out.

_transport input (type of connections allowed): The different types of traffic that can be input are as follows. 

Telnet only allows Telet connections.

SSH only allows SSH connections.

Telnet SSH allows both.

All allows all connections.

None allows no connections.

_access-class (acl number) in_: The _access-class_ command applies an ACL to the VTY lines, where the _ip access-group_ command applies an ACL to an interface.

# SSH

SSH is a protocol that was developed in 1995 to replace less secure protocols such as Telnet. The "shell" portion of Secure Shell is referring to any program which allows an OS's services to be accessed by a user or another program, either through a CLI or a GUI (Graphical User Interface), and is named such because it is the outermost layer of the OS. A major revision to the protocol, SSHv2, was released in 2006, and should be used whenever possible. If a device supports both versions, it is said to run "version 1.99", which isn't a real version, just a way to say it runs both v1 and v2. SSH provides security features such as data encryption and authentication. SSH servers use TCP port 22.

The steps for SSH configuration on a device are as follows:

First, you should confirm that your device supports SSH using the Priveleged Exec command _show version_. If your IOS image supports SSH, it will have "K9" in its name. Cisco exports NPE (No Payload Encryption) IOS images to countries that restrict the use of payload encryption technologies, and these images don't support cryptographic features like SSH.

You can also use the Priveleged Exec command _show ip ssh_ to see if your image supports SSH. This will also show if SSH is enabled on your device, as well as the specifics of the SSH configuration.

For SSH to be enabled and used on your device, you must generate an RSA (Rivest-Shamir_Adleman) public and private key pair to be used for data encryption and decryption, authentication, etc. These are two large prime numbers, due to the fact that the computation of factoring sufficiently large integers has no efficient solution at the moment. The Fully Qualified Domain Name (FQDN), which is made up of the host name and the domain name, is used to name the RSA keys. The Global Config commands are as follows:

_ip domain name (domain name)_

_crypto key generate rsa_: This will show the name of the RSA keys, and then ask you to specify the length of the key modulus, with a range of 360 to 4096 bits. Note that SSHv2 requires a length greater than 768 bits. You can also simply add the command _modulus (length in bits)_ to the end of this command to achieve the same result.

Now that SSH has been enabled, use the following commands in Global Config mode to configure it on your device.

As with Telnet, use the _enable secret (password)_, _username (username) enable password/secret (password)_ and _access-list (number) permit host(ip address)_ commands.

_ip ssh version 2_: This is an optional but recommended command to restrict SSH to version 2 only.

_line vty 0 15_

_login local_

_exec-timeout (minutes)(seconds)_

_transport input ssh_: The best practice is to enable SSH input only.

_access-class (number) in_

To connect via SSH on a PC, use the command _ssh -l (username)(ip address)_ or _ssh (username)@(ip address)_ 

This has covered SSH for the purpose of the CCNA Exam.