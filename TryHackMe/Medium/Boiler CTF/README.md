# _**Boiler CTF**_
![](boiler.jpg)

## _**Enumeração**_
Primeiro, vamos começar com um scan <mark>Nmap</mark>  
O primeiro, para descobrir portas, o segundo, portas adicionais, e o terceiro, versões e softwares  
> ```bash
> nmap [ip_address]
> nmap -p- -sS -T5 [ip_address]
> nmap -p [ports_discovered] -A [ip_address]
> ```
![](scan_nmap.jpg)

Temos alguns serviços descobertos
* FTP, com login anônimo disponível
* Website, na porta 80, com _robots.txt_, disponível
* MiniServ
* SSH na porta 55007

Vamos realizar login no serviço **FTP** e verificar por arquivos  
Um arquivo foi encontrado: **.info.txt**  
Lendo, temos a seguinte string: _Whfg jnagrq gb frr vs lbh svaq vg. Yby. Erzrzore: Rahzrengvba vf gur xrl!_  
Parece ser **cifra de césar**  
Tentando decifrar, temos o seguinte  

![](txt_file_found.jpg)

Visitando o website, temos apenas a página inicial do Apache  
Seguindo para a porta do _MiniServ_, temos uma página de login  

![](login_page.jpg)

Procurando por vulnerabilidades com <mark>searchsploit</mark>, temos diversos  
Mas nenhum é possível com o atual estado que temos  
Vamos voltar e visitar o diretório _/robots.txt_ que foi apontado pelo Nmap anteriormente na porta 80  
Temos resultado  

![](robots.jpg)

Diversos diretórios e uma longa string de números  
Escrito, ```not a+rabbit role or is it```  
Primeiro, a string  
Pedindo para o **Gemini** decodificar, temos o seguinte  

![](decode_string.jpg)

Parece ser MD5  
Decodificando, temos a palavra _kidding_  
Vamos para os diretórios agora  
