FluxCapacitor

https://www.hackthebox.eu/home/machines/profile/119

* Author: del_EzjAx34h
* Series: HTB

## Scan target using nmap

### Service Scan
```
root@kali:~/Downloads/CTF-Walkthrough/HTB/fluxcapacitor# nmap -sV 10.10.10.69
Starting Nmap 7.70 ( https://nmap.org ) at 2018-05-27 21:26 MDT
Nmap scan report for 10.10.10.69
Host is up (0.14s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
80/tcp open  http    SuperWAF
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port80-TCP:V=7.70%I=7%D=5/27%Time=5B0B7701%P=x86_64-pc-linux-gnu%r(GetR
SF:equest,270,"HTTP/1\.1\x20200\x20OK\r\nDate:\x20Mon,\x2028\x20May\x20201
SF:8\x2023:44:31\x20GMT\r\nContent-Type:\x20text/html\r\nContent-Length:\x
SF:20395\r\nLast-Modified:\x20Tue,\x2005\x20Dec\x202017\x2016:02:29\x20GMT
SF:\r\nConnection:\x20close\r\nETag:\x20\"5a26c315-18b\"\r\nServer:\x20Sup
SF:erWAF\r\nAccept-Ranges:\x20bytes\r\n\r\n<!DOCTYPE\x20html>\n<html>\n<he
SF:ad>\n<title>Keep\x20Alive</title>\n</head>\n<body>\n\tOK:\x20node1\x20a
SF:live\n\t<!--\n\t\tPlease,\x20add\x20timestamp\x20with\x20something\x20l
SF:ike:\n\t\t<script>\x20\$\.ajax\({\x20type:\x20\"GET\",\x20url:\x20'/syn
SF:c'\x20}\);\x20</script>\n\t-->\n\t<hr/>\n\tFluxCapacitor\x20Inc\.\x20in
SF:fo@fluxcapacitor\.htb\x20-\x20http://fluxcapacitor\.htb<br>\n\t<em><met
SF:><doc><brown>Roads\?\x20Where\x20we're\x20going,\x20we\x20don't\x20need
SF:\x20roads\.</brown></doc></met></em>\n</body>\n</html>\n")%r(HTTPOption
SF:s,135,"HTTP/1\.1\x20405\x20Not\x20Allowed\r\nDate:\x20Mon,\x2028\x20May
SF:\x202018\x2023:44:32\x20GMT\r\nContent-Type:\x20text/html\r\nContent-Le
SF:ngth:\x20179\r\nConnection:\x20close\r\n\r\n<html>\r\n<head><title>405\
SF:x20Not\x20Allowed</title></head>\r\n<body\x20bgcolor=\"white\">\r\n<cen
SF:ter><h1>405\x20Not\x20Allowed</h1></center>\r\n<hr><center>openresty/1\
SF:.13\.6\.1</center>\r\n</body>\r\n</html>\r\n")%r(RTSPRequest,B3,"<html>
SF:\r\n<head><title>400\x20Bad\x20Request</title></head>\r\n<body\x20bgcol
SF:or=\"white\">\r\n<center><h1>400\x20Bad\x20Request</h1></center>\r\n<hr
SF:><center>openresty/1\.13\.6\.1</center>\r\n</body>\r\n</html>\r\n")%r(X
SF:11Probe,135,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nDate:\x20Mon,\x2028\
SF:x20May\x202018\x2023:44:32\x20GMT\r\nContent-Type:\x20text/html\r\nCont
SF:ent-Length:\x20179\r\nConnection:\x20close\r\n\r\n<html>\r\n<head><titl
SF:e>400\x20Bad\x20Request</title></head>\r\n<body\x20bgcolor=\"white\">\r
SF:\n<center><h1>400\x20Bad\x20Request</h1></center>\r\n<hr><center>openre
SF:sty/1\.13\.6\.1</center>\r\n</body>\r\n</html>\r\n")%r(FourOhFourReques
SF:t,12F,"HTTP/1\.1\x20404\x20Not\x20Found\r\nDate:\x20Mon,\x2028\x20May\x
SF:202018\x2023:44:33\x20GMT\r\nContent-Type:\x20text/html\r\nContent-Leng
SF:th:\x20175\r\nConnection:\x20close\r\n\r\n<html>\r\n<head><title>404\x2
SF:0Not\x20Found</title></head>\r\n<body\x20bgcolor=\"white\">\r\n<center>
SF:<h1>404\x20Not\x20Found</h1></center>\r\n<hr><center>openresty/1\.13\.6
SF:\.1</center>\r\n</body>\r\n</html>\r\n");

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 32.50 seconds
```

