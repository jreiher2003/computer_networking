# Kali tutorial use of tools
### configure tor to kali if not already
##### add tor to sources.list 
`deb http://deb.torproject.org/torproject.org <DISTRIBUTION> main`    
#####add gpg keys
`gpg --keyserver keys.gnupg.net --recv 886DDD89`  
`gpg --export A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89 | sudo apt-key add -`  
`sudo apt-get update`  
`sudo apt-get install tor` 
### set proxychains.conf
`nano /etc/proxychains.conf`   
change *strict* to *dynamic_chain*  
add this to bottom to config socks5 also  
`socks5 127.0.0.1 9050`  
### check tor if running 
`service tor status `  
`service tor start `  
### change dns server
`nano /etc/dhcp/dhclient.conf`  
uncomment prepend domain-name-servers use opendns.com  
add 208.67.222.222, 208.67.220.220, 8.8.8.8;  


### set up VPN  
on firefox or iceweasel  
`about:config`  
search `media.peerconnection.enabled` in box  
change value to *false*  -- close browser to reset  
##### go to [www.vpnbook.com](vpn) its a free vpn  
go to free vpn and download any of the links available  
browser must be **closed** to set up vpn.
`cd /Downloads`  `unzip VPNBook.com.zip`  
`openvpn vpnbook-de233-tcp443.ovpn`  put username and password in  
once you see *Initialization Sequence Completed* start up your browser.  
go to dnsleak test and you should be in germany  
open another terminal `wget http://ipinfo.io/ip -qO -`  should be germany  
check connection and turn off
`nmcli con up/down id Profile\ 3` or `nmcli con`  

