## Study Guide for CDP & LLDP 

# Overview

This will be a study guide for the CDP and LLDP portion of the CCNA Exam. It will introduce Layer 2 Discovery Protocols, specifically the Cisco Discovery Protocol (CDP) and the Link Layer Discovery Protocol (LLDP).

# Layer 2 Discovery Protocols

These protocols share and discover information about neighboring devices on the network operating on Layer 2. This information includes the host name, the device type, the IP address and other relevant info. They can be a security risk, as they share information about devices in the network, so it is usually up to the network engineer or adminstrator to decide if they want them to be used or not.

# CDP

Ciscos proprietary protocol is CDP, and is enabled on Cisco devices by default. CDP messages are sent to the multicast MAC address 0100.0CCC.CCCC every 60 seconds by default. Even though they are a multicast message, the devices that recieve them process and discard them, rather than forwarding them to other devices on the network that are not diectly connected. CDP has a default hold time of 180 seconds, If this time passes without receiving a CDP message from a CDP neighbor, then that neighbor is removed from the CDP neighbor table to clear old neighbors that used to be connected. There are two versions of CDP, CDPv1 and CDPv2. CDPv1 is typically no longer used, so v2 messages are sent by default. 
Here are some basic commands for CDP:

Privileged Exec Mode commands:

_show cdp_: shows basic CDP info, such as packet send times and hold times, as well as the version of advertisements enabled.

_show cdp traffic_: show the number and types of CDP packets sent and recieved

_show cdp interface_: shows how many and which interfaces have CDP enabled and what their send and hold times are.

_show cdp neighbors_: displays the device ids of all cdp neighbors, which local interface they are connected to, how long each device hs left in their hold time, the capabilities of the devices, the platform (model) of the device and the port id of the neighboring device.

_show cdp neighbors detail_: a more detailed breakdown of all cdp neighbors, including the IOS version on each neighbor, information on the VTP status, and the  native vlan and duplex being used by each device. This helps to identify any mismatches of native VLANs or interface duplex. CDP will actually display messages on a device if any mismatches are found. 

_show cdp entry (neighbor id)_: this will give you the information from _show cdp neighbors detail_ for a single neighbor, making it easier to see detailed cdp information and determine if there are any issues on a specific device without having to search through the whole list of neighbors. 

Global config mode commands:

_(no) cdp run_: enables or disables cdp globally

(config-if)#_(no) cdp enable_: enables or disables cdp on specified interfaces

_cdp timer (seconds)_: configures the CDP timer

_cdp holdtime (seconds)_: configures the CDP holdtime

_(no) cdp advertise-v2_: enables or disables CDPv2 (this command will probably never need to be used, as v2 is the default, but it's good to know)

# LLDP

This is the industry standard protocol (IEEE 802.1AB). CDP was the original protocol, so LLDP was created later for use in all network devices. Since CDP is enabled by default on all Cisco devices, LLDP is disabled by default and must be manualy enabled. Even though you will typically use one or the other, both protocols can be run at the same time. LLDP messages are sent to the multicast MAC address 0180.C200.000E, but are not forwarded. The LLDP timer is set to 30 seconds by default, and the holdtime is 120 seconds. LLDP also has an additional "reinitialization delay" timer, set to 2 seconds by default. If LLDP is enabled, either globally or on an individual interface, this timer delays the actual initialization of LLDP. This is helpful in cases of "flapping", where LLDP is rapidly enabled/disabled for some reason.

Here are some basic LLDP commands: 

As you will have to enable LLDP, we'll begin with global config mode commands:

_lldp run_: this enables LLDP globally (in addition to global LLDP enabling, you must also enable it on each interface.)

(config-if)#_lldp transmit_: enables LLDP transmission on a specified interface

(config-if)#_lldp receive_: enables LLDP reception on a specified interface.

_lldp timer (seconds)_: configures the LLDP timer.

_lldp holdtime (seconds)_: configures the LLDP holdtime.

_lldp reinit (seconds)_: configures the LLDP reinitialization timer.

Privileged exec mode commands:

_show lldp_: shows basic global LLDP information including status and timer lengths.

_show lldp traffic_: shows traffic statistics such as the number of frames in and out, recieved in error, and discarded or unrecognized frames.

_show lldp interface_: shows the LLDP status of each enabled interface, including the enabled state of LLDP RX and TX and the current TX and RX state.

_show lldp neighbors_: shows the same basic information as _show cdp neighbors_ (device ids, local interfaces, capabilities and Port Id). However, there is no "platform" field, and rather than showing the amount of time left for each neighbors holdtime, it simply shows the holdtime configured on the device (e.g. 120).

_show lldp neighbors detail_: similar information to what is shown above, along with the IOS, holdtime remaining, enabled capabilities, management addresses, autonegotiation statuses, physical media capabilities, media attachment unit types and the VLAN ID.  

_show lldp entry (neighbor hostname)_: shows the same information as neighbors detail, but for a specified interface.

This covers the topics of CDP and LLDP for the purposes of the CCNA exam
