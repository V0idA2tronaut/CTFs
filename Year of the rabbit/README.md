# __*Year of the rabbit**_
![](rabbit.jpg)

## _**Enumeração**_
Primeiro, vamos começar com um scan <mark>nmap</mark>
> ```bash
> nmap --open -A -T5 [ip_address]
> ```
![](scan_nmap.jpg)

Também, vamos realizar um scan de diretórios com <mark>Gobuster</mark>
> ```bash
> gobuster dir --url [ip_address] -w ../seclists/Discovery/Web-Content/common.txt
> ```
![](scan_gobuster.jpg)

Vamos verificar os diretórios:  
<mark>/assets</mark>  

![](rick_rolled.jpg)

<mark>RickRolled.mp4</mark>  
É literalmente o vídeo da música, porém, existem trechos de áudio alterados:
> “está procurando no lugar errado”

<mark>/Style.css</mark>  

![](css.jpg)

Investigando o link  

![](s3cr3t.jpg)

Investigando o link novamente com o JS desativado  

![](you_rolled.jpg)

Acredito que não será a última vez que verei isto  
Vamos continuar  
Tentei novamente desabilitando o JavaScript e consegui chegar nesta página  

![](no_JS.jpg)

Realizei o download do vídeo com ```wget```  
Vamos verificar para ofuscação de dados no vídeo com <mark>binwalk</mark>
> ```bash
> binwalk -e RickRolled.mp4
> ```  
![](binwalk_result.jpg)

Extração feita com sucesso!  
Temos o seguinte:  

![](binwalk_zip.jpg)

Investigando um pouco o que foi encontrado...  
Isto não leva a nada  
Vamos tentar com outra ferramenta, <mark>exiftoot</mark>  

![](exiftool.jpg)

Parece não ter nada também  
Vamos analisar as requisições feitas pelo site com Burp Suite ao acessar os diretórios descobertos  
Afinal, DEVE ter algo escondido por ali  

![](burp.jpg)

Temos um diretório escondido  
Vamos verificar ele  

![](hidden.jpg)

Vamos realizar o download da imagem com ```wget```  
Como steghide não aceita entradas do tipo _.png_, vamos tentar extrair seu conteúdo com <mark>binwalk</mark>  
> ```bash
> binwalk -e Hot_Babe.png
> ```  
![](binwalk_hot.jpg)

Não levou a nada  
Vamos tentar com <mark>exiftool</mark>  

![](exiftool2.jpg)

Nada também  
Vamos com a próxima, a ferramenta <mark>strings</mark>  

![](strings.jpg)

## _**Ganhando acesso**_
Temos um usuário: <mark>ftpuser</mark>  
E uma grande possibilidade de senhas  
Mas não é problema  
Vamos copiar elas para um arquivo _.txt_ e usar o <mark>hydra</mark> para realizar um ataque de força bruta com ele
> ```bash
> hydra -l ftpuser -p [passwd.txt] ftp://[ip_address]
> ```  
![](hydra_brute.jpg)

Conseguimos!  
Vamos realizar login  
Temos um arquivo  
Extraímos ele para nosso computador e verificamos com ```cat```  

![](login&extraction.jpg)  

![](brainfck.jpg)  

Essa simbologia é conhecida, linguagem de programação _**brainfuck**_  
Vamos usar um decodificador online  

![](decoder.jpg)

Temos usuário e senha!  
Vamos tentar login via _ssh_  

![](ssh_login.jpg)

Vamos usar o comando find para tentar encontrar a primeira flag em _user.txt_
> ```bash
> find / -name user.txt 2>/dev/null
> ```  
![](user_flag.jpg)

Vamos tentar buscar algo através da mensagem de banner deixada por _root_
> ```bash
> find / -name “secret” 2>/dev/null
> ```
![](root.jpg)

Vamos tentar de outra maneira
> ```bash
> find / -name s3cr3t 2>/dev/null
> ```
![](root_s3cr3t.jpg)

E assim, conseguimos a primeira _flag_  

![](first_flag.jpg)

## _**Escalando privilégios**_
Vamos começar com um comando ```find```
> ```bash
> find / -writable -type f -name "*.sh" 2>/dev/null
> ```
![](find_command.jpg)

Apesar de existir a possibilidade de execução de comandos no arquivo, não foi possível escalar privilégios  
Vamos ter que buscar outros meios  
Pesquisando na Internet, encontramos a [CVE-2021-4034](https://nvd.nist.gov/vuln/detail/cve-2021-4034)  
Inicia-se o servidor python após baixar o exploit, transfere-se para a máquina-alvo, concede permissões e executa o exploit  

![](priv_esc.jpg)

Conseguimos a última flag!
