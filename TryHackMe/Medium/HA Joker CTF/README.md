# _**HA Joker CTF**_
![](joker.jpg)

## _**Enumeração**_
Primeiro, vamos começar com um scan <mark>Nmap</mark>
> ```bash
> nmap [ip_address]
> nmap -sS -T5 -p- [ip_address]
> nmap -p [ports_discovered] -A [ip_address]
> ```
![](scan_nmap.jpg)

Temos 2 serviços importantes:
* página web na porta 80
* **SSH** na porta 22

Vamos enumerar o site procurando por diretórios escondidos com <mark>Gobuster</mark>
> ```bash
> gobuster dir --url [ip_address] -w ../seclists/Discovery/Web-Content/common.txt
> ```
![](scan_gobuster.jpg)

Vamos investigar estes diretórios descobertos  
O diretório _phpinfo.php_ nos revela informações de qual versão PHP está sendo utilizada no _backend_  
Na porta 8080, precisamos de usuário e senha para acessar  
Continuando nossa enumeração, vamos alterar o prompt de busca adicionando ```-x html,txt,php```  
Conseguimos encontrar um novo arquivo  

![](scan_gobuster2.jpg)

O arquivo _secret.txt_ nos dá 2 usuários que podemos utilizar para tentar entrar na porta 8080  
Vamos tentar com _joker_  
> ```bash
> hydra -l joker -P ../wordlists/rockyou.txt [ip_address] -t 5 http-get -s 8080
> ```
![](hydra_result.jpg)

Conseguimos!  
Agora, temos acesso a um novo website  
Vamos enumerar manualmente primeiro  
No fim da página, podemos notar um nome: **joomla**  
Sabemos que, a página de login de administrador padrão é _administrator_  
Para obtermos mais informações, vamos utilizar uma ferramenta de busca de vulnerabilidades, <mark>Nikto</mark>  
> ```bash
> nikto -h http://[ip_address]:8080/ -id joker:hannah
> ```
![](scan_nikto.jpg)

Conseguimos encontrar alguns arquivos interessantes como: **robots.txt**, _/administrator_ como mencionado e um **backup.zip**  
Realizamos o download, e para extrair, precisamos de uma senha  
Por sorte e no chute, tentamos a senha para o usuário **joker** e conseguimos extrarir a página principal e a página que apresenta _administrator_ e mais alguns arquivos  

![](zip_extracted.jpg)

Lendo o arquivo **.sql** que encontramos, temos usuário e o _hash_ de senha  

![](sql_hash.jpg)

Vamos transferir essa **hash bcrypt** para um arquivo.txt e utilizar o comando abaixo para tentar quebrá-la  
> ```bash
> john [file_name].txt -w=../rockyou.txt --format=bcrypt
> ```
![](john_cracked.jpg)

Vamos tentar realizar upload de um arquivo para obtermos um _shell reverso_  
Não tem como fazer upload, mas podemos alterar o arquivo _index.php_ em _templates_ para isso  
Após a alteração, acessamos _[ip_address]:[port]/templates/beez3/index.php_  

![](reverse.jpg)

Logo verificando ```id```, podemos ver um grupo diferente  

![](container.jpg)

Precisamos criar uma _build alpine_ para obtermos acesso _root_  
Primeiro, executamos os comandos abaixo para criar um arquivo _.tar.gz_ em nossa máquina  
> ```bash
> git clone https://github.com/saghul/lxd-alpine-builder.git
> cd lxd-alpine-builder
> sudo ./build-alpine
> ```

Agora, iniciamos nosso servidor ```python``` e transferimos o arquivo  



