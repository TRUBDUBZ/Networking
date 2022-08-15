# Network Interfaces

- Network Interface: A network interface can refer to any kind of software interface to networking hardware. For instance, if you have two network cards in your computer, you can control and configure each network interface associated with them individually.

- A network interface may be associated with a physical device, or it may be a representation of a virtual interface. The “loopback” device, which is a virtual interface to the local machine, is an example of this.

- Interfaces are networking communication points for your computer. Each interface is associated with a physical or virtual networking device.

Typically, your server will have one configurable network interface for each Ethernet or wireless internet card you have.

In addition, it will define a virtual network interface called the “loopback” or localhost interface. This is used as an interface to connect applications and processes on a single computer to other applications and processes. You can see this referenced as the “lo” interface in many tools.

Many times, administrators configure one interface to service traffic to the internet and another interface for a LAN or private network.

In DigitalOcean, in datacenters with private networking enabled, your VPS will have two networking interfaces (in addition to the local interface). The “eth0” interface will be configured to handle traffic from the internet, while the “eth1” interface will operate to communicate with the private network.



## Packets 

  -  
