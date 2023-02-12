# Discovering Hosts

## ICMP
 
-  This is the easiest and fastest way to discover if a host is up or not.
  
- You could try to send some **ICMP** packets and expect responses. The easiest way is just sending an echo request and expect from the response. You can do that using a simple `ping` or using `fping` for ranges.
 
- You could also use `nmap` to send other types of ICMP packets (this will avoid filters to common ICMP echo request-response).
        
```bash

  ping -c 1 199.66.11.4 # 1 echo request to a host
  fping -g 199.66.11.0/24 # Send echo requests to ranges
  nmap -PEPM -sP -n 199.66.11.0/24  # Send echo, timestamp requests and subnet mask requests
  
```

## TCP Port Discovery

- It's very common to find that all kind of ICMP packets are being filtered. Then, all you can do to check if a host is up is try to find open ports. Each host has 65535 ports, so, if you have a "big" scope you cannot test if each port of each host is open or not, that will take too much time.
Then, what you need is a fast port scanner [massscan](https://github.com/robertdavidgraham/masscan) and a list of the ports more used:

```bash

# Using massscan to scan top20ports of nmap in a /24 range (less than 5 mins)
  
  massscan -p20,21-23,25,53,80,110,111,135,139,143,443,445,993,995,1723,3306,3389,5900,8080 199.66.11.0/24

```
-  You could also perform this step with `nmap`, but its slower and somewhat has problems identifying hosts.    
        
## HTTP Port Discovery

- This is just a TCP port discovery useful when you want to focus on *discovering HTTP services*:


```bash

masscan -p80,443,8000-8100,8443 199.66.11.0/24

```

## UDP Port Discovery

- Youy could also try to check for some **UDP port open** to deecide if you should **pay more attention** to a **host**.   

- As UDP servies usually dont repsond with any data to a regulary empty UDP probe packet it is difficult to say if a port is being filtered or open.   
  
- The easiest way to decide this is to send a packet related to the running service, and as you don't know which service is running, you should try the most probable based on the port number:   
   
```bash

    nmap -sU -sV --version-intensity 0 -F -n 199.66.11.53/24
# The -sV will make nmap test each possible known UDP service packet
# The "--version-intensity 0" will make nmap only test the most probable


```

## SCTP Port Discovery

```bash

nmap -T4 -sY --open -Pn <IP/range>

```

### Pentesting Wifi

- here you can find a guide of [well known wifi attacks](https://book.hacktricks.xyz/generic-methodologies-and-resources/pentesting-wifi)


#### Discovering Hosts From the Inside

- If you are inside one of the first things you will want to do is to **discover other hosts**
  
- Depending on **how much noise** you can/want to do, different actions could be performed.
  
##### Passive

- You can use these tools to passively discover hosts inside a connected network
    
```bash
netdiscover -p
p0f -i eth0 -p -o /tmp/p0f.log
# Bettercap2
net.recon on/off
net.show
set net.show.meta true #more info
```

##### Active

- Note that the techniques commented in *Discovering hosts from the outside(TCP/HTTP/UDP/SCTP Port Discovery)* can also be applied here 
    
```bash

# ARP discovery
nmap -sn <Network> #ARP Requests (Discover IPs)
netdiscover -r <Network> #ARP requests (Discover IPs)

# NBT discovery 
nbtdiscover -r 192.168.0.1/24 #Search in domain

# Bettercap2 (By default ARP requests are sent)
net.probe on/off  # Activates all service discover and ARP
net.probe.mdns  # Search local mDNS servies(Disover local)
net.probe.nbns  # Ask for NetBios name (Discover local)
nep.probe.upnp  # Search services (Discover local)
net.probe.wsd  # Search Web Services Directory (Discover local)
net.probe.throttle # 10 #10ms between requests sent (Discover local)

# IPv6 
alive6 <IFACE> # Send a pingv6 to multicast
  
```

##### Active ICMP

- Note that the techniques commented in [Discovering hosts from the outside [ICMP]](https://book.hacktricks.xyz/generic-methodologies-and-resources/pentesting-network#icmp) can also be applied here

- But, as you are in the **same network** as the other hosts, you cna do **more things:**

- If you **ping** a **subnet broadcast address** the ping should be arriving to **each host** and they could **respond** to you: `ping -b 10.10.5.255`
  
- Pinging the **network** broadcast address you could even find hosts inside **other subnets:** `ping -b 255.255.255.255`
  
- Use the `-PEPM` flag of `nmap` to perform host discovery sending **ICMPv4 echo, timestamp,** and **subnet mask requests:** `nmap -PEPM -sP -vvv -n 10.12.5.0/24`
    
##### Wake On LAN

- Wake On Lan is used to turn on computers through a network message. The magic packet used to turn on the computer is only a packet where a MAC Dst is provided and then it is repeated 16 times inside the same paket.

- Then this kind of packets are usually sent in an ethernet 0x0842 or in a UDP packet to port 9.
If no [MAC] is provided, the packet is sent to broadcast ethernet (and the broadcast MAC will be the one being repeated).

```bash

#WOL (without MAC is used ff:...:ff)
wol.eth [MAC] # Send a WOL as a raw ethernet packet of type 0x0847
wol.udp [MAC] # Send a WOL as an IPv4 broadacast packet to UDP port 9
# Bettercap2 can also be used for this purpose

```

###### Scanning Hosts

- Once you have discovered all the IPs(external or internal) you want to scan in depth, different actions can be performed.

- Open port: *SYN --> SYN/ACK --> RST*
  
- Closed port: *SYN --> RST/ACK*
    
- Filtered port: *SYN --> [NO RESPONSE]*
  
- Filtered port: *SYN --> ICMP message*
  
```bash
# Nmap fast scan for the most 1000tcp ports used
nmap -sV -sC -O -T4 -n -Pn -oA fastscan <IP> 
# Nmap fast scan for all the ports
nmap -sV -sC -O -T4 -n -Pn -p- -oA fullfastscan <IP> 
# Nmap fast scan for all the ports slower to avoid failures due to -T4
nmap -sV -sC -O -p- -n -Pn -oA fullscan <IP>

#Bettercap2 Scan
syn.scan 192.168.1.0/24 1 10000 #Ports 1-10000
```

###### UDP 

- There are 2 options to scan UDP ports:
  
- Send a **UDP packet** and check for the response *ICMP unreachble* if the port is **closed**(in several cases ICMP will be filtered so you won't receive any information inf the port is close or open). 
  
- Send a formatted datagrams to elicit a response from a service (e.g., DNS, DHCP, TFTP, and others, as listed in nmap-payloads). If you receive a response, then, the port is open.
  
- Nmap will mix both options using "-sV" (UDP scans are very slow), but notice that UDP scans are slower than TCP scans:

```bash 
# Check if any of the most common udp services is running
udp-proto-scanner.pl <IP> 

# Nmap fast check if any of the 100 most common UDP services is running
nmap -sU -sV --version-intensity 0 -n -F -T4 <IP>

# Nmap check if any of the 100 most common UDP services is running and launch defaults scripts
nmap -sU -sV -sC -n -F -T4 <IP> 

# Nmap "fast" top 1000 UDP ports
nmap -sU -sV --version-intensity 0 -n -T4 <IP>

# You could use nmap to test all the UDP ports, but that will take a lot of time
```

###### SCTP Scan 

- SCTP sits alongside TCP and UDP. Intended to provide transport of telephony data over IP, the protocol duplicates many of the reliability features of Signaling System 7 (SS7), and underpins a larger protocol family known as SIGTRAN. SCTP is supported by operating systems including IBM AIX, Oracle Solaris, HP-UX, Linux, Cisco IOS, and VxWorks.

- Two different scans for SCTP are offered by nmap: -sY and -sZ

```bash
# Nmap fast SCTP scan
nmap -T4 -sY -n -oA SCTFastScan <IP>
# Nmap all SCTP scan

```




