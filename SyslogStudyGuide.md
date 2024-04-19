## Syslog Study Guide

# Overview

This will be an overview of Syslog for the purpose of studying for the CCNA Exam. It will cover [what Syslog is](#what-is-syslog) and the basics of how it works, [Syslog message formats](#syslog-message-format), facilities and severity levels, [the locations of Syslog Messages](#syslog-logging-locations) and [basic Syslog configuration](#basic-syslog-configurations).

# What is Syslog

Syslog is the industry standard protocol for message logging. It is used to log events on network devices, such as changes in interface status or OSPF neighbor status, system restarts, and changes to interface configurations, to name a few. These logs are crucial to the process of troubleshooting, finding the causes of issues, examining who has made changes to configurations, etc., and can be saved in the devices RAM or sent to an external Syslog server. Syslog is a different but complimentary method to SNMP for troubleshooting and monitoring devices.

# Syslog Message format

Here is the format of a Syslog message, followed by an explanation of the purposes of each section.

seq:time stamp: %facility-minority-MNEMONIC:description

seq- a sequence number that indicates the order/sequence of messages.

time stamp- a timestamp that indicates when the message was generated. NTP comes into play with this section, ensuring that the messages on separate devices can be compared to determine the correct sequence of events that took place on each.

facility- the value that indicates which process on the device generated the message (e.g OSPF, EIGRP, if-config, etc.).

severity- a number from 0-7 that indicates the severity of a logged event. A table breaking down what each number indicates will follow this section. Note that the conditions that these levels denote are not industry standard, and each vendor may have a different set of conditions from the next.

MNEMONIC- a short code that indicates what happened.

description- detailed information about the event that is being reported.

| Level | Keyword                | Description                        | Example                                         |
|-------|------------------------|------------------------------------|-------------------------------------------------|
| 0     | Emergency              | System is unusable                 | System is unusable                              |
| 1     | Alert                  | Action must be taken immediately   | A change in interface status to down            |
| 2     | Critical               | Critical conditions                | A failure in Memory Allocation                  |
| 3     | Error                  | Error conditions                   | A change in interface status to up              |
| 4     | Warning                | Warning conditions                 | Achange in line protocol status on an interface |
| 5     | Notice (Notifications) | A normal but significant condition | A change in OSPF neighbor status                |
| 6     | Informational          | Informational messages             | A change in the system clock configuration      |
| 7     | Debugging              | Debug level messages               | Information about an IP packet                  |

# Syslog Logging Locations

Here are some locations where Syslog messages are stored.

The console line. When connected to the device via the console line, Syslog will be displayed in the CLI. By default, this includes all messages from level 7-0.

VTY Lines. Syslog messages will be displayed in the CLI whe a user is remotely accessing the device using SSH or Telnet. This is disabled by default.

Buffer. By default, all Syslog messages, from 0-7, are saved to RAM. You can view the messages saved here using the _show logging_ command.

External servers. Devices can be configured to send Syslog messages to external servers using UDP port 514.

# Basic Syslog Configurations

Below are some basic Syslog configuration commands, all used in Global Config mode.

_logging console (level number or keyword)_: This enables logging on the CLI. By default, all levels from 0-7 are enabled in CLI logging, but you can adjust your levels to reduce resource useage. The level chosen and all levels above it are included when you specify, e.g. level 6 means that levels 6,5,4,3,2,1 and 0 will all be logged.

_logging monitor (level number or keyword)_: This enables logging to the VTY lines. Note that enven when enabled, Syslog messages will not be displayed when connected using Telnet or SSH by default. You must use the command _terminal monitor_ to enable display of Syslog messages, and you must use this command every time you log in using Telnet or SSH.

While logging in VTY lines, using the _logging synchronous_ command will allow for a more readable CLI experience by causing a new line with your command to be printed if a new Syslog message is recieved while you are entering it.

_logging buffered (size in bytes)(level number or keyword)_: This enables logging to the buffer, and dictates the size of the buffer and the level of message logging desired.

_logging [host] (ip address of the external Syslog Server)_: This enables Syslog messages to be logged on an external server. The "host" portion of the command is optional.

_logging trap (level number or keyword)_: This command specifies the level of Syslog messages to be sent to the external server.

_service timestamps log (datetime or uptime)_: This enables timestamps on syslog messages, either with the date and time (preferred) or how long the device had been running when the event occurred.

This has been an overview of Syslog for the purposes of the CCNA Exam.
