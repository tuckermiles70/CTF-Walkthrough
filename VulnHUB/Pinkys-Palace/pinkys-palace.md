Pinkys-Palace

https://www.hackthebox.eu/home/machines/profile/114

* Author: mrb3n
* Series: VulHub

## Scan target using nmap

### Service Scan
```
nroot@kali:~/Downloads/CTF-Walkthrough/VulnHUB/Pinkys-Palace# nmap -sV 172.16.0.189
Starting Nmap 7.70 ( https://nmap.org ) at 2018-05-27 20:11 MDT
Nmap scan report for 172.16.0.189
Host is up (0.00049s latency).
Not shown: 998 closed ports
PORT      STATE    SERVICE VERSION
80/tcp    open     http    Apache httpd 2.4.25 ((Debian))
31337/tcp filtered Elite
MAC Address: 00:0C:29:3B:45:93 (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 26.03 seconds
```

http port found
Going to run tools on them
80

## Investigate Port 80

### Web navigate
```
Pinky's Blog
``` 

### Running Nikto on Apache httpd 2.4.25
```
root@kali:~/Downloads/CTF-Walkthrough/VulnHUB/Pinkys-Palace# nikto -h 172.16.0.189
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          172.16.0.189
+ Target Hostname:    172.16.0.189
+ Target Port:        80
+ Start Time:         2018-05-27 20:16:48 (GMT-6)
---------------------------------------------------------------------------
+ Server: Apache/2.4.25 (Debian)
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ Uncommon header 'link' found, with contents: <http://pinkydb/index.php?rest_route=/>; rel="https://api.w.org/"
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Web Server returns a valid response with junk HTTP methods, this may cause false positives.
+ DEBUG HTTP verb may show server debugging information. See http://msdn.microsoft.com/en-us/library/e8z01xdh%28VS.80%29.aspx for details.
+ OSVDB-3268: /secret/: Directory indexing found.
+ OSVDB-3092: /secret/: This might be interesting...
+ Server leaks inodes via ETags, header found with file /icons/README, fields: 0x13f4 0x438c034968a80 
+ OSVDB-3233: /icons/README: Apache default file found.
+ /wp-content/plugins/akismet/readme.txt: The WordPress Akismet plugin 'Tested up to' version usually matches the WordPress version
+ /wp-links-opml.php: This WordPress script reveals the installed version.
+ OSVDB-3092: /license.txt: License file found may identify site software.
+ Cookie wordpress_test_cookie created without the httponly flag
+ /wp-login.php: Wordpress login found
+ 7536 requests: 0 error(s) and 15 item(s) reported on remote host
+ End Time:           2018-05-27 20:17:24 (GMT-6) (36 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested

```

### Investigate /secret 
```
bambam.txt discovered 
Contents:
8890
7000
666

pinkydb

```

Looks like port numbers
Will investigate to scan port numbers

### nmap scan ports
```
root@kali:~/Downloads/CTF-Walkthrough/VulnHUB/Pinkys-Palace# nmap -p 666,8890,7000 172.16.0.189
Starting Nmap 7.70 ( https://nmap.org ) at 2018-05-27 20:25 MDT
Nmap scan report for 172.16.0.189
Host is up (0.0023s latency).

PORT     STATE  SERVICE
666/tcp  closed doom
7000/tcp closed afs3-fileserver
8890/tcp closed ddi-tcp-3
MAC Address: 00:0C:29:3B:45:93 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 13.20 seconds
```

Ports discovered however they are closed
re-ran services scan

