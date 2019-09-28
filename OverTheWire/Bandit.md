Bandit

overthewire.org/wargames/bandit4

* Staff: Steven "Steven" Van Acker, morla
* Series: Wargames

Something I dediced to giver on over my lunch break.

## Level 0

### SSH > bandit.labs.overthewire.org
```
ssh bandit0@bandit.labs.overthewire.org -p 2220

bandit0@bandit:~$ cat readme 
boJ9jbbUNNfktd78OOpsqOltutMc3MY1

```

## Level 1

### Special Characters
```
ssh bandit1@bandit.labs.overthewire.org -p 2220

bandit1@bandit:~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9

``` 

## Level 2

### Spaces in Filenames
```
ssh bandit2@bandit.labs.overthewire.org -p 2220

bandit2@bandit:~$ cat "spaces in this filename" 
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK

```

## Level 3

### Hidden files
```
bandit3@bandit:~$ cat inhere/.hidden 
pIwrPrtPN36QITSp3EQaw936yaFoFgAB

```

## Level 4

### Human Readable
```
bandit4@bandit:~$ file ./inhere/-* |grep ASCII
./inhere/-file07: ASCII text
bandit4@bandit:~$ cat ./inhere/-file07
koReBOKuIDDepwhWk7jZC0RTdopnAYKh

```

## Level 5

### Find the File
```
bandit5@bandit:~$ find ./inhere/maybehere* -type f -size 1033c
./inhere/maybehere07/.file2
bandit5@bandit:~$ cat ./inhere/maybehere07/.file2
DXjZPULLxYr17uwoI01bNLQbtFemEgo7

```

## Level 6

### Find Password File
```
bandit6@bandit:~$ find / -type f -size 33c 2>&1  |grep bandit7 |grep -v "Permission"
/var/lib/dpkg/info/bandit7.password
/etc/bandit_pass/bandit7
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password 
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs 

```

## Level 7

### Find Password in File
```
bandit7@bandit:~$ cat data.txt |grep millionth
millionth	cvX2JJa4CFALtqS87jk27qwqGhBM9plV

```

## Level 8

### Find Password within File of Passwords
```
bandit8@bandit:~$ sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR

```

## Level 9

### Find Password in Haystack
```
bandit9@bandit:~$ strings data.txt | grep ====
2========== the
========== password
========== isa
========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk

```

## Level 10

### Base64 Encoding
```
bandit10@bandit:~$ cat data.txt | base64 -d data.txt 
The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR

```

## Level 11

### Rot13
```
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu

```

## Level 12

