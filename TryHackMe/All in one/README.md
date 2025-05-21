# _**All in one CTF**_
![](all.jpg)

## _**Enumeração**_
Primeiro, vamos começar com um scan <mark>Nmap</mark>
> ```bash
> nmap -p 0-9999 -A -T5 [ip_address]
> ```
![](scan_nmap.jpg)

Parece que temos login anônimo via FTP e um _web-server_  
Vamos investigar primeiro apenas a página web  
Parece ser a página _default_ do Apache2  
Agora, vamos realizar login via FTP  
Nenhum arquivo  

Segundo, vamos realizar uma enumeração de diretórios com <mark>Gobuster</mark>
> ```
> gobuster dir --url [ip_address] -w ../seclists/Discovery/Web-Content/common.txt
> ```
![](scan_gobuster.jpg)

Temos um diretório _wordpress_  
Vamos realizar novamente um scan com <mark>Gobuster</mark>, mas com uma _wordlist_ diferente  
> ```bash
> gobuster dir --url [ip_address]/wordpress -w ../seclists/Discovery/Web-Content/CMS/wordpress.fuzz.txt
> ```
![](wp_gobuster.jpg)

Temos um LONGO retorno  
Mas por enquanto, iremos deixar de lado  
Vamos investigar a página seguindo alguns links encontrados  
Após um pouco de leitura, um dos links que nos chama a atenção é: _/wordpress/wp-login.php_  
Verificando, temos uma página de login  

Tentamos primeiro qualquer login, nada  
Segundo, tentamos login com credenciais de administrador (admin:admin), também nada  
Terceiro, o _username_ no site, **elyana**, com credenciais normais, nenhum sucesso  

Já fazia um tempo que não acontecia, mas resolvi tentar um <mark>brute force com hydra</mark>  
E sucesso!  

![](hydra_brute.jpg)  

Apesar, nenhuma das senhas funcionou  
Parece que vamos ter que achar outra maneira  
Com nenhuma outra opção em mente, voltamos a estaca zero e novamente com um scan de diretórios
> ```bash
> gobuster dir --url [ip_address] -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
> ```
![](medium.jpg)



