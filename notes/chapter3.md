# Chapter 3 Transport Layer

## 3.1 Transport layer services

- provide logical communication between app processes
- end system
	- send: breaks message into segments, pass to network layer
	- recv: reassemble segments from network layer, pass to application layer

#### Transport layer vs. Netork layer

Network layer: logical communication between hosts
Transport layer: logical communication between processes

Transport layer protocols

- TCP ( reliable, in-order delivery )
	- congestion control
	- flow control
	- connection setup
- UDP ( unreliable, unordered delivery )
	- no-frills extension of “best-effort” IP 
- services not available
	- delay guarantees
	- bandwidth guarantees

## 3.2 Multiplexing and Demultiplexing

- Multiplexing ( sender ): handle data for multiple sockets, add transport header for identification
- demultiplexing ( receiver ): use header info to deliver received segments to correct socket

### Demultiplexing

Host uses IP addresses & port numbers to direct segment to appropriate socket.

1. Connectionless, identify with ( UDP socket )
	- dest. IP, port

	IP datagrams with same dest. port #, but different source IP addresses and/or source port numbers will be directed to same socket at dest.


2. Connection - oriented demux, identify with ( TCP socket )
	- src. IP, port
	- dest. IP, port


## 3.3 Connectionless transport: UDP

Usage: DNS, SNMP, streaming multi - media apps

- best effort, lost and out - of - order delivery may occur
- connectionless: no handshaking, segments handled independently
- simple, no connection state, small header size.

Reliable transfer over UDP:- add reliability at application layer- application-specific error recovery!

#### UDP checksum

Goal: detect errors in transmitted segment.

1. Sender:
	- treat segment content as 16 - bit integer
	- checksum: addition of segment content
	- checksum value into UDP checksum field
2. Receiver:
	- compute checksum of received segment
	- check if computed checksum equals checksum field value

#### Used in

DNS, SNMP.

##### Or you can add reliability in the application layer!

## 3.4 Reliable data transfer

### rdt 1.0: reliable transfer over a reliable channel

Underlying channel perfectly reliable, which means no bit errors and no loss of packets.

### rdt 2.0: channel with bit errors

Underlying channel may flip bits in packet. How could we recover from errors?

Add control messages from receiver to sender.

- acknowledgements (ACKs): receiver explicitly tells sender that pkt received OK
- negative acknowledgements (NAKs): receiver explicitly tells sender that pkt had errors

However there is a fatal flaw! What if ACK and NAK corrupts!!? This gives us a version of improvement.

### rdt 2.1: garbled(雜亂的) ACK/NAK

#### Sender

- sequence added to the packet.
- two sequences (0/1) is suffice
- must check if received ACK/NAK is corrupted
- twice more states than rdt 2.0

#### Receiver

- must check if receive duplicate packets

### rdt 2.2: NAK-free

Remove NAK from the protocol, using ACK only.

Receiver must explicitly include seq # of pkt being ACKed

### rdt 3.0: channels with error and loss

New assumption: underlying channel can also lose packets (data, ACKs)

Sender waits “reasonable” amount of time for ACK to retransmits if no ACK received in time.

In this way, the transfering performance will be very bad, so we introduce pipelining!!!

## 3.5 Pipelined protocol

Sender allows multiple, “in-flight”, yet-to-be-acknowledged pkts.

### Go Back N (GBN)

Sender can have up to N unacked packets in pipeline. Receiver sends **cumulatoive** (offset) acks.

Sender has timer for **oldest unacked packet** (oldest in-flight packet). When timer expires, retransfer all unacked packets.

### Selective repeat (比較詳細的 GBN，每個 packet 一個 timer)

Sender can have up to N unacked packets in pipeline. Receiver sends **individual** acks.

Sender maintains timer for each unacked packet. When timer expires, retransmit only **that unacked packet**.


## 3.6 Connection-oriented: TCP

TCP socket identifies a 4-tuple: - source IP address- source port number 
- dest IP address- dest port number

This is unlike UDP, that only needs Dest-IP and port.

Web server will hold clients with different sockets. In non-persistent HTTP (stateless), holds different socket for different requests.

### Flow control

Receiver controls sender, so sender won’t overflow receiver’s buffer by transmitting too much, too fast.

Receiver “advertises” free buffer space by including **rwnd** value in TCP header of receiver-to-sender segments. Sender limits amount of unacked (“in-flight”) data to receiver’s **rwnd** value. This gaurantees the receive buffer will not overflow.

### Congestion control

TCP slow start: exponentially increase

Additive increase: linear increase after loss

multiplicative decrease: cut half after loss

- loss indicated by timeout: set to 1
- loss indicated by 3 duplicate ACKs (TCP Reno): set to half. This kind of loss is less drastic, so +3 MSS after cutting to half is a good adjustment.
- TCP Tahoe always set to 1, no matter timeout or duplicate ACK. After setting to 1, it is TCP fast recover, which congestion window will exponentially increase until the threshold.







