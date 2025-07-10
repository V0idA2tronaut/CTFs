# _**Cat pictures CTF**_
![](cat.jpg)

## _**Enumeração**_
Primeiro, vamos começar com um scan <mark>Nmap</mark>
> ```bash
> nmap -p- --open -A [ip_address]
> ```
![](scan_nmap.jpg)

O scan nos retornou informações interessantes  
Além das que estão na imagem, temos  
* 3000/tcp open  http    Golang net/http server
* 8080/tcp open  http    SimpleHTTPServer 0.6

Vamos investigando cada um, primeiro, o que está na porta 80  
Um scan com <mark>Gobuster</mark> nos retorna a imagem abaixo
> ```bash
> gobuster dir --url http://[ip_address]:80/ -w ../seclists/Discovery/Web-Content/big.txt
> ```
![](scan_gobuster.jpg)

E temos alguns diretórios, mas ao tentar acessar, temos código 403  
Vasculhando a página inicial, encontramos um formulário de login  

![](login_form.jpg)

Algumas tentativas de SQL injection foram feitas, mas sem sucesso  
Com versão disponível, nenhum exploit foi encontrado  
Vamos para a porta 1337  

![](github_port.jpg)

Na porta 3000, temos um website com formulário de login  

![](port_3000.jpg)