### Hex Dump
```
bandit12@bandit:/tmp/static$ head -4 data.txt
00000000: 1f8b 0808 d7d2 c55b 0203 6461 7461 322e  .......[..data2.
00000010: 6269 6e00 013c 02c3 fd42 5a68 3931 4159  bin..<...BZh91AY
00000020: 2653 591d aae5 9800 001b ffff de7f 7fff  &SY.............
00000030: bfb7 dfcf 9fff febf f5ad efbf bbdf 7fdb  ................

bandit12@bandit:/tmp/static$ xxd -r data.txt data1
bandit12@bandit:/tmp/static$ hexdump -C data1 | head 
00000000  1f 8b 08 08 d7 d2 c5 5b  02 03 64 61 74 61 32 2e  |.......[..data2.|
00000010  62 69 6e 00 01 3c 02 c3  fd 42 5a 68 39 31 41 59  |bin..<...BZh91AY|
00000020  26 53 59 1d aa e5 98 00  00 1b ff ff de 7f 7f ff  |&SY.............|
00000030  bf b7 df cf 9f ff fe bf  f5 ad ef bf bb df 7f db  |................|
00000040  f2 fd ff df ef fa 7f ff  fb d7 bd ff b0 01 39 8c  |..............9.|
00000050  10 06 80 00 00 00 0d 06  99 00 00 00 68 34 00 0d  |............h4..|
00000060  01 a1 a0 00 00 7a 80 00  0d 00 00 06 9a 00 d0 34  |.....z.........4|
00000070  0d 1a 32 34 68 d1 e5 36  a6 d4 40 00 34 1a 62 00  |..24h..6..@.4.b.|
00000080  00 69 a0 00 00 00 00 00  d0 03 d2 00 68 1a 0d 00  |.i..........h...|
00000090  00 01 b5 1a 1a 0c 20 1e  a0 00 6d 46 80 68 06 9a  |...... ...mF.h..|

bandit12@bandit:/tmp/static$ cp data1 data1.gz
bandit12@bandit:/tmp/static$ gzip -d data1.gz
bandit12@bandit:/tmp/static$ hexdump -C data1 | head 
00000000  42 5a 68 39 31 41 59 26  53 59 1d aa e5 98 00 00  |BZh91AY&SY......|
00000010  1b ff ff de 7f 7f ff bf  b7 df cf 9f ff fe bf f5  |................|
00000020  ad ef bf bb df 7f db f2  fd ff df ef fa 7f ff fb  |................|
00000030  d7 bd ff b0 01 39 8c 10  06 80 00 00 00 0d 06 99  |.....9..........|
00000040  00 00 00 68 34 00 0d 01  a1 a0 00 00 7a 80 00 0d  |...h4.......z...|
00000050  00 00 06 9a 00 d0 34 0d  1a 32 34 68 d1 e5 36 a6  |......4..24h..6.|
00000060  d4 40 00 34 1a 62 00 00  69 a0 00 00 00 00 00 d0  |.@.4.b..i.......|
00000070  03 d2 00 68 1a 0d 00 00  01 b5 1a 1a 0c 20 1e a0  |...h......... ..|
00000080  00 6d 46 80 68 06 9a 68  34 34 0c a7 a8 34 06 40  |.mF.h..h44...4.@|
00000090  00 06 80 00 01 ea 06 81  90 03 f5 40 32 1a 00 03  |...........@2...|

bandit12@bandit:/tmp/static$ cp data1 data2.bz
bandit12@bandit:/tmp/static$ bzip2 -d data2.bz
bandit12@bandit:/tmp/static$ hexdump -C data2 | head
00000000  1f 8b 08 08 d7 d2 c5 5b  02 03 64 61 74 61 34 2e  |.......[..data4.|
00000010  62 69 6e 00 ed d1 3d 48  1b 71 1c c6 f1 ff 9d 54  |bin...=H.q.....T|
00000020  2c 18 88 90 29 43 bc 2c  19 44 e5 de 8c 88 4b 22  |,...)C.,.D....K"|
00000030  2a 96 22 85 8a c8 05 1c  92 2a d5 a5 82 5e a0 05  |*."......*...^..|
00000040  d1 6b 0d d1 0c 62 91 0c  45 29 14 7c a5 38 b4 5b  |.k...b..E).|.8.[|
00000050  45 87 1b 8a e8 52 ac 2f  34 2d a5 55 04 27 45 90  |E....R./4-.U.'E.|
00000060  74 13 6c ec a8 60 a7 da  16 be 9f e5 f9 c1 ef d9  |t.l..`..........|
00000070  9e ae b8 1d af a9 4e f4  3e 12 7f 8e 5a 10 36 cd  |......N.>...Z.6.|
00000080  5f 59 70 29 75 f3 e2 d6  0c 23 ac 19 35 9a a1 d7  |_Yp)u....#..5...|
00000090  0a 55 d3 f4 42 5d 51 c5  0d 48 0e d8 f1 7e 45 11  |.U..B]Q..H...~E.|

bandit12@bandit:/tmp/static$ cp data2 data3.gz
bandit12@bandit:/tmp/static$ gzip -d data3.gz
bandit12@bandit:/tmp/static$ hexdump -C data3 | head
00000000  64 61 74 61 35 2e 62 69  6e 00 00 00 00 00 00 00  |data5.bin.......|
00000010  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00000060  00 00 00 00 30 30 30 30  36 34 34 00 30 30 30 30  |....0000644.0000|
00000070  30 30 30 00 30 30 30 30  30 30 30 00 30 30 30 30  |000.0000000.0000|
00000080  30 30 32 34 30 30 30 00  31 33 33 36 31 33 35 31  |0024000.13361351|
00000090  33 32 37 00 30 31 31 32  34 34 00 20 30 00 00 00  |327.011244. 0...|
000000a0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00000100  00 75 73 74 61 72 20 20  00 72 6f 6f 74 00 00 00  |.ustar  .root...|

