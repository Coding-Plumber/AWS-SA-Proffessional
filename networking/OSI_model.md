```
+---------------------------+
|                           |
|    OSI 7-Layer Model      |
|                           | 
+---------------------------+ \
|     LAYER7 - APPLICATION  |  \ 
+---------------------------+   \
|    LAYER6 - PRESENTATION  |   Host Layers  
+---------------------------+   / 
|      LAYER5 - SESSION     |  /
+---------------------------+ /   
|     LAYER4 - TRANSPORT    |/     
+---------------------------+   
|      LAYER3 - NETWORK     |\  
+---------------------------+ \  
|     LAYER2 - DATA LINK    | Media layers 
+---------------------------+ / 
|      LAYER1 - PHYSICAL    |/
+---------------------------+
        
```

# Layer 1: Physical

Layer 1 (Physical) specifications define the transmission and reception of raw bit streams between a device and a shared physical medium. It encompasses:

- Voltage levels
- Timing
- Data rates
- Transmission distances
- Signal modulation
- Physical connectors

The physical medium can be:
- Copper (electrical)
- Fiber (light)
- WiFi (Radio Frequency)

> Note: When we refer to a device as "Layer X" (e.g., Layer 3), it means that device has functionality for that layer and all layers below it. Layer 1 only understands Layer 1, while Layer 3 understands Layer 3 and under.

## Layer 1 Physical Hub

A Layer 1 hub, for example a 4-port hub, allows connection of 4 devices. Its primary function is to replicate anything received on any port to every other port, including errors and collisions.

### Characteristics of Layer 1:

- No media access control
- No collision detection
- Absence of device addressing (all data is processed by all connected devices)
- Collisions occur when multiple devices transmit simultaneously

Layer 1 is considered "dumb" due to its limited functionality, which impacts its scalability.

### Key aspects of Layer 1:

- Provides a physical shared medium
- Defines standards for transmitting onto the medium
- Specifies standards for receiving from the medium
- Lacks access control
- No unique device identification
- Absence of direct device-to-device communication

These limitations at Layer 1 necessitate the implementation of higher layers in the OSI model to enable more sophisticated networking capabilities.

## Layer 2: Data Link

The Data Link layer is one of the most critical layers in the OSI model. It builds upon Layer 1 and introduces several important concepts:

- Requires a functional Layer 1 to operate
- Can run on various Layer 1 mediums (electrical, light, radio frequency)

### Ethernet

Ethernet is a prominent Layer 2 protocol that introduces the concept of frames.

#### Key Concepts:

