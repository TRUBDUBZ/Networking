# OSI Model (Open Systems Interconnection)

1. Physical
    
    - voltage levels on an ithernet cables,the readio frequency of Wifi etc….

2. Data Link

    - this level build on physical layer, It deals with protocols for directly communicating between two nodes.It defines how a direct message between nodes starts and ends (framing), error detection and correction, and flow control.

3. Network Layer

    - the network layer provides the methods to transmit data sequences (called packets). This is the layer that internet protocol is defined on.

4. Transport Layer

    - at this layer we have methods to reliably deliver variable length data between hosts. the **Transmission Control Protocol TCP** and **User Datagram Protocol UDP** are commonly said to exist on this layer.

5. Session Layer

    - this layer build on the the transport layer by adding methods to establish, checkpoint, suspend, resume, and terminate dialogs.

6. Presentation Layer

    - this is the lowest layer at which data structure and presentation for an application are defined. Concerns such as date encoding, serialization, and encryption are handled here.

7. Application Layer

    - The application that the user interfaces with(Ex. web browsers adn email clients etc...)


- The OSI model has 7 layers while the TCP/IP has 4 Layers

- In both models, same function are performed; they are just divided differently.


## Transmission Control Protocol TCP / Internet Protocol IP
  

1. Network Access

  - Physical connection and data framing happen.(for Example, sending Ethernet and Wi-Fi packet).

2. Internet 

  - This layer deals with the concerns of addressing packets and routing them over multiple interconnection networks. IP address is defined at this layer.

3. Host-to-Host 

  -  this provides two protocols TCP/IP and UDP.These protocols address concerns such as data order,data segmentation,network congestion, and error correction.

4. Process/Application Layer

  - protocols such as HTTP, SMTP, and FTP are implemented.



### Internet Protocol 

  - there are two versions:
    
    1. IPv4
    2. IPv6

  - IPv4 : IPv4 uses 32-bit addresses, which limits it to addressing no more than 232 or 4,294,967,296 systems. However, these 4.3 billion addresses were not initially assigned efficiently, and now many Internet Service Providers (ISPs) are forced to ration IPv4 addresses

  - IPv6 : IPv6 was designed to replace IPv4 and has been standardized by the Internet Engineering Task Force (IETF) since 1998. It uses a 128-bit address, which allows it to address a theoretical 2128 = 340,282,366,920,938,463,463,374,607,431,768,211,456, or about a 3.4 x 1038 addresses how it would be

#### What is an Address?

  - All internet protocol traffic routes to and address.
  - IPv4 addresses are 32 bits long. They are commonly divided into four 8-bit sections. Each section is displayed as a decimal number between 0 and 255 inclusive and is delineated by a period.
    - 0.0.0.0
    - 127.0.0.1
    - 255.255.255.255
  - a special address, called the **loopback address** is reserved at `127.0.0.1` this means `establish a connection to myself`
  - Operating systems short-circuit thisaddress so that packets to it never enter the network but instead stay local on theoriginating system.


