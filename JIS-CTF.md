# JIS-CTF: VulnUpload

Available on VulnHub:
https://www.vulnhub.com/entry/jis-ctf-vulnupload,228/

* Author: Mohammad Khreesha
* Series: JIS-CTF

## Scan target using nmap

### Service Scan
```
nmap -sV 
```
```

```


### WordPress Scan
```
nmap -p 80 --script=http-wordpress-enum 192.168.24.132
```
```
Starting Nmap 7.70 ( https://nmap.org ) at 2018-04-27 22:41 MDT
Nmap scan report for 192.168.24.132
Host is up (0.00045s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http
| http-wordpress-enum
| Search limited to top 100 themes/plugins
|   plugins
|     askimet 4.0.2
|   themes
|     twentyfifteen 1.9
|     twentysixteen 1.4
|     twentyseventeen 1.4
MAC ADDRESS: 00:0C:29:3B:45:93 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.71 seconds
```

