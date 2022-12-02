# Domain Name System (DNS)

- The DNS is the system in the internet that maps names of objects(usually host names) into IP numbers or other resource record values. The name space of the internet is divided into domains, and the responsibility from managing names within each domain is delegated, typically to systems within each domain

- For example, all internet systems that belong to the Universtiy of Arizona's CCIT Telecom (a system which also happens to be called arizona.edu with the IP addresses 128.196.128.233 and 128.196.128.234)
- The Telecom name server can, in turn, delegate portions of the arizona.edu name space to departmental name servers on campus. By this system, the department gains a measure of autonomy in inventing and managing the names within its subdomain. For example, some or all of the subdomains of arizona.edu can be nameserved by various departments (such as Computer Science, Math, or Physics)

- In addition to the Internet being divided namewise into domains and subdomains, such as arizona.edu for University of Arizona and apple.com for Apple Computer, it is divided numberwise into networks and subnets, such as 128.196.0.0 or 130.43.0.0. for University of Arizona and Apple, respectively. The namewise layout of the Internet tracks administrative responsibility (ownership), while the numberwise layout tracks physical topology.

- There is no necessary relationship between the name(s) of an object in the Internet and its number(s). For example, the 128.196.0.0 network physically resides at the University of Arizona. However, if a machine that belongs to Apple were to be plugged into the University of Arizona network, its name would still be something.apple.com, even though its number would be 128.196.xxx.yyy. In this case, however, Apple and the University of Arizona would share nameservice responsibility for this system: Apple for the name-to-number nameservice, and University of Arizona for the number-to-name nameservice.

- The primary job that DNS performs is to map between names and numbers. Most importantly, it must provide the translation from host names to IP addresses, so that applications can effect a network connection from a command such as ftp prep.ai.mit.edu. Also, DNS must map from IP addresses back to names in order to provide some level of authentication, as with the r commands.

- Reverse mapping from IP addresses to host names is performed under the auspices of the IN-ADDR.ARPA pseudo-domain. Because the order of significance in the naming system is highest on the right, the notation for addresses is reversed. Therefore, the DNS entry for the IP address 128.196.120.82 is given as 82.120.196.128.IN-ADDR.ARPA.