Identified port 80 open running a Web Application Firewall
(Fuzzing)... 
Examining further

## Port 80 Investigation

### WEb Browser
```
http://10.10.10.69
OK: node1 alive 
FluxCapacitor Inc. info@fluxcapacitor.htb - http://fluxcapacitor.htb
Roads? Where we're going, we don't need roads.
```

### dirbuster on 10.10.10.69
```
dirbuster -u http://10.10.10.69 -l /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 15
Starting OWASP DirBuster 1.0-RC1
Starting dir/file list based brute forcing
Dir found: / - 200
File found: /sync.php - 200
Dir found: /sync/ - 200
Dir found: /sync/index/ - 200
Dir found: /sync/2006/ - 200
Dir found: /sync/images/ - 200
Dir found: /sync/news/ - 200
Dir found: /sync/warez/ - 200
Dir found: /sync/full/ - 200
Dir found: /sync/contact/ - 200
Dir found: /sync/12/ - 200
Dir found: /sync/serial/ - 200
Dir found: /sync/download/ - 200
Dir found: /sync/privacy/ - 200
Exception in thread "Timer-1" java.lang.ArrayIndexOutOfBoundsException: No such child: 26
	at java.desktop/java.awt.Container.getComponent(Container.java:346)
	at com.sittinglittleduck.DirBuster.monitorThreads.ProcessChecker.run(ProcessChecker.java:183)
	at java.base/java.util.TimerThread.mainLoop(Timer.java:556)
	at java.base/java.util.TimerThread.run(Timer.java:506)
Dir found: /sync/spacer/ - 200
Dir found: /sync/crack/ - 200
Dir found: /sync/cgi-bin/ - 200
Dir found: /sync/11/ - 200
Dir found: /sync/search/ - 200
Dir found: /sync/10/ - 200
Dir found: /sync/about/ - 200
Dir found: /sync/logo/ - 200
Dir found: /sync/06/ - 200
Dir found: /sync/2/ - 200
Dir found: /sync/09/ - 200
Dir found: /sync/07/ - 200
Dir found: /sync/links/ - 200
Dir found: /sync/08/ - 200
Dir found: /sync/01/ - 200
Dir found: /sync/faq/ - 200
Dir found: /sync/rss/ - 200
Dir found: /sync/blog/ - 200
Dir found: /sync/home/ - 200
Dir found: /sync/img/ - 200
Dir found: /sync/new/ - 200
Dir found: /sync/default/ - 200
Dir found: /sync/2005/ - 200
Dir found: /sync/products/ - 200
Dir found: /sync/sitemap/ - 200
Dir found: /sync/archives/ - 200
Dir found: /sync/1/ - 200
Dir found: /sync/help/ - 200
Dir found: /sync/login/ - 200
Dir found: /sync/articles/ - 200
Dir found: /sync/05/ - 200
Dir found: /sync/keygen/ - 200
Dir found: /sync/support/ - 200
Dir found: /sync/03/ - 200
Dir found: /sync/events/ - 200



```

### examine futher
```
http://10.10.10.69/sync
403 Forbidden
openresty/1.13.6.1
```

This doesn't make sense.... 
Compare between browsers...
Try to find what is different

User-Agent
DirBuster - DirBuster-1.0-RC1 (http://www.owasp.org/index.php/Category:OWASP_DirBuster_Project)
Brower - Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0

Change User-Agent:  HttpRequester
Response:

GET http://10.10.10.69/sync/crack/serial
User-Agent: Iam-IronMan

 -- response --
200 OK
Date:  Tue, 29 May 2018 01:09:57 GMT
Content-Type:  text/plain
Transfer-Encoding:  chunked
Connection:  keep-alive
Server:  SuperWAF

20180529T03:09:57






