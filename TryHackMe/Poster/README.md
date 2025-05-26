![image](https://github.com/user-attachments/assets/3058fd06-1a87-4bd6-bb08-5be87644c8c0)# _**Poster CTF**_
![](post.jpg)

## _**Enumeração**_
Primeiro, vamos começar com um scan <mark>Nmap</mark>
> ```bash
> nmap -p 0-9999 -A -T5 [ip_address]
> ```
![](scan_nmap.jpg)

Parece que temos um banco de dados, <mark>PostgresSQL DB</mark> na porta 5432  
Vamos investigar a página web primeiro  
Um scan com <mark>Gobuster</mark>
> ```bash
> gobuster dir --url [ip_address] -w ../seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
> ```
![](scan_gobuster.jpg)

Após, vamos utilizar o <mark>Metasploit</mark> para encontrar uma vulnerabilidade na aplicação  

![](metasploit.jpg)

As etapas a seguir foram feitas para poder encontrar um módulo de tentativa de enumeração de usuário e senha  

![](msf_module.jpg)  

![](msf_exploit.jpg)  

Conseguimos!  
Vamos agora escolher outro módulo para conseguirmos executar comandos com as credenciais encontradas  

![](msf_command_exe.jpg)

Conseguimos a versão do **banco de dados**

![](msf_exe.jpg)

Vamos procurar um módulo para extrair hashes de usuários  

![](msf_hash.jpg)

Configurando o módulo para as credenciais essenciais, executamos  

![](msf_hash_exe.jpg)

Com mais uma busca, conseguimos encontrar mais dois móudulos dos quais estão sendo requisitados  

![](msf_more_auxiliar.jpg)

A partir deste último módulo, vamos configurá-lo e executar para podermos obter acesso a máquina-alvo  

![](exploit_sql.jpg)

## _**Ganhando acesso_**
Parece que conseguimos encontrar um documento com credenciais de login SSH

![](ssh_login.jpg)

Vamos realizar

## _**Escalando privilégios**_
Para conseguirmos escalar privilégios, executamos alguns comandos como ```sudo -l``` e ```find```, mas sem muito sucesso  
Vamos tentar com [LinEnum.sh](https://github.com/rebootuser/LinEnum)  
Após a execução, investigando, temos o seguinte arquivo com credenciais!  

![](host_passwd.jpg)

Conseguimos login!  
Agora, vamos escalar privilégios para _root_  
Primeiro, ```sudo -l```

![](sudo.jpg)

Parece que apenas um ```sudo su``` e temos _root_  
Basta agora buscar as flags!
