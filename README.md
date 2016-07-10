# Computer Networking 

## Linux Ubuntu trusty64 commands
`ssh -i ~/.ssh/jeffp jeffp@52.42.43.156`

**provision box first**
`sudo apt-get install netcat-openbsd tcpdump traceroute mtr`

### Lesson1 
#### ip - show  / manipulate routing, devices, policy routing and tunnels  
show what interfaces your computer has  
`ip addr show`   
`ip addr show eth0`

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

### lesson 2
`ping google.com`  
`host google.com` `host -t a google.com`  

#### dig same info as host but formatted better for scripts  
`dig google.com`  

#### DNS record types
*CNAME - canoical name  
*AAAA (quad-A) - IPv6 address  
*A - IPv4 address  
*NS - DNS name server  what DNS servers hold the records for that domain.  


### lesson 3  
32-bit addresses 
#####netblocks   
198.51.1000/22 - 22-bit network part/10-bit host part  
/24 network = 8 bits of host part = 2^8 addresses = 256  
minus the resrved two and the router, 253 addresses remain for hosts.  

#####subnet masks  
32-bit values 
/24 netblock  = 24 ones, 8 zeros  
255.255.155.0  decimal dotted quad  
11111111 | 11111111 | 11111111 | 00000000   binary      
   ff         ff         ff         00    hex    

subnet mask /16 network  
255.255.0.0  

Stanford University  
171.64.0.0/14  
* 1st oct - all 8 bits fixed  
* 2nd oct - 6 bits fixed, 2 bits free so there are 4 values for this octet  
All address starting with 171.64, 171.65, 171.66 or 171.67 are in this /14 netblock.  

subnet mask for /14 netblock  
14-bit network part -  18-bit host part  
11111111111111 | 00000000000000000  
11111111 | 111111 | 00 00000000 | 00000000 |  
255         252         0            0        255-3 which is 3 == 11 in binary  

how many addresses in a /14?  
18-bit host par = 2^18 addresses = 262144  

show what interfaces your computer has  
`ip addr show` 
lo = loopback interface  
eth0 = ethernet interface

`ifconfig`  does same thing  

##### LAN
local area network 
where your computers and devices are   

##### WAN
wide area network  
outward facing interface that connects to rest of internet  

Find the default gateway  
`ip route show default`  

##### NAT  Network Address Translation  
makes a table of what inside addresses and ports are connected to what outside addresses and ports  
192.168.0.42:17384  <----> 206.190.36.45:443  

most common on home routers:  
192.168.0.0/24  
default gateway 192.168.0.1  

**private address netblocks:**  
10.0.0.0/8  
172.16.0.0/12    
192.168.0.0/16  


### Lesson 4  HTTP on TCP on IP  

| HTTP | Application | HTTP | SSH | NTP |
| ---- | ----------- | -----| --- | --- |
|TCP  | Transport  | TCP | UDP |
|IP | Internet | IP |
|Hardware | Router | Ethernet | WIFI |  

| Protocol | Concepts | Where the code is | Failures |
| ------- | --------- | ----------------- | -------- | 
| HTTP | resources, URL's, verbs, cookies | Flask, Apache, Browsers, | error code slow reponse |
| TCP | ports, sessions, stream sockets | OS kernel, system libraries | broken connections timeouts | 
| IP | IP addresses, packets | OS kernel, routers | various |
| WIFI | access points, WPA passwords | device drivers | network unavailable |  

##### Using Ping and DNS in tcpdump  
`sudo tcpdump -n host 8.8.8.8`  listens for traffic going from my host to ip 8.8.8.8  
in another terminal  
`ping -c3 8.8.8.8`  you should see packets in first terminal  

##### Listen for DNS traffic default port for dns is 53    
`sudo tcpdump -n port 53`  
2nd terminal  
`ping jeffreiher.com`  

##### Listen for HTTP in tcpdump  
`sudo tcpdump -n port 80`  
`sudo tcpdump -n port 80 -w write-to-file.txt`    
2nd terminal  
`printf "GET / HTTP/1.1\r\nHost: www.example.com\r\n\r\n" | nc www.example.com 80` 

#######prints out a network capture  
load up in wireshark.    
`sudo tcpdump -n port 80 -w jeffreiher_tcp_capture.pcap`    

##### Sequence Diagrams  
```
Browser          Server    
|------------------|  
|  ->->->->->->->  |  
GET / Connection: keep-alive  

| <-<-<-<-<-<-<-<- |  
contents of main page  
                            
|  ->->->->->->->  |  
GET / favicon.ico  
                            
| <-<-<-<-<-<-<-<- |    
|    favicon file  |  
```

#### connection establishment  
| What TCP Does | How TCP Does It |  
| ------------- | --------------- |  
| communicate between two hosts | IP Layer (addresses + routing) |
| multiple applications per host | port numbers |  
| in-order delivery |  sequence numbers |
| lossless delivery |  acknowledgment + retransmission |  
| keeping connections distinct | random initial sequence numbers |  

#### TCP Flags  
###### The six basic TCP flags  
1. SYN (synchronize) [S] — This packet is opening a new TCP session and contains a new initial sequence number.  
2. FIN (finish) [F] — This packet is used to close a TCP session normally. The sender is saying that they are finished sending, but they can still receive data from the other endpoint.  
3.  PSH (push) [P] — This packet is the end of a chunk of application data, such as an HTTP request.  
4.  RST (reset) [R] — This packet is a TCP error message; the sender has a problem and wants to reset (abandon) the session.  
5.  ACK (acknowledge) [.] — This packet acknowledges that its sender has received data from the other endpoint. Almost every packet except the first SYN will have the ACK flag set.  
6. URG (urgent) [U] — This packet contains data that needs to be delivered to the application out-of-order. Not used in HTTP or most other current applications.  

###### TCP timeouts  
1. the other host is powered off  
2. a windstorm blows down cable between you and your isp.  
3. You're connecting to a server that doesn't exist.  

### Lesson 5 
#### Traceroute and MTR  
`traceroute jeffreiher.com`  
`mtr jeffreiher`  

#####bandwidth * delay(latency) product:
The amount of data that can be in transit through a connection.  
`bits/seconds * seconds = bits`  

**Bandwidth** If Internet cables is a water pipe then bandwidth is the flow rate through the pip (in liters/second)  
**Latency** and delay or latency is the time it takes to get through the pip (seconds)  
**Product** their product is the volume of the pipe (in liters).  
































