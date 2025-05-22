# _**Contexto de Scripts em Python**_
O uso de scripts é uma parte importante da prática de segurança  
O principal motivo para usar scripts é a personalização e a automação; você pode querer executar ações diversas vezes que não são suportadas por ferramentas padrão  

As primeiras linhas de _scripts Python_ geralmente são instruções **import** e são usadas para importar bibliotecas  
Neste caso, na linha 1, estamos importando a biblioteca **requests**  
Bibliotecas/pacotes de software podem ser considerados funções em um arquivo diferente  
Geralmente usamos bibliotecas por vários motivos:
* <mark>Eficiência</mark> - se alguém escreveu um código, então não faz sentido escrevê-lo do zero
* <mark>Modularidade</mark> - Se escrevermos código em termos de bibliotecas, reduziremos a complexidade de alterá-lo e mantê-lo, pois só precisamos fazê-lo em um só lugar

As linhas 3 e 4 são chamadas de variáveis  
Variáveis ​​são apenas espaços reservados para valores diferentes  
Python é o que chamamos de linguagem de tipagem dinâmica, o que significa que o interpretador Python tentará assumir que tipo de dado você está tentando armazenar em uma variável  
As variáveis ​​estão no formato: <mark>Nome = valor</mark>  

Na linha 6, temos um -loop while_  
Podemos querer repetir ações várias vezes com base em uma condição, e os loops nos permitem fazer isso  
A condição neste _loop while_ é verificar se a variável host não é uma string vazia  
Na linha 7, você pode ver que estamos usando a biblioteca que importamos  

Uma função geralmente recebe parâmetros ou argumentos  
Ela precisa de parâmetros ou argumentos quando precisa usar valores externos  
É por isso que funções são tão úteis  
Você pode ter o mesmo bloco de código e trocar valores diferentes  
Aqui você pode ver que o parâmetro é <mark>host + caminho</mark>  
Essa sintaxe é usada para concatenar duas strings, então host + caminho será interpretado pelo Python como: 
* <mark>https://tryhackme.com/ + robots.txt = https://tryhackme.com/robots.txt</mark>

A linha 7 basicamente pega uma URL, faz uma **requisição GET** para a página especificada e armazena a resposta em uma variável  
Assim que recebermos a resposta, podemos fazer várias coisas diferentes com ela  
A linha 9 acessa o código de status – isso é importante para verificar se um recurso existe  
Na maioria dos casos, esperamos obter resposta 200  
As linhas 11 a 14 são comentadas com o caractere #, isso significa que essas linhas não serão executadas

A linha 11 mostra que podemos acessar os dados no formato JSON, que é o mesmo de um dicionário Python  
O formato JSON é comumente usado para enviar e recuperar dados  

A linha 13 é necessária porque a biblioteca _requests_ converte os dados enviados dentro do objeto JSON para um tipo diferente de codificação  
Para facilitar a interpretação e a leitura dos dados, eles são convertidos para ASCII legível por humanos  
Você pode ver acima que a sintaxe _print()_ é usada para imprimir os dados especificados  

Como você usaria algo assim no mundo real:
+ Se você estiver tentando escrever um script para testar a funcionalidade de login de um aplicativo web onde o login é multietapas e usa dados de páginas diferentes:
  * Por exemplo, a primeira página requer um nome de usuário e senha, e a segunda página requer os dados retornados desta resposta
+ Rastreamento de páginas web para construir um mapa do site
  * Envio de uma solicitação para uma página, obtenção de outros links e execução do mesmo procedimento para todos os links do domínio

# _**Parte 2: Execução**_
Vamos criar o script para realizar a tarefa

> ```
> #!/usr/bin/python3
> 
> import requests
> 
> host = 'http://[ip_address]:[port]/'
> path = ''
> value = ''
> 
> while (path != 'end'):
>     	response = requests.get(host + path)
>     	print(response)
>     	parsed_response=response.json()
>     	value = value+parsed_response['value']
>     	path = parsed_response['next']
>     	print (path)
>     	print (value)
> print (value)
> 
> chmod +x [file_name].py
> python3 [file_name].py
> ```
