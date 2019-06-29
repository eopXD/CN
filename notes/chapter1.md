# Chapter 1 Computer network and the internet

## 1.1 What is the internet

major in three components:

- devices: the clients and end systems. Run network apps to connect
- communication links: fiber, or physical things that runs electricity
- routes and switches: forward packets, control and managing the flow

#### Protocols

protocols define format, order of messages sent and received among network entities, 
and actions taken on message transmission, receipt.

For example: HTTP, TCP, 802.11, IP

#### Internet standards

- RFC: request for comments
- IETF: Internet Engineering Task Force

## 1.2 Network edges

End systems access the internet through Internet Service Providers (ISP).

End systems are referred as hosts, and we divide them into clients and servers.

To analyze a system, we consider some attributes.

1. the bandwidth ?
2. dedicated / shared ?

Also the type of accessing the internet...

#### Home access: DSL, Cable ...

1. DSL

	Use existing telephone lines to central office DSLAM. 
	
	Transmission is divided into upstream transmission 
	and downstream transmission.
	
	- < 1 Mbps upstream transmission rate
	- < 24 Mbps downstream transmission rate (typically < 10 Mbps)

	```
	DSL modern ( home ) --> DSL access multiplexer ( DSLAM, the central office ) --> ISP
	```
2. Cable
	
	Use hybrid fiber coax (HFX) for transmitting. Unlike DSL, it is not a dedicated access to the central office. 
	
	Data, TV transmitted at different frequencies over shared cable distribution network
	
	- 2 Mbps upstream transmission rate
	- 30 Mbps downstream transmission rate
	
	```
	Hundreds of homes --> Fiber nodes --> Cable modern termination system ( CMTS ) --> ISP
	```
	
3. Wireless devices

	```
	Wireless devices --> Optical network terminator ( ONT ) --> Optical line terminator ( OLT ) --> ISP
	```
	
#### Enterprise access: Ethernet and Wi-Fi

```
End hosts --> Ethernet switch --> institutional router --> institutional ISP
```

## 1.3 Network cores

#### Packet switching

Most packet switches use store-and-forward transmission at the inputs to the links. 
Host:

- want to send some data with $N$ bit
- breaks it into pakcets each with length $L$
- access to internet has transmission rate $R$ ( also called capacity, link bandwidth )

Packet transmission delay = time needed to transmit $L$-length packet = $L/R$

Store-and-forward: packet switch must receive the entire packet before transmit onto the outbound link. Store-and-forward may face some problems, one of it is queueing and loss.

Queueing and loss: arrival rate to link exceeds transmission rate of link.

- packet will queue
- packet can be dropped if memory buffer fills up.

End-to-end delay: $2 * L/R$

There are two key of network-core function.

1. routing: determining the source-destination of packet
2. forwarding: move packets from router input to appropriate output.

#### Circuit switching

End-end resources allocated to, and reserved for call between source and destination.

#### Packet switching vs. circuit switching

Now all users share a 3Mbps link, each user require 150Kbps, and using 10% of the time on network service.

a. If we use circuit switching here, we can only allow 20 users.

b. If we use packet switching, and as the assumption, we can assume probability of a user using the service is 10%. Now there are 100 users. probability of n users transmitting is $P(n) = {{100}\choose{n}} * (0.1)^n * (0.9)^{120-n}$

However packet switching is not the ultimate winner. If we need streaming services circuit switch provides a stable transmission rate. To build a circuit-like packet switching routing is still a challenge right now.

## 1.4 delay, loss, throughput in networks

三個網路系統重要的因素 - delay, loss, throughput

無線的情況下容易影響傳輸狀況

#### delay

packets queue in router buffers, packet arrival rate to link exceed output link capacity

sources of delay

- nobal processing: check bit errors and determine output link
- queueing: time waiting at output link for transmission
- transmission: $\frac{packet\ length}{link\ bandwidth}$
- propagation: $\frac{physical\ length}{propagation\ speed}$

usually processing/transmission/propagation delay are constant.

the big problem is on the queueing delay.

#### queueing delay

- $R$: link bandwidth(bps)
- $L$: packet length(bits)
- $a$: avg. packet arrival rate

senarios:(delay 是指數成長的)

- $La/R \approx 0$: queueing delay small
- $La/R \rightarrow 1$: queueing delay large
- $La/R > 1$: queueing delay to **_infinite_**

使用 traceroute 可以追蹤封包如何傳送，也可以檢查哪邊是 bottleneck

#### packet loss

- packet arriving to full queue dropped
- lost packet may be retransmitted by previous node, by
source end system, or not at all

#### throughput

rate (bits/time unit) at which bits transferred between sender/receiver

## 1.5 protocol layers, service models

以上 terminology 的整合與溝通 -----> layering

#### why layering?

- structure for identification
- modularize for maintenance

Each layer implements a service via its own internal-layer action, that relies on layer below.

#### Internet protocol stack

分為五層

- application: FTP, SMTP, HTTP
- transport: TCP, UDP
- network: IP, routing protocol
- link: ethernet, 802.111 (wifi), PPP
- physical: bits

#### ISO/OSI reference model

多出了兩層

- presentation: encryption, compression, machine-specific convention
- session: synchronization


