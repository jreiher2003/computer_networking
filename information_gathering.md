#Network analysis footprinting and information gathering (using linux-kali-rolling)
#### 7 steps to footprinting 
1. Information gathering
2. Determining the network range
3. Identifying active machines
4. Finding open ports and access points
5. OS fingerprinting
6. Fingerprinting services
7. Mapping the network

## Nmap - Network exploration tool and security / port scanner  
[scripts to use with nmap](nmap.org/nsedoc/)  to detect vulnerability's  
[exploit-db](www.exploit-db.com) 

##### basic scan  
`nmap scanme.nmap.org -vv` 

##### local ip network scan range on port 22 saving to file SCAN  
`nmap -oG - 192.168.1.0-255 -p 22 -vv > /home/SCAN`  
`cat SCAN | grep Up`  this is to see what ports are open  
just print out ip addresses and print to another file  
`cat SCAN | grep Up | awk -F " " '{print $2}' > /home/SCAN2`  
now pass file list to nmap and do a full scan on each ip  
`nmap -iL SCAN2 -vv`  

### curl an ip 
`curl ipinfo.io/74.207.244.221`  
returns a json object  same info as whois  
```
{
  "ip": "74.207.244.221",
  "hostname": "li86-221.members.linode.com",
  "city": "Fremont",
  "region": "California",
  "country": "US",
  "loc": "37.5483,-121.9886",
  "org": "AS63949 Linode, LLC",
  "postal": "94536"
}
```
## Nslookup
`nslookup www.cisco.com`  
```
Server:     208.67.222.222
Address:    208.67.222.222#53

Non-authoritative answer:
www.cisco.com   canonical name = www.cisco.com.akadns.net.
www.cisco.com.akadns.net    canonical name = wwwds.cisco.com.edgekey.net.
wwwds.cisco.com.edgekey.net canonical name = wwwds.cisco.com.edgekey.net.globalredir.akadns.net.
wwwds.cisco.com.edgekey.net.globalredir.akadns.net  canonical name = e144.dscb.akamaiedge.net.
Name:   e144.dscb.akamaiedge.net
Address: 23.202.80.170
```
## traceroute find path to server 
#### unable to complete on vm have to use full install
`traceroute www.host.com`  







