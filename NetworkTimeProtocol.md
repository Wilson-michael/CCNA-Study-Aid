## Network Time Protocol Study Guide

# Overview

This will cover the subject of Network Time Protocol (NTP) for the purposes of the CCNA exam. It will cover the importance of time for network devices, manual time configuration and the basics of NTP and NTP configuration.

# Network time and its importance.

All devices have an internal clock, from servers to routers to pcs and phones. In the Cisco IOS, you can view the time in Privileged Exec mode with the command _show clock_. This will display the date and the time, which by default is set to Coordinated Universal Time, or UTC. The command _show clock detail_ will show the time source, the default of which on Cisco devices is the hardware calendar (the internal clock on the device). If there is an asterisk next to the time, it means that the time is not considered authoritative, as the internal hardware clock will drift over time, making it a somewhat unreliable time source. From the perspective of the CCNA, it is important to have an accurate time source to maintain accurate logs for troubleshooting.

# Manual Time Configuration

The software clock on a Cisco device can be manually set using the command _clock set_ in Privileged Exec mode. You will begin by setting the hour, minute and second (in 24 hour time format), followed by the day of the month, then the month of the year, then the year.

To configure the hardware clock on a Cisco device, while in Privileged Exec mode, 
enter the command _calendar set_. The remainder of the command is the same as that used to set the software clock.

To sync the hardware clock (calendar) to the software clock (clock), use the Privileged Exec mode command _clock update-calendar_. If you want the clock to sync to the calendars time, use the command _clock read-calendar_. This is useful when one of the times is correct and the other is not.

To configure the timezone, use the Global Config mode command _clock timezone_, followed by the name for the correct time zone, then the hours and minutes that it is offset from the default UTC time zone. 

Finally, to configure daylight savings time, enter Global Config mode, and use the command _clock summer-time_, followed by the name of the time zone. Next, you will either choose the date on which DST will begin this year, or, the better choice, configure a recurring change to and from DST on a specific day each year. To do this, after entering _recurring_, you specify which week in the month it will start, then the weekday, then the month, then the time, followed by the week of the month it will end, then the day, month and time and the offset to add in minutes (e.g. R1(config)#clock summer-time 3 Sunday March 02:00 2 Sunday November 02:00 ).

# Network Time Protocol

When you are dealing with a large network, manual configuration of device clocks is not a viable option. Manually configured clocks will drift, and there will be innacurate times across the network. Network Time Protocol (NTP) allows for the time to be automatically synced over a network.
NTP clients request the time from NTP servers (note, devices can be both NTP clients and servers at the same time). This protocol allows for accuracy within roughly a single millisecond if the client and server are on the same LAN, and within around 50 milliseconds if they are connecting over a WAN or the internet. Some NTP servers are "better" than others, this being determined by this distance, or "stratum", of the server from the original reference clock (usually a very accurate time device, such as an atomic or GPS clock) which has a stratum of 0. The higher the stratum, the less accurate the clock will potentially be. NTP servers connected directly to the reference clock, known as "Primary Servers", have a stratum value of 1. The servers connected to those, are stratum 2, the next connected servers are stratum 3, etc, up to a maximum of stratum 15, past which the clocks are deemed unreliable. These are all considered "Secondary Servers", and operate in server and client mode at the same time. Devices on the same stratum can "peer" with each other, providing a more accurate time and a backup in the case of disconnection from the lower stratum server. This is the third NTP mode Cisco devices can operate in, in addition to Server and Client, known as "Symmetric Active" mode. NTP clients can get their reference times from multiple servers.

# NTP Configuration

If you want to configure your network to use NTP for your clocks and calendars, you need to first determine where you are going to be syncing that information from. In the commands listed below, time will be synced from time.google.com.

The IP addresses listed for time.google.com are as follows:
216.239.35.4
216.239.35.12
216.239.35.8
216.239.35.0

While in Global Config Mode, enter the command _ntp server 216.239.35.0_.
Repeat for .4,.8 and .12.

NTP will automatically choose which server to sync from. In most cases, it is best to provide multiple servers for NTP, in case one starts to slow or stop responding altogether and NTP needs to choose another.

If you want to manually make a device prefer one of the servers, simply add prefer to the end of the command containing the server you want.

NTP uses the UTC time zone by default, so if you are not in the UTC, you will need to manually configure your time zone.

First, enter the command _clock timezone (name of the timezone)(hours and minutes the timezone is offset from UTC)_

Next, enter the command _ntp update-calendar_, which will adjust the hardware clock to match the correct timezone while still using NTP to keep accurate time. Keeping the hardware clock accurate is a good practice, as it will track time even in the case of shutdowns or lost power, and it will initialize the software clock when the system is restarted.

After you have configured NTP, you will want to return to Privileged Exec mode and enter the command _show ntp associations_. This will list the addresses of the NTP servers configured and which of them is currently syncing to the device and which are candidates for syncing, the name of the reference clock associated with each server and their stratum.

Another useful "show" command is _show ntp status_. This will let you see the synchronization status of the clock, the stratum of the device andthe IP address of the reference clock, along with other information.

The command _show clock detail_ shows the time and time zone configured on the device, as well as where the time source comes from.

In the case of larger networks, you can use the devices that are NTP enabled to act as servers to other devices on the network. The commands are as follows.
While in Global Configuration mode, create your loopback interface with a dedicated IP address and subnet mask, then exit back to global config mode and enter the command _ntp source (loopback interface number)_. This allows the NTP server to advertise NTP messages to other devices on the network without the possibility of an interface going down and removing the NTP enabled interface from the networks routing tables. 

After this, enter the command _ntp server (loopback interface ip address)_. This enables NTP on the interface and allows it to advertise to other network devices.

These same commands can be configured on all network devices, enabling redundancies and accurate time keeping.

In cases where the network doesn't have access to a reference clock, you can still ensure that all network devices maintain the same time by configuring an NTP master clock.

While in Global Config mode on the device you want to designate as the master clock, enter the command _ntp master [Stratum number 1-15]_ (the defaul stratum number is 8)

To configure NTP Symmetric Active mode, enter the command _ntp peer (ip address of the device you want to peer with)_.

# NTP Authentication

This is an optional configuration that allows NTP clients to ensure that they only sync to the intended NTP servers. Below are the Global Config mode commands you will use to configure NTP Authentication

Enter the command _ntp authenticate_ to enable NTP authentication

_ntp authentication-key (key-number)md5 (key)_ creates a key number and key (password)

_ntp trusted-key (key-number)_ specifies the trusted key or keys.

_ntp server/peer (ip address) key (key-number)_ indicates which key to use for the server. (This command is not needed on the server itself)

This has covered the basics of NTP for the purpose of the CCNA Exam.



