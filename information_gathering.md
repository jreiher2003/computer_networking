#Network analysis information gathering (using linux-kali-rolling)

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





