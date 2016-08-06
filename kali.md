# Kali tutorial use of tools
### configure tor to kali if not already
##### add tor to sources.list 
`deb http://deb.torproject.org/torproject.org <DISTRIBUTION> main`    
#####add gpg keys
`gpg --keyserver keys.gnupg.net --recv 886DDD89`  
`gpg --export A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89 | sudo apt-key add -`  
`sudo apt-get update`  
`sudo apt-get install tor` 
##### check tor if running 
`service tor status `  
`service tor start `  
### set proxychains.conf
`nano /etc/proxychains.conf`   
change *strict* to *dynamic_chain*  
add this to bottom to config socks5 also  
`socks5 127.0.0.1 9050`  
### change dns server
`nano /etc/dhcp/dhclient.conf`  
uncomment prepend domain-name-servers use opendns.com  
add 208.67.222.222, 208.67.220.220, 8.8.8.8;  


### set up VPN  
on firefox or iceweasel  
`about:config`  
search `media.peerconnection.enabled` in box  
change value to *false*  -- close browser to reset  
##### go to [www.vpnbook.com](www.vpnbook.com) its a free vpn  
go to free vpn and download any of the links available  
browser must be **closed** to set up vpn.
`cd /Downloads`  `unzip VPNBook.com.zip`  
`openvpn vpnbook-de233-tcp443.ovpn`  put username and password in  
once you see *Initialization Sequence Completed* start up your browser.  
go to dnsleak test and you should be in germany  
open another terminal `wget http://ipinfo.io/ip -qO -`  should be germany  
check connection and turn off
`nmcli con up/down id Profile\ 3` or `nmcli con`  

### macchanger  - changes hardware address of wireless card
`ifconfig` check ether or HWaddr  
search mac address venders [here](http://www.coffer.com/mac_find/?string=08%3A00%3A27) put in 08:00:27 first part.  
`macchanger -h`  
`macchanger --show eth0`  show current mac address  
`macchanger -A eth0` changes mac address to random vender  
won't connect in a VM only works with a linux full machine version.  
to change mac address in VM turn machine off go to settings and network then advanced.  push rotate button and presto. turn back on.  

### change hostname 
put into a file and chmod +x mv /usr/bin/filename  
`newhn=$(cat /dev/urandom | tr -dc 'A-Za-z' | head -c8)
echo $newhn > /etc/hostname


### create a boot time script
1. create script and save to /Documents  ex.
this script changes the mac address on every boot up.  
```
#!/bin/bash
ifconfig eth0 down 
macchanger -r eth0 
ifconfig eth0 up 
```
`chmod +x change_mac.sh`  

2.  configure script
`crontab -e` scroll down to end of file  
`@reboot /root/Documents/change_mac.sh` or file to path of .sh  

