# The Basic Scan

- By default Nmap does a standard TCP SYN scan on the top 1000 ports of the host. I never really use this by itself
  
  `$ nmap <host>`

- At an absolute minimum, I will get more verbosity using -v or -vw

  `$ nmap -vv <host>` 


## Target Specification

- Nmap accepts target specification in loads of different formats including plain IP addresses, CIDR ranges and dash notation.

```zsh
$ nmap hostname
$ nmap 123.123.123.123
$ nmap 123.123.123.1/24
$ nmap 123.123.123.1–255
```


## Ping Sweeping

- If you just want to find which hosts are alive you can perform a ping scan with `-sn`

- `$ nmap -sn 123.123.123.1/24`