bandit12@bandit:/tmp/static$ cp data3 data4.tar
bandit12@bandit:/tmp/static$ tar -xf data4.tar
bandit12@bandit:/tmp/static$ ls
data1  data2  data3  data4.tar  data5.bin  data.txt
bandit12@bandit:/tmp/static$ hexdump -C data5.bin | head
00000000  64 61 74 61 36 2e 62 69  6e 00 00 00 00 00 00 00  |data6.bin.......|
00000010  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00000060  00 00 00 00 30 30 30 30  36 34 34 00 30 30 30 30  |....0000644.0000|
00000070  30 30 30 00 30 30 30 30  30 30 30 00 30 30 30 30  |000.0000000.0000|
00000080  30 30 30 30 33 34 31 00  31 33 33 36 31 33 35 31  |0000341.13361351|
00000090  33 32 37 00 30 31 31 32  34 37 00 20 30 00 00 00  |327.011247. 0...|
000000a0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00000100  00 75 73 74 61 72 20 20  00 72 6f 6f 74 00 00 00  |.ustar  .root...|

bandit12@bandit:/tmp/static$ tar -xf data5.bin
bandit12@bandit:/tmp/static$ ls
data1  data2  data3  data4.tar  data5.bin  data6.bin  data.txt
bandit12@bandit:/tmp/static$ hexdump -C data6.bin | head
00000000  42 5a 68 39 31 41 59 26  53 59 36 10 01 45 00 00  |BZh91AY&SY6..E..|
00000010  89 7f ff dc 59 80 40 c0  55 ff e0 00 5a a3 88 74  |....Y.@.U...Z..t|
00000020  21 de 80 00 48 02 08 80  10 c0 00 02 75 92 80 20  |!...H.......u.. |
00000030  00 44 08 20 00 94 0d 48  d4 f4 8d 30 80 06 81 a6  |.D. ...H...0....|
00000040  86 86 46 86 4d 1a 0d 34  c6 84 1a 45 1e a6 9a 0d  |..F.M..4...E....|
00000050  0c 20 32 00 c9 89 a0 c4  62 68 00 69 b9 f5 8f 41  |. 2.....bh.i...A|
00000060  02 01 84 00 40 82 2a 1e  ed f4 c6 21 59 01 63 11  |....@.*....!Y.c.|
00000070  10 1f 75 d3 09 9b 8d bb  49 1a 83 89 ec c0 57 2a  |..u.....I.....W*|
00000080  c8 df e6 61 d9 bb 7e d5  d7 bc 01 39 da 03 e1 59  |...a..~....9...Y|
00000090  d3 c0 ce 14 a8 2c f2 a5  21 59 4b 89 5a 41 34 52  |.....,..!YK.ZA4R|

bandit12@bandit:/tmp/static$ bzip2 -d data6.bin
bzip2: Can't guess original name for data6.bin -- using data6.bin.out
bandit12@bandit:/tmp/static$ hexdump -C data6.bin.out | head
00000000  64 61 74 61 38 2e 62 69  6e 00 00 00 00 00 00 00  |data8.bin.......|
00000010  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00000060  00 00 00 00 30 30 30 30  36 34 34 00 30 30 30 30  |....0000644.0000|
00000070  30 30 30 00 30 30 30 30  30 30 30 00 30 30 30 30  |000.0000000.0000|
00000080  30 30 30 30 31 31 37 00  31 33 33 36 31 33 35 31  |0000117.13361351|
00000090  33 32 37 00 30 31 31 32  35 32 00 20 30 00 00 00  |327.011252. 0...|
000000a0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00000100  00 75 73 74 61 72 20 20  00 72 6f 6f 74 00 00 00  |.ustar  .root...|

