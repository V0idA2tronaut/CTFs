# _**Brooklyn Nine Nine CTF**_
![](b99.jpg)

## _**Enumeração**_
Primeiro, vamos começar com um scan <mark>Nmap</mark>
> ```bash
> nmap -p 0-9999 -A -T5 [ip_address]
> ```
![](scan_nmap.jpg)

Também vamos realizar um scan com <mark>Gobuster</mark>
> ```bash
> gobuster dir --url [ip_address] -w ../Discovery/Web-Content/common.txt
> ```
![](scan_gobuster.jpg)

Investigando <mark>index.html</mark>  

![](index.jpg)

Olhando o código da página, temos a seguinte mensagem:  

![](go_steg.jpg)

Vamos realizar o download da imagem com ```wget```
> ```bash
> wget [ip_address]/brooklyn99.jpg
> ```

## _**Ganhando acesso**_
Agora, vamos utilizar a ferramenta <mark>Steghide</mark> para tentar extrair seu conteúdo  

![](steg_fail.jpg)

Precisamos de uma senha  
Acredito que ela deva estar no serviço FTP 
Vamos realizar login anonimamente  

![](ftp.jpg)

Sabemos um detalhe agora, nome de usuário: **jake**  
Vamos continuar realizando um ataque de força bruta com <mark>Hydra</mark> no serviço SSH  

![](hydra.jpg)

Vamos realizar login e obter a primeira flag  

![](login.jpg)

## _**Escalando privilégios**_
Primeiro, vamos olhar a dica  
Diz para utilizarmos o comando ```sudo```  
Vamos usar ```sudo -l``` e ver o que podemos fazer  

![](sudo.jpg)

Podemos usar o comando ```less``` sem precisarmos de senha através de /usr/bin/less  
O comando ```less``` é um leitor de paginas de texto para sistemas Unix-like, como Linux e macOS  
Ele é usado para visualizar o conteúdo de arquivos de texto  
Vamos usar isso para buscar a próxima flag
> ```bash
> sudo /usr/bin/less /root/root.txt
> ```
![](less.jpg)

E assim, concluído!
