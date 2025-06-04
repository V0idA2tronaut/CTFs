# _**PenTest**_
<mark>Primeiro, um scan nmap na rede em todas as portas</mark>
> ```bash
> nmap -p- -sV -A -T5 [ip_address]
> ```
> ```bash
> nmap -p- -sV -A -T5 -sU [ip_address]
> ```

**porta 80: aberta | http | Microsoft IIS httpd 10.0**  
**porta 3389: aberta | ms-wbt-server | Microsoft Terminal Services**  
**Windows OS**  

<mark>Segundo, um teste de força bruta em diretórios com o gobuster</mark>
> ```bash
> gobuster dir --url [ip_address] -w ../seclists/Discovery/Web-Content/common.txt
> ```
> ```bash
> gobuster dir --url [ip_address] -w ../seclists/Discovery/Web-Content/Web-Servers/IIS-POST.txt
> ```
> ```bash
> gobuster dir --url [ip_address] -w ../seclists/Discovery/Web-Content/Web-Servers/IISsystemweb.txt
> ```
> ```bash
> gobuster dir --url [ip_address] -w ../seclists/Discovery/Web-Content/Web-Servers/IIS.txt
> ```

Apenas um deles deu retorno (o último) e foi a página inicial, nenhum outro diretório encontrado  
Nova tentativa efetuada após estudar o conteúdo das _seclists_  
> ```bash
> gobuster dir --url [ip_address] -w ../seclists/Discovery/Web-Content/big.txt
> ```

Um retorno de código 301 e diretório: <mark>/retro</mark>  
Tentativa de exploração de diretórios com força bruta e a ferramenta gobuster  
> ```bash
> gobuster dir --url [ip_address]/retro -w ../seclists/Discovery/Web-Content/common.txt
> ```
> ```bash
> gobuster dir --url [ip_address]/retro -w ../seclists/Discovery/Web-Content/big.txt
> ```
> ```bash
> gobuster dir --url [ip_address]/retro -w ../seclists/Discovery/Web-Content/Logins.fuzz.txt
> ```
> ```bash
> gobuster dir --url [ip_address]/retro -w ../seclists/Discovery/Web-Content/Passwords.fuzz.txt
> ```

<mark>Diretórios encontrados por common.txt</mark>
* /index.php (cód.301)
* /wp-admin (cód.301)
* /wp-content (cód.301)
* /wp-includes (cód.301)
* /xmlrpc.php (cód.405)

<mark>Diretórios retornados por Logins.fuzz.txt</mark>
* /wp-login.php (cód.200)
* /wp-signup.php (cód.200)

<mark>Os retornos de big.txt e Passwords.fuzz.txt foram idênticos ou não retornaram nada</mark>  
As tentativas de verificar os conteúdos dos diretórios levaram a esta descoberta especificamente no diretório /xmlrpc.php: _XML-RPC server accepts POST requests only_  
Após muita busca por ajuda na Internet e verificação de arquivos, constatou-se que a senha para o usuário Wade é **parzival**  
Como temos um serviço na porta 3389, isso significa que podemos tentar uma conexão remota com o usuário _Wade_ e a senha _parzival_  
Instalando e utilizando o serviço remmina, é possível obter um login com sucesso e acesso ao arquivo _users.txt_  

Na área de trabalho, existe outro arquivo, um executável: _hhupd_  
Pesquisando no google, encontra-se a **CVE-2019-1388**: _An elevation of privilege vulnerability exists in the Windows Certificate Dialog when it does not properly enforce user privileges, aka 'Windows Certificate Dialog Elevation of Privilege Vulnerability_  

Com estas informações, tenta-se executar _hhupd_  
A janela abre para senha de administrador e um link de certificado  
Apesar de não funcionar mais, podemos salvar a página do Internet Explorer como C:\Windows\system32\*.*  
Haverá uma negação, porém, teremos acesso a pasta e podemos executar o CMD como administrador  
Para confirmar, basta um ```whoami```

Navegando pelos diretórios, encontra-se o arquivo _root.txt_ em \Users\Administrator\Desktop  
Um simples comando more root.txt e vemos seu conteúdo  