### namp scan 
```
root@kali:~/Downloads/CTF-Walkthrough/VulnHUB/Pinkys-Palace# nmap -sV 172.16.0.189
Starting Nmap 7.70 ( https://nmap.org ) at 2018-05-27 20:28 MDT
Nmap scan report for 172.16.0.189
Host is up (0.0010s latency).
Not shown: 998 closed ports
PORT      STATE SERVICE VERSION
80/tcp    open  http    Apache httpd 2.4.25 ((Debian))
31337/tcp open  Elite?
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31337-TCP:V=7.70%I=7%D=5/27%Time=5B0B6972%P=x86_64-pc-linux-gnu%r(N
SF:ULL,59,"\[\+\]\x20Welcome\x20to\x20The\x20Daemon\x20\[\+\]\n\0This\x20i
SF:s\x20soon\x20to\x20be\x20our\x20backdoor\n\0into\x20Pinky's\x20Palace\.
SF:\n=>\x20\0")%r(GetRequest,6B,"\[\+\]\x20Welcome\x20to\x20The\x20Daemon\
SF:x20\[\+\]\n\0This\x20is\x20soon\x20to\x20be\x20our\x20backdoor\n\0into\
SF:x20Pinky's\x20Palace\.\n=>\x20\0GET\x20/\x20HTTP/1\.0\r\n\r\n")%r(SIPOp
SF:tions,138,"\[\+\]\x20Welcome\x20to\x20The\x20Daemon\x20\[\+\]\n\0This\x
SF:20is\x20soon\x20to\x20be\x20our\x20backdoor\n\0into\x20Pinky's\x20Palac
SF:e\.\n=>\x20\0OPTIONS\x20sip:nm\x20SIP/2\.0\r\nVia:\x20SIP/2\.0/TCP\x20n
SF:m;branch=foo\r\nFrom:\x20<sip:nm@nm>;tag=root\r\nTo:\x20<sip:nm2@nm2>\r
SF:\nCall-ID:\x2050000\r\nCSeq:\x2042\x20OPTIONS\r\nMax-Forwards:\x2070\r\
SF:nContent-Length:\x200\r\nContact:\x20<sip:nm@nm>\r\nAccept:\x20applicat
SF:ion/sdp\r\n\r\n")%r(GenericLines,5D,"\[\+\]\x20Welcome\x20to\x20The\x20
SF:Daemon\x20\[\+\]\n\0This\x20is\x20soon\x20to\x20be\x20our\x20backdoor\n
SF:\0into\x20Pinky's\x20Palace\.\n=>\x20\0\r\n\r\n")%r(HTTPOptions,6F,"\[\
SF:+\]\x20Welcome\x20to\x20The\x20Daemon\x20\[\+\]\n\0This\x20is\x20soon\x
SF:20to\x20be\x20our\x20backdoor\n\0into\x20Pinky's\x20Palace\.\n=>\x20\0O
SF:PTIONS\x20/\x20HTTP/1\.0\r\n\r\n")%r(RTSPRequest,6F,"\[\+\]\x20Welcome\
SF:x20to\x20The\x20Daemon\x20\[\+\]\n\0This\x20is\x20soon\x20to\x20be\x20o
SF:ur\x20backdoor\n\0into\x20Pinky's\x20Palace\.\n=>\x20\0OPTIONS\x20/\x20
SF:RTSP/1\.0\r\n\r\n")%r(RPCCheck,5A,"\[\+\]\x20Welcome\x20to\x20The\x20Da
SF:emon\x20\[\+\]\n\0This\x20is\x20soon\x20to\x20be\x20our\x20backdoor\n\0
SF:into\x20Pinky's\x20Palace\.\n=>\x20\0\x80")%r(DNSVersionBindReqTCP,59,"
SF:\[\+\]\x20Welcome\x20to\x20The\x20Daemon\x20\[\+\]\n\0This\x20is\x20soo
SF:n\x20to\x20be\x20our\x20backdoor\n\0into\x20Pinky's\x20Palace\.\n=>\x20
SF:\0")%r(DNSStatusRequestTCP,59,"\[\+\]\x20Welcome\x20to\x20The\x20Daemon
SF:\x20\[\+\]\n\0This\x20is\x20soon\x20to\x20be\x20our\x20backdoor\n\0into
SF:\x20Pinky's\x20Palace\.\n=>\x20\0")%r(Help,5F,"\[\+\]\x20Welcome\x20to\
SF:x20The\x20Daemon\x20\[\+\]\n\0This\x20is\x20soon\x20to\x20be\x20our\x20
SF:backdoor\n\0into\x20Pinky's\x20Palace\.\n=>\x20\0HELP\r\n");
MAC Address: 00:0C:29:3B:45:93 (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.76 seconds
```

Now there seems to be a service running on port 31337
Navigate to the port via webportal

### webporta
```
172.16.0.189:31337
[+] Welcome to The Daemon [+]
�This is soon to be our backdoor
�into Pinky's Palace.
=> �GET / HTTP/1.1
Host: 172.16.0.189:31337
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive
Upgrade-Insecure-Requests: 1

```	

### Netcat to address
```
root@kali:~/Downloads/CTF-Walkthrough/VulnHUB/Pinkys-Palace# nc 172.16.0.189 31337
[+] Welcome to The Daemon [+]
This is soon to be our backdoor
into Pinky's Palace.
=> 

``` 

