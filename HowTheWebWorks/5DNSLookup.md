# DNS Lookup

*Note: The website can still use the HSTS policy without being in the HSTS list. The first HTTP request to the website by a user will receive a response requesting that the user only send HTTPS requests. However, this single HTTP request could potentially leave the user vulnerable to a downgrade attack, which is why the HSTS list is included in modern web browsers.

Modern browsers requests https first*


- The browser tries to figure out the IP address for the entered domain. The DNS lookup proceeds as folloows:
  
  - Broswer Cache: The  browser caches DNS records for some time. Interestingly, the OS does not tell the browser the time-to-live for each DNS record, and so the browser caches them for a fixed duration (varies between browsers, 2 – 30 minutes).  - 
  
  - OS cache:  If the browser cache does not contain the desired record, the browser makes a system call (gethostbyname in Windows). The OS has its own cache.
  
  - Router cache: The request continues on to your router, which typically has its own DNS cache.   
  
  - ISP DNS cache: The next place checked is the cache ISP’s DNS server. With a cache, naturally. 
  
  - Recursive Search: Your ISP's DNS server begins a recursive search, from the root nameserver, through the .com top-level nameserver, to Google's nameserver. Normally, the DNS server will have names of the .com nameservers in cache, and so a hit to the root nameserver will not be necessary 
  
- Heres a diagram of what a recursive DNS search looks like [HTWW - DNS Diagram](https://github.com/vasanthk/how-web-works/blob/master/img/Example_of_an_iterative_DNS_resolver.svg)


One worrying thing about DNS is that the entire domain like wikipedia.org or facebook.com seems to map to a single IP address. Fortunately, there are ways of mitigating the bottleneck:

   - Round-robin DNS is a solution where the DNS lookup returns multiple IP addresses, rather than just one. For example, facebook.com actually maps to four IP addresses.
  
   - Load-balancer is the piece of hardware that listens on a particular IP address and forwards the requests to other servers. Major sites will typically use expensive high-performance load balancers.
  
   - Geographic DNS improves scalability by mapping a domain name to different IP addresses, depending on the client’s geographic location. This is great for hosting static content so that different servers don’t have to update shared state.

   - Anycast is a routing technique where a single IP address maps to multiple physical servers. Unfortunately, anycast does not fit well with TCP and is rarely used in that scenario.
 
   - Most of the DNS servers themselves use anycast to achieve high availability and low latency of the DNS lookups. Users of an anycast service (DNS is an excellent example) will always connect to the 'closest' (from a routing protocol perspective) DNS server. This reduces latency, as well as providing a level of load-balancing (assuming that your consumers are evenly distributed around your network).
      
   
