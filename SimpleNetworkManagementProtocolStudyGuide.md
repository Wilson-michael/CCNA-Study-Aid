## Simple Network Manangement Protocol (SNMP) Study Guide

# Overview

This study guide will cover the basics of SNMP for the purposes of the CCNA Exam, including a basic overview, the different versions, SNMP messages and some basic configuration.

# What is SNMP

Originally released in 1988, SNMP is an industry standard framework and protocol used to monitor the status of devices, make changes to their configurations, provide notifications on security events, etc. There are 3 widely used versions of SNMP. The first is SNMPv1, the original. Then there was SNMPv2c, which allowed for large amounts of information to be retrieved by a single request, increasing efficiency and reducing network traffic and resources required. Lastly is SNMPv3. This version supports encryption and authentication, making it much more secure than previous versions, and the first choice for use in modern networking. 

There are 2 main types of devices in SNMP. First are Managed Devices, which are those being managed using SNMP, usually routers and switches. Then there are Network Management Stations (NMS). These are the device or devices being used to manage the managed devices.

# How does SNMP function

There are three main operations used in SNMP. 

Number one is managed devices being able to notify the NMS of events, such as interfaces going down or disconnects between devices. 

Second is that the NMS can ask the managed devices about their current status.

Lastly is the NMS can tell the managed devices to make changes to their configuration.

# SNMP Components

SNMP has four different components that allow it to function. The NMS has two, and the managed devices have the other two. The table below gives a more in depth overview.

| Device Type     | Component Name             | Purpose                                                                                                                                                                                         |
|-----------------|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| NMS             | SNMP Application           | Provides an interface for the Network Admin to interact with, displaying alerts, stats, charts, etc.                                                                                            |
| NMS             | SNMP Manager               | The software on the NMS that interacts with the Managed Devices, requesting and receiving information and notifications, sending configuration changes, etc. They use port 162                  |
| Managed Devices | SNMP Agent                 | The SNMP software that interacts with the SNMP Manager on the NMS, it sends and receives messages from the NMS. They use port 161                                                               |
| Managed Devices | Management Info Base (MIB) | The structure that contains the variables managed by SNMP, each of which is identified by an Obect ID (OID). The variables inlude such things as Interface status, cpu usage. temperature, etc. |

# SNMP Message types

There are four different classes of SNMP messages. 

The first class is Read. It is the type of message sent from the NMS to read information from the Managed Devices on the network. "Get", "GetNext" and "GetBulk" are all different R"ead Messages. "Get" is a request to obtain the value of one or more OIDs. "GetNext" is a request to discover the next available variable in the MIB. "GetBulk" is a more effecient version of GetNext.

The second class is Write. These are the messages sent from the NMS to Managed Devices to change information, such as an IP address. "Set" is the Write Message.

The third message class is Notification. These are messages sent from Managed Devices to the NMS to alert it to a particular event, i.e. an interface going down or an unauthorized access attempt. "Trap" and "Inform" are both Notification class messages. "Trap" is sent from the Agent to the Manager. However, the Manager does not send a Response message, causing "Trap" messages to be classified as unreliable.
"Inform" is basically a "Trap" message that is acknowleged with a response. Updates to SNMP have made it so "Inform" can be sent not only between Managers, but between Agents and Managers. 

Lastly, the fourth message class is Response. This message is sent from either the NMS or a managed device in response to a previous message or request. Aptly, the message "Response" falls into this category.

# Basic SNMP Configuration

To set up an SNMP Agent on a Cisco router, use the following commands while in Global Config Mode:

_snmp-server contact (contact email address)_

_snmp-server location (physical location of device)_

Note that these both contain information that is optional, and not required to configure an SNMP Agent.

_snmp-server community (password, aka community string)(ro or rw)_: Ro denotes that this NMS is read only, and cannot use "Set" to make any changes. Rw means that this NMS has both read and write privileges and can make changes to the device by using "Set" messages.

_snmp-server host (IP address of the NMS you want to configure) version (version of SNMP you want to configure, e.g. 2c)(community string)_

_snmp-server enable traps (types of traps to be sent to the NMS)_: You can configure as many different types of Trap deemed necessary by repeating this command with different variables.

This has covered the basics of SNMP for the purpose of the CCNA.