1. **MAC Addresses**: 
   - Unique hardware addresses (e.g., 3e:22:fb:b9:4b:75)
   - 48 bits in hexadecimal format
   - First 24 bits identify the manufacturer
   - Should be globally unique
   - Frames can be addressed to a specific destination or broadcast (all F's)

2. **Frames**: 
   - Used to encapsulate data for transmission to the physical layer
   - Structure of an Ethernet frame:

     ```
     <--------------------- MAC HEADER ----------------------->
     PREAMBLE       Destination  Source     ET        PAYLOAD           FCS 
     56 bits        MAC          MAC        16 bit    46-1500 BYTES     32 BITS
     FSD - 8 bits   Address     Address    Bits      
     ```

   - **Payload**: Data carried from source to destination, typically provided by Layer 3
   - **EtherType (ET)**: Defines which Layer 3 protocol is used (e.g., IP)

3. **Layer Interaction**:
   - Layer 2 software passes data to Layer 1 hardware for transmission

### Collision Handling

In Layer 1, if two devices connected by a physical medium (e.g., a cable) send data simultaneously, it can result in a collision, potentially causing physical corruption of the signal due to overlap.

Layer 2 aims to solve this problem by implementing collision detection and handling mechanisms, improving the reliability of data transmission over shared media.

> Note: The introduction of Layer 2 functionality marks a significant improvement over the limitations of Layer 1, enabling more sophisticated network communication and laying the groundwork for higher-layer protocols.


### Process Overview

1. **Initiating Communication**
   - Left computer (Game) intends to send data to the right computer
   - Uses destination MAC address (e.g., 3e:22:fb:b9:5b:7e) to create a frame at Layer 2

2. **Carrier Sense**
   - Sending computer checks if the medium (cable) is free of traffic
   - This is the "Carrier Sense" part of CSMA/CD

3. **Transmission**
   - If no carrier detected (line is clear):
     - Layer 2 passes the frame to Layer 1
     - Layer 1 converts digital data to appropriate physical signals and transmits

4. **Reception**
   - Right computer's Layer 1 receives the physical signals
   - Converts signals back into digital data
   - Passes the frame up to Layer 2

5. **Processing**
   - Receiving computer's Layer 2 processes the frame
   - Checks if destination MAC address matches its own
   - If matched, data is passed up to higher layers (e.g., game application)

### Key Points

- Each computer has a unique MAC address (e.g., 3e:22:fb:b9:5b:75 and 3e:22:fb:b9:5b:7e)
- "CD" (Collision Detection) activates if both computers transmit simultaneously:
  - Collision occurs
  - Both stop transmission
  - Wait for a random time
  - Attempt to retransmit
- CSMA/CD allows multiple devices to share the same physical medium without a central controller
- Modern switched networks simplify this process:
  - Switches create separate collision domains for each port
  - Significantly reduces the chance of collisions
  - Data gets encapsulated in each layer, in layer 2 its the frames 


### Network Devices: Hubs vs. Switches

#### Hubs (Layer 1 Device)
- A 4-port hub operates only at Layer 1
- Acts as a simple repeater
- Broadcasts raw data received on one port to all other ports
- No understanding of frame structure or addressing

#### Switches (Layer 2 Device)
- Operates at both Layer 1 and Layer 2
- Understands and processes Ethernet frames

1. **MAC Address Table**
   - Maintains a table mapping MAC addresses to physical ports
   - Learns and updates this table dynamically based on incoming frames

2. **Intelligent Frame Handling**
   - Interprets source and destination MAC addresses in frames
   - Stores frames temporarily and forwards them only to the appropriate port(s)
   - Does not blindly repeat data like hubs

3. **Collision Domain Isolation**
   - Each port on a switch represents a separate collision domain
   - Collisions are isolated to the port where they occur
   - An X-port switch effectively creates X collision domains

4. **Scalability**
   - Allows for interconnection of multiple switches
   - Enables creation of larger, more efficient networks

### Comparison: Hub vs. Switch Performance

1. **Data Transmission**
   - Hub: Broadcasts to all ports, increasing collision risk
   - Switch: Sends frames only to the intended destination port

2. **Collision Handling**
   - Hub: Collisions affect all connected devices
   - Switch: Collisions are isolated to individual ports

3. **Network Efficiency**
   - Hub: Performance degrades as more devices are added
   - Switch: Maintains performance by intelligently directing traffic

4. **Security**
   - Hub: All data is visible to all connected devices
   - Switch: Data is only sent to the intended recipient, improving security


## Layer 2 summary 

Layer 2, the Data Link layer, forms the crucial bridge between the physical transmission of data and the logical networking of higher layers. It packages raw bits from Layer 1 into structured frames, using MAC addresses to enable direct communication between devices on the same network segment. This layer implements error detection, flow control, and media access control mechanisms like CSMA/CD, ensuring reliable point-to-point data transfer. By employing devices such as switches and bridges, Layer 2 efficiently manages local network traffic, isolating collision domains and improving overall network performance. Essentially, Layer 2 provides the foundation for all higher-level network communications by establishing dependable links between adjacent network nodes.


## Layer 3 Network 

Layer 3, also known as the Network layer, builds upon the functionality of Layers 1 and 2 to enable communication between different networks. It introduces several important concepts and protocols, most notably IP (Internet Protocol).

## Key Concepts

### IP Addresses
- Logical addresses assigned to devices (e.g., 192.168.1.1)
- IPv4 uses 32-bit addresses, while IPv6 uses 128-bit addresses
- Can be assigned statically or dynamically (e.g., via DHCP)
- Divided into network and host portions

### Packets
- Basic unit of data at Layer 3
- Structure of an IP packet:

  ```
  +---------------------------+
  |       IP Header           |
  |---------------------------|
  |    Source IP Address      |
  |---------------------------|
  |  Destination IP Address   |
  |---------------------------|
  |          Payload          |
  +---------------------------+
  ```

- Payload typically contains data from Layer 4 (e.g., TCP or UDP segment)

### Routing
- Process of forwarding packets between different networks
- Routers use routing tables to determine the best path for packets
- Routing protocols (e.g., OSPF, BGP) help maintain and update routing tables

## Layer 3 Devices: Routers

Routers are the primary Layer 3 devices. They:
- Operate at Layers 1, 2, and 3
- Connect different networks (e.g., LAN to WAN)
- Make forwarding decisions based on IP addresses
- Maintain routing tables
- Can segment broadcast domains

### Router Functionality
1. **Packet Reception**: Receives frames from Layer 2
2. **De-encapsulation**: Extracts the IP packet from the frame
3. **Routing Decision**: Consults routing table to determine next hop
4. **Re-encapsulation**: Places IP packet into a new frame with appropriate Layer 2 addressing
5. **Transmission**: Sends the new frame out on the appropriate interface

## Subnetting

Subnetting is a crucial concept in Layer 3 networking:
- Allows efficient use of IP address space
- Enables network segmentation for improved performance and security
- Uses subnet masks to define network and host portions of IP addresses

Example:
- IP Address: 192.168.1.10
- Subnet Mask: 255.255.255.0
- Network Portion: 192.168.1.0
- Host Portion: 0.0.0.10

## Network Address Translation (NAT)

NAT is a common Layer 3 technique that:
- Allows multiple devices to share a single public IP address
- Enhances security by hiding internal network structure
- Helps conserve public IP addresses

## Comparison: Switch (Layer 2) vs. Router (Layer 3)

```
+------------------+----------------------------------------+----------------------------------------------------+
| Aspect           | Switch (Layer 2)                       | Router (Layer 3)                                   |
+------------------+----------------------------------------+----------------------------------------------------+
| Addressing       | Uses MAC addresses                     | Uses IP addresses                                  |
+------------------+----------------------------------------+----------------------------------------------------+
| Network Scope    | Operates within a single network (LAN) | Connects different networks (LAN to WAN)           |
+------------------+----------------------------------------+----------------------------------------------------+
| Decision Making  | Forwards based on MAC address table    | Forwards based on IP routing table                 |
+------------------+----------------------------------------+----------------------------------------------------+
| Broadcast        | Forwards broadcasts within the LAN     | Does not forward broadcasts between networks       |
| Handling         |                                        | (by default)                                       |
+------------------+----------------------------------------+----------------------------------------------------+
| Protocol Support | Primarily works with Ethernet          | Works with various Layer 3 protocols (primarily IP)|
+------------------+----------------------------------------+----------------------------------------------------+
```

## Layer 3 Summary

Layer 3, the Network layer, enables inter-network communication through logical addressing and routing. It encapsulates data from higher layers into packets, which are then routed between different networks based on IP addresses. Key aspects include:

IP Addresses (IPv4/v6): Used for cross-network addressing.
ARP (Address Resolution Protocol): Finds the MAC address for a given IP address.
Routing: Determines where to forward packets.
Route Tables: Contain multiple routes for packet forwarding decisions.
Routers: Move packets from source (SRC) to destination (DST), encapsulating them in Layer 2 frames along the way.
Device-to-Device Communication: Enables communication over the internet.
No Channel Method: Layer 3 doesn't provide a method for communication channels, only source IP to destination IP.
Packet Delivery: Can be delivered out of order.

Routers, the primary Layer 3 devices, make intelligent forwarding decisions using routing tables and protocols. This layer introduces concepts like subnetting and NAT, which are crucial for efficient and secure network design. By providing these capabilities, Layer 3 forms the backbone of modern internetworking, allowing for the creation of large-scale, interconnected networks like the Internet.


## Layer 4 & 5 
