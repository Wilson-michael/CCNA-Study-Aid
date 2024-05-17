## Quality of Service (QoS) Study Guide, Part 1

# Overview

This is part 1 of the study guide on Quality of Service (QoS) for the purpose of the CCNA Exam. It will cover [IP Phones and Voice VLANS](#IP-phones-and-voice-vlans), [Power over Ethernet (POE)](#Power-over-ethernet-(poe)), and an [introduction to QoS](#intro-to-qos).

# IP Phones and Voice VLANS

Traditionally, phones utilize the public switched telephone network (PTSN) to communicate, aka Plain Old Telephone Service (POTS). IP Phones use Voice over IP (VoIP) to allow users to make calls over an IP network, e.g. the Internet. Juist like a typical end host, IP phones are connected to a switch. However, IP Phones also have an internal 3-port switch, consisting of an 'uplink' port to the external switch, a 'downlink' port for connecting to a PC, and an internal port that connects to the phone itself. This means that the PC and the IP Phone can share a single switchport, reducing the number of switchports and additional switches required by an enterprise. Separating 'voice' traffic from the IP phone and 'data' traffic from the PC through the use of a 'voice VLAN' is recommended. This will leave traffic coming from the PC untagged while tagging phone traffic with a VLAN ID. The commands for configuring a voice VLAN are as follows.

While in Global Config Mode, designate the interface you want to configure the voice VLAN on, e.g. _int g0/0_

Enter the command _switchport mode access_

Enter the command _switchport access (the VLAN you want to assign to the port, e.g. vlan 10)_

Enter the command _switchport voice (the vlan you wat to assign as the voice VLAN, e.g. vlan 11)_

With this configuration, the interface g0/0 is still considered an access port, even though it allows traffic from two separate VLANs.

# Power over Ethernet (PoE) 

Originally a Cisco technology called Cisco Inline Power (ILP), Power over Ethernet (PoE) is an innovation that reduces the need for dedicated outlets to power devices such as IP Phones, IP cameras, Wireless Access Points, smart doorbells, etc. These Powered Devices (PDs) receive power, as well as data, over an Ethernet cable from Power Sourcing Equipment (PSE), typically a switch. The PSE converts AC power from an outlet to DC power that is sent to the PDs. 

As devices can be damaged if they receive too much power, PoE can determine if and how much a connected device needs. It begins by sending low power signals to a device when it is connected to a PoE port and monitoring the response to determine the power needs of the device. If it receives a signal that the device needs power, it will supply the power needed for the device to boot. It continually monitors the PD, and supplies the amount of power required to run the device.

To prevent a PD from taking too much power, Power Policing can be configured on the PSE using the following commands; either _power inline police_ or _power inline police action err-disable_. Both of these commands will disable the port, put it into an 'error-disabled' state and send a Syslog message if a PD draws too much power. The interface can be re-enabled by entering the commands _shutdown_ and _no shutdown_, respectively.

Another option is the command _power inline police action log_. This command does not shut the interface down in the case of a PD drawing too much power; rather, it restarts the interface and sends a Syslog message. This will cause the PD to restart as well, reinitiating the process of determining its power needs.

Below is a table breaking down the Types of POE.

| Name                     | Standard #                     | Watts | Powered Wire Pairs |
|--------------------------|--------------------------------|-------|--------------------|
| Cisco Inline Power (ILP) | Cisco Proprietary, no standard | 7     | 2                  |
| POE (Type 1)             | 802.3af                        | 15    | 2                  |
| POE+ (Type 2)            | 802.3at                        | 30    | 2                  |
| UPoE (Type 3)            | 802.3bt                        | 60    | 4                  |
| UPoE+ (Type 4)           | 802.3bt                        | 100   | 4                  |

# Intro to QoS

As stated in the introduction, phone and data traffic used to use separate networks to function, PSTN for phone and an IP network such as an enterprise WAN or the Internet for data. The two types of traffic didn't have to compete for bandwidth, so QoS wasn't necessary. Now, however, most modern networks are converged networks, where phone, video, gaming, regular data and other types of traffic all share the same IP network. While this convergence has been hugely beneficial by enabling cost savings and creating advanced features for voice and video traffic like collaboration software such as Cisco WebEx, Microsoft Teams, Zoom, etc., it also means that the different types of traffic are all competing for bandwidth, and can cause issues with delay-sensitive traffic such as a phone call or video conference. This is where QoS comes in.

QoS is a set of tools used by network devices to designate diferent packets with different priority levels. It is used to manage the following aspects of network traffic.

First off is bandwidth. Bandwidth, as you know, is the overall capacity of a datalink, and is measured in bits per second. QoS can allow for the reservation of a specified amount of the bandwidth for different types of traffic, e.g. 25% for voice, 30% for specific types of data traffic and 45% for all other types.

Next is the delay. This measures the amount of time it takes data to travel, either from the source to the destination (one-way delay), or from the source to the destination then back to the souce (two-way delay).

Then there is jitter. This is the variation in one-way delay between packets sent by the same application (e.g., one packet takes 10ms to arrive while the next takes 100ms). As a high amount of jitter can cause disruptions in smooth communication with VOIP, IP phones have what is known as a jitter buffer to create a fixed delay to audio packets.

Last is loss. This is the percentage of packets that do not reach their destination, either due to physical issues such as faulty cables, or the issue of a device having a full packet queue which resultsi in the device discarding packets.

In order to ensure acceptable interactive audio quality (ie. a phone call), the standard recommended one-way delay should not exceed 150ms, jitter should be 30ms or less, and loss should be 1% or less. Exceeding these values can result in noticeable reductions in call quality.

As mentioned above, network devices use queuing to forward packets if it receives them faster than it forward them from the appropriate interface, such as when a switch is receiving messages from two gigabit interfaces to be forwarded out on a single gigabit interface. The switch will create a queue for messages to be forwarded in First In First Out (FIFO) order. Once the queue becomes full, new messages will be dropped, known as tail drop. Not only does this result in lower quality data transmission, it can also cause TCP glabal synchronization. This occurs due to the TCP sliding window discussed in previous study guides. A quick rundown of this process is that hosts using TCP use the 'sliding window' to increase or decrease the rate of their traffic transmission. It will retransmit a packet when it is dropped and reduce the rate at which it sends traffic, then gradually increase the rate again. When tail drop happens, all hosts (Global, in relation to the network) will reduce the rate at which they forward traffic, meaning that the network becomes underutilized, then they increase the rate, causing congestion and tail drop, causing a cycle of underutilization, congestion and dropped packets.

There are a couple of tools that can be used to prevent this issue. The first is Random Early Detection (RED). This is when a certain threshold of traffic in the queue is reached and the device randomly selects packets to drop, causing the random hosts using TCP to slow their rate of traffic. While this does cause a small amount of disruption to data traffic, it eliminates the waves of underutilizantion and overutilization caused by Global TCP Synchronization. With this tool, all traffic is treated the same.

The next tool is Weighted Random Early Detection (WRED), an improved version of RED. It allows for the control of which types of traffic can be dropped at what point. This will be covered in more detail in Part 2 of the study guide.
