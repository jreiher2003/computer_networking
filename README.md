# Computer Networking 

## Linux Ubuntu trusty64 commands
`ssh -i ~/.ssh/jeffp jeffp@52.42.43.156`

**provision box first**
`sudo apt-get install netcat-openbsd tcpdump traceroute mtr`

### Lesson1 Introduction
#### ip - show  / manipulate routing, devices, policy routing and tunnels   
`ip addr show eth0`

```2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 06:a5:fc:0b:a6:e5 brd ff:ff:ff:ff:ff:ff
    inet 10.20.16.140/18 brd 10.20.63.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::4a5:fcff:fe0b:a6e5/64 scope link
       valid_lft forever preferred_lft forever
```
`ip route show`  `route -n`  
```default via 10.20.0.1 dev eth0
10.20.0.0/18 dev eth0  proto kernel  scope link  src 10.20.16.140```

#### ping, ping6 - send ICMP ECHO_REQUEST to network hosts
`ping -c3 8.8.8.8`  **ping** an ip address server to see if its on line or available.  

#### host - DNS lookup utility
 host is a simple utility for performing DNS lookups. It is normally used to convert names to IP addresses and vice versa. When no arguments or options are given, host prints a short summary of its command line arguments and options.  
`host -t aaaa udacity.com`   **host** check dns records a or aaaa or cname  

#### tcpdump - dump traffic on a network  
Tcpdump prints out a description of the contents of packets on a network interface that match the boolean expression.  It can also be run with the -w flag, which causes it to save the packet data to a file for later analysis, and/or with the -r flag, which causes it to read from a saved packet file rather than  to  read  packets  from  a  network  interface

`tcpdump -n -c5 -i eth0 port 22`  
`tcpdump -v host udacity.com`  

#### traceroute - print the route packets trace to network host  
traceroute  tracks  the route packets taken from an IP network on their way to a given host. It utilizes the IP protocol's time to  live  (TTL)
field  and  attempts to elicit an ICMP TIME_EXCEEDED response from each gateway along the path to the host.  
`traceroute www.udactiy.com`

#### mtr - a network diagnostic tool  
mtr combines the functionality of the traceroute and ping programs in a single network diagnostic tool.
`mtr www.udacity.com`  

#### printf - format and print data  
Print ARGUMENT(s) according to FORMAT, or execute according to OPTION:

#### nc — arbitrary TCP and UDP connections and listens.
The nc (or netcat) utility is used for just about anything under the sun involving TCP, UDP, or UNIX-domain sockets.  It can open TCP connections, send UDP packets, listen on arbitrary TCP and UDP ports, do port scanning, and deal with both IPv4 and IPv6.  Unlike telnet(1), nc scripts nicely, and separates error messages onto standard error instead of sending them to standard output, as telnet(1) does with some.

`nc -l 3456`  **tells netcat to listen on port 3456**  
`nc localhost 3456` **tells netcat to connect to port 3456**  
`nc localhost 22`  **connects to ssh port**

| or pipe means take output of first program as input to next program  
`printf 'HEAD / HTTP/1.1\r\nHost: en.wikipedia.org\r\n\r\n' | nc en.wikipedia.org 80`  # prints out head of http request.  

fetching content  
`printf "GET / HTTP/1.1\r\nHost: www.example.com\r\n\r\n" | nc www.example.com 80`  
`printf "GET / HTTP/1.1\r\nHost: www.example.com\r\n\r\n" | nc www.example.com 80 > example.txt`  

on server   
`printf 'HTTP/1.1 302 Moved\r\nLocation: https://www.eff.org/' | nc -l 2345`  
then go to 52.42.43.156:2345 and watch a redirect


#### lsof - list open files, including network sockets (listening or connected)  
An  open file may be a regular file, a directory, a block special file, a character special file, an executing text  reference,  a  library,  a
stream  or  a  network  file  (Internet socket, NFS file or UNIX domain socket.)  A specific file or all the files in  a  file  system  may  be
selected by path.
`sudo lsof -i`

