## Port Security Study Guide for the CCNA Exam

## Overview

This is a study guide on Port security for the purpose of the CCNA Exam. It will cover an [introduction to port security](#Intro_to_port_security),[why we use port security](#why_we_use_port_security), and some basic [port security configuration](#port_security_configuration).

# Intro to port security

Cisco switches all have port security as a feature, allowing you control ofver which source MAC addresses are permitted to enter a switchport. When an unauthorized source MAC address enters a port, an action will be taken. By default, the port will be placed in an _'err-disabled' state_.

When port security is enabled on an interface, a maximum of one MAC address is allowed by default, but this can be manually set by a user. These MAC addresses can be manually configured, or dynamically learned by the switch itself when devices are connected to the switchport.

# Why we use port security

Port security is a fundamental aspect of network security for any network admin. It allows an admin to have control over the specific devices allowed access to the network. It also helps to limit the effectiveness of spoofing and DHCP starvation attacks by setting a maximum number of MAC addresses allowed on each port. 

# Port security configuration

All port security configuration takes place in global config mode. Begin by designating the interface on which you want to configure port security, e.g. _int g0/1_. Note that by default, switchports are set to _dynamic auto_ for their administrative mode. In order to configure port security, you must designate the port as either an access port or a trunk port using the command _switchport mode (trunk or access)_. Then, enter the command _switchport port-security_. At this point, the interface is configured with the default port security settings, meaning that the violation response is to shut down the port, there is no aging timer, the aging type is absolute and only one MAC address is allowed to access the port. 

If you have an event that shuts down the interface, such as an unauthorized device connecting to the port, and you need to re-enable it, begin by ensuring that the offending device is disconnected, then enter the _shutdown_ and _no shutdown_ commands.

Another method of re-enabling an _errdisabled_ interface is through _ErrDisable Recovery_. To determine the status of this option, enter the _show errdisable recovery_ command. This will display the err-disabled reasons and whether or not errdisable recovery is enabled for each of them. In order to enable it, determine which errdisable reason you want it to function with, and enter the command  _errdisable recovery cause (the cause of the errdisable state, e.g. psecure-violation, which is a port security violation)_. You can then specify the interval at which recovery will be attempted with a range between 30 seconds and 86400 seconds (24 hours). The default is 300 seconds (5 minutes). While this is a useful tool, you must remember to remove the unauthorized device. If the MAC address allowed on the interface is statically configured, it will continue to shut down the interface. If the switch dynamically learns its secure MAC addresses, then the previous device might be cleared, and the new unauthourized device might be added, which is a huge potential security risk.

Above examples have used the default violation mode of _shutdown_, which places an interface in an err-disabled state and generates a Syslog and/or SNMP message if an unauthorized frame enters it while configured with port-security. This also means that it won't continue generating messages, even if the unauthorized device continues to try to send traffic. 

There are two other modes that can be configured on an interface with port-security enabled. 

First is Restrict. This causes the switch to discard any traffic from unauthorized MAC addresses, but not to disable the interface, allowing devices with authorized MAC addresses to continue to use it. It will generate a Syslog and/or SNMP message every time an unauthorized MAC is detected, and the violation counter will be incremented by 1 for each unauthorized frame.

The last mode is Protect. As with Restrict, it will discard unauthorized traffic without disabling the interface. It does not, however, generate Syslog/SNMP messages, nor does it increment the violation counter.

In order to enable these violation modes, follow the same steps as you would with Shutdown, using the commands _switchport port-security_, _switchport port-security (the statically configured MAC address)_, and finally the command _switchport port-security violation (Restrict or Protect)_.

Another method of securing a port is the use of _Secure MAC address aging_. By default, the age-out timer for secure MAC addresses is set to 0 minutes, meaning that they will not 'age out'. 

In order to set an age out timer, use the command _switchport port-security aging time (minutes)_. 

The default aging type on a switch is _Absolute_, meaning that after the timer expires, the dynamically learned MAC address of a connected device will be removed, regardless of whether or not it continues to receive frames from that device. Note that statically configured MAC addresses will not 'age out' by default. You can configure them to do so with the command _switchport port-security aging static_.

You can configure the device to forget the MAC address after a set time of inactivity by using the command _switchport port-security aging type inactivity_. You can also configure an absolute time by using _absolute_ in this command, rather than _inactivity_.

A final method of ensuring port security is the use of _'Sticky Secure MAC Addresses'_. These are enabled through the use of the command _switchport port-security mac-address sticky_, which will add dynamically learned MAC addresses to the running config. These addresses will never 'age out', and all current dynamically-learned secure MAC addresses will be converted to sticky secure MAC addresses. In order to ensure they remain in the MAC address table in case of a switch restart, be sure to save the running-config to the startup-config. 

You can remove the 'sticky' status of all current dynamically-learned MAC addresses by issuing the command _no switchport port-security mac-address sticky_.

Sticky secure MAC addresses are useful from a security perspective because they persist through reboots and only allow pre-learned MAC addresses (similar to Statically-learned secure MAC addresses) without the time and effort needed to statically configure each MAC address (similar to dynamically-learned secure MAC adresses).

If you want to view the MAC address table, issue the command _show MAC address-table_. A type of _Static_ indicates either a Static or Sticky Secure MAC address, while the _Dynamic_ type indicates a Dynamically-learned secure MAC address. To see only secure MAC addresses, issue the command _show MAC address-table secure_.

This has covered the basics of Port security for the purpose of the CCNA Exam.