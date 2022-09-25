# Sniffing 

  - With **Sniffing** you can learn details of IP ranges, subnet sizes, MAC addresses, and hostnames by reviewing captured frames and packets. If the network is misconfigured or switching fabric under stress, attackers can capture sensitive material via passive network sniffing.

  - If a switched Ethernet network is configured properly, you will only see broadcast frames and material destined for your MAC address.

## TCPDump

```bash

sudo tcpdump -i <INTERFACE> udp port 53 #Listen to DNS request to discover what is searching the host

tcpdump -i <IFACE> icmp #Listen to icmp packets

sudo bash -c "sudo nohup tcpdump -i eth0 -G 300 -w \"/tmp/dump-%m-%d-%H-%M-%S-%s.pcap\" -W 50 'tcp and (port 80 or port 443)' &"

```


## Bettercap2

```bash
net.sniff on
net.sniff stats
net.sniff.output #Output file
net.sniff.local #Accept packets from this machine
net.sniff.filter
net.sniff.regexp

```

## WireShark

  - Obviously. 
  
## Capturing Credentials

  - You can use tools like https://www.github.com/lgandx/PCredz to parse credentials from a pcap or a live interface.  
    