bandit12@bandit:/tmp/static$ tar -xf data6.bin.out
bandit12@bandit:/tmp/static$ ls
data1  data2  data3  data4.tar  data5.bin  data6.bin.out  data8.bin  data.txt
bandit12@bandit:/tmp/static$ hexdump -C data8.bin
00000000  1f 8b 08 08 d7 d2 c5 5b  02 03 64 61 74 61 39 2e  |.......[..data9.|
00000010  62 69 6e 00 0b c9 48 55  28 48 2c 2e 2e cf 2f 4a  |bin...HU(H,.../J|
00000020  51 c8 2c 56 b0 88 ca aa  74 0e ca 74 0a 77 8b cc  |Q.,V....t..t.w..|
00000030  ce 4b 4d cc f0 28 af 70  2e 33 2e 4f 32 4a 34 f4  |.KM..(.p.3.O2J4.|
00000040  0f 2a 88 f4 e1 02 00 0f  9b 95 9f 31 00 00 00     |.*.........1...|
0000004f

bandit12@bandit:/tmp/static$ cp data8.bin data8.gz
bandit12@bandit:/tmp/static$ gzip -d data8.gz
bandit12@bandit:/tmp/static$ ls
data1  data2  data3  data4.tar  data5.bin  data6.bin.out  data8  data8.bin  data.txt
bandit12@bandit:/tmp/static$ hexdump -C data8
00000000  54 68 65 20 70 61 73 73  77 6f 72 64 20 69 73 20  |The password is |
00000010  38 5a 6a 79 43 52 69 42  57 46 59 6b 6e 65 61 68  |8ZjyCRiBWFYkneah|
00000020  48 77 78 43 76 33 77 62  32 61 31 4f 52 70 59 4c  |HwxCv3wb2a1ORpYL|
00000030  0a                                                |.|
00000031

bandit12@bandit:/tmp/static$ strings data8
The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL

```

## Level 13

### Keys
```
bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost 
Could not create directory '/home/bandit13/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit13/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

Linux bandit 4.18.12 x86_64 GNU/Linux

--------
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e

```

## Level 14

### netcat this
```
bandit14@bandit:~$ nc localhost 30000
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
Correct!
BfMYroe26WYalil77FoDi9qh59eK5xNr

```

## Level 15

### netcat this secure
```
bandit15@bandit:~$ ncat -C --ssl localhost 30001
BfMYroe26WYalil77FoDi9qh59eK5xNr
Correct!
cluFn7wTiGryunymYOu4RcffSxQluehd

```

## Level 16

## netcat multi port
```
bandit16@bandit:~$ nmap localhost -p 31000-32000

Starting Nmap 7.40 ( https://nmap.org ) at 2019-09-27 22:58 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00026s latency).
Not shown: 999 closed ports
PORT      STATE    SERVICE
31518/tcp filtered unknown
31790/tcp open     unknown

Nmap done: 1 IP address (1 host up) scanned in 1.28 seconds

andit16@bandit:~$ ncat -C --ssl localhost 31790
cluFn7wTiGryunymYOu4RcffSxQluehd
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

```

## Level 17

### Running the diffs
```
bandit17@bandit:~$ diff passwords.new passwords.old
42c42
< kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
---
> hlbSBPAWJmL6WFDb06gpTx1pPButblOA

```

## Level 18

### bash.rc booo urrns
```
Byebye !
Connection to localhost closed.
bandit17@bandit:~$ ssh -t bandit18@localhost /bin/sh
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit17/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0640 for '/home/bandit17/.ssh/id_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "/home/bandit17/.ssh/id_rsa": bad permissions
bandit18@localhost's password: 
$ ls
readme
$ cat readme    
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x

```

## Level 19

### Setuid
```
bandit19@bandit:~$ ls
bandit20-do
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
GbKksEFF4yrVs6il55v6gwY5aVje5f0j

```

## Level 20

### Tmux
```
bandit20@bandit:~$ tmux split-window
bandit20@bandit:~$ nc -nvlp 8869
listening on [any] 8869 ...
connect to [127.0.0.1] from (UNKNOWN) [127.0.0.1] 58732
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr
bandit20@bandit:~$

bandit20@bandit:~$ ./suconnect 8869
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password

```

## Level 21

### Cron.d
```
bandit21@bandit:~$ cat /etc/cron.d/cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit21@bandit:~$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:~$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI

```

## Level 22

### Cron.d * 2
```
bandit22@bandit:~$ cat /etc/cron.d/cronjob_bandit23 .
./            ../           .bash_logout  .bashrc       .profile      
bandit22@bandit:~$ cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:~$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
bandit22@bandit:~$ whoami
bandit22
bandit22@bandit:~$ echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:~$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
bandit22@bandit:~$ 

```

## Level 23

### script writter
```
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ

```


## Level 24

### Brute Force
```
Wrong PINCODE: 6725
Wrong PINCODE: 6726
Wrong PINCODE: 6727
Wrong PINCODE: 6728
Wrong PINCODE: 6729
Correct!
The password of user bandit25 is uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG

```

## Level 25

### Tricky tty
```
showme the text

5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z

```

## Level 26

