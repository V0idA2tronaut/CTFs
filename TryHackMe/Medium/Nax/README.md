# _**Nax CTF**_
![](nax.jpg)

## _**Enumeração**_
Primeiro, vamos começar com um scan de rede com <mark>Nmap</mark>
> ```bash
> nmap -p- -T3 [ip_address]
> nmap -p [ports_discovered] -sV -O [ip_address]
> ```
![](scan_nmap.jpg)

Parece que temos alguns serviços:
* http
* https
* ldap
* smtp

Vamos, primeiro investigar as versões dos serviços **LDAP** e **SMTP**  
Com a ferramenta <mark>searchsploit</mark>, encontramos um DoS para LDAP e um _Remote Code Injection_ para SMTP  
Não temos como determinar a versão do SMTP, então ignoramos  

Para o segundo passo, vamos investigar os websites  
Tanto para a porta 80, quanto a porta 443, temos a imagem abaixo

![](cicada.jpg)

Parece que temos uma linguagem ou código e os símbolos dos elementos abaixo  
Talves sejam importantes:
* prata | nº atômico = 47
* mercúrio | nº atômico = 80
* tântalo | nº atômico = 73
* antimônio | nº atômico = 51
* polônio | nº atômico = 84
* paládio | nº atômico = 46
* platina | nº atômico = 78
* laurêncio | nº atômico = 103  

Voltando, vamos tentar uma enumeração aprofundada do serviço **SMTP**  
Utilizando **Telnet**, conseguimos conexão no serviço com: ```Telnet [ip_address] 25```  
Pesquisando sobre, encontramos que, com <mark>smtp-user-enum</mark>, podemos tentar identificar usuários
> ```bash
> smtp-user-enum -M VRFY -U [userlist_name].txt -t [target_ip_address].com
> ```
![](smtp_user_enumm.jpg)

Encontramos 2:
* root
* mysql

Tentando com a ferramenta <mark>Metasploit</mark>, temos o resultado abaixo

![](meta_enum.jpg)

Podem ser úteis para depois  
Vamos tentar procurar por diretórios no website via <mark>Gobuster</mark>
> ```bash
> gobuster dir --url [ip_address]:[port] -w ../seclists/../Web-Services/common.txt
> ```
![](scan_gobuster.jpg)

Verificando por mais pistas no website, temos um comentáiro: **/nagiosxi**  
Podemos tentar acessar também  

![](index_php.jpg)

Acessando **/nagiosxi**, temos uma página de loing

![](nagios.jpg)



