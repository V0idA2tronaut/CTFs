# _**Contexto de HTTP**_

Um site geralmente é executado no que é chamado de servidor web
Um servidor web é o que é necessário para essencialmente tornar o site acessível na internet mais ampla (entre outras coisas)
Um computador precisa de uma maneira padronizada de se comunicar com um servidor web
Existem tantos tipos diferentes de sistemas operacionais, navegadores e dispositivos e precisamos garantir que essa comunicação seja consistente em todos eles

_Hyper Text Transfer Protocol (HTTP) geralmente funciona na forma de solicitações em que um cliente (navegador) envia uma solicitação para concluir uma ação específica para o site (servidor)_
_Essas ações podem variar de fazer login e recuperar páginas até adicionar alguns dados_

> ```bash
> GET /login HTTP/1.1 [1]
> Host: localhost:3000 [2]
> User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:61.0) Gecko/20100101 Firefox/61.0 [3]
> Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8 [4]
> Accept-Language: en-GB,en;q=0.5 [5]
> Accept-Encoding: gzip, deflate [6]
> Connection: close [7]
> Upgrade-Insecure-Requests: 1 [8]
> ```

Temos:
+ [1] A primeira parte é conhecida como verbo HTTP que diz ao servidor que tipo de ação o cliente está solicitando. Os tipos mais comuns são:
	* GET - usado para recuperar recursos, para retirar um livro da estante ou abrir em uma página específica desse livro
	* POST - usado para alterar o estado no servidor, para escrever algo
A segunda parte se refere ao caminho para onde a ação é direcionada
A terceira parte se refere ao protocolo
+ [2] Usado para passar o nome de domínio do servidor
Isso é útil em uma situação em que um servidor hospeda vários sites, para que o servidor saiba qual página passar de volta para o navegador
+ [3] Especifica qual navegador fez a solicitação
Isso é útil porque diferentes páginas da web são renderizadas de forma diferente em navegadores da web, então os servidores saberiam exatamente o que servir dependendo de qual navegador solicitou

Quando o servidor receber essa solicitação, ele responderá à ação com uma resposta HTTP
No caso da solicitação a seguir, esta é a resposta:

> ```bash
> HTTP/1.1 200 OK[1]
> X-Powered-By: Express[2]
> Content-Type: text/html; charset=utf-8[3]
> Content-Length: 1493[3]
> ETag: W/"5d5-ZdJhoKmkW86HklS/Wy+dOEaa80A"[4]
> Date: Fri, 29 Nov 2019 00:20:52 GMT[5]
> Connection: close[6]
> ```

+ [1] Contém a versão do protocolo HTTP que o servidor usa
+ [2] Conhecida como código de status/resposta
Códigos de resposta são usados ​​pelo servidor para indicar o status de uma solicitação
Eles são divididos nas seguintes classes:
* 1XX - Information Requests
* 2XX - Successful Requests
* 3XX - Redirects
* 4XX - Client Errors
* 5XX - Server Errors

Aqui estão algumas outras coisas úteis a serem observadas sobre o protocolo HTTP:
* HTTP é stateless: o servidor não tem como controlar a ordem das solicitações que o cliente está enviando
* O HTTP não é criptografado e geralmente é usado com TLS para formar HTTPS
* O HTTP normalmente é executado na porta 80, enquanto o HTTPS normalmente é executado na porta 443

# _**Contexto de Cookies**_
Se o HTTP não consegue manter o estado, como o servidor rastreia o que o cliente está fazendo, especialmente se ele estiver conectado e comprando produtos
Cookies são um par de valores-chave na forma de nome:valor e podem ser usados ​​para vários propósitos
Gerenciamento de sessão se refere a como o servidor mantém o controle das ações realizadas por um cliente
As sessões geralmente são criadas com o seguinte fluxo de trabalho:
* O usuário envia nome de usuário e senha ao servidor para se autenticar
* O servidor verifica se os detalhes dos usuários estão corretos e define um cookie
* Cada vez que o usuário realiza uma ação, o navegador envia o cookie como parte da solicitação ao servidor, que então verifica o cookie para garantir que o usuário esteja autorizado a realizar uma ação específica

Uma observação importante a considerar é que os valores de cookies serão codificados de tempos em tempos
Um navegador e um servidor podem não ser capazes de interpretar caracteres específicos, então o valor colocado em um cookie é codificado quando ele é enviado e decodificado sem o usuário ver

Um invasor tentaria primeiro identificar como o cookie é usado para gerenciamento de sessão
Usando o Burp Suite, eles interceptariam cada solicitação mencionada no fluxo acima para examinar como o servidor define o cookie
As características comuns do gerenciamento de sessão inseguro incluem:
* Usando um valor de cookie fixo
Se um invasor obtiver o valor do cookie de um usuário e esse valor do cookie for sempre o mesmo, o invasor poderá usá-lo para obter acesso à conta de um usuário
* Usando um valor previsível como parte de um cookie
Se um servidor cria cookies que usam valores previsíveis como nome de usuário ou números, um invasor pode simplesmente definir seu próprio cookie que um servidor autenticaria como um usuário diferente

Embora você possa usar o Burp Suite para manipular cookies, você também pode usar os recursos de inspeção de elementos dos navegadores

# _**Execução**_
Cria-se três novos usuários:
* Teste1
* Teste2
* Teste3

Ao efetuar login com cada um deles, nota-se que eles possuem valores de _cookies_ idênticos: <mark>dGVzdGUxdjRlcjlsbDEhc3M%3D</mark>  
Estes valores de cookies estão codificados em **base64**  
Decodificando cada um deles através do comando abaixo, obtemos:  
> ```bash
> echo dGVzdGUzdjRlcjlsbDEhc3M%3D | base64 -d
> ```
Resultado: <mark>teste3v4er9ll1!ss</mark>

Parte de usuário: <mark>teste1</mark>  
Parte fixa: <mark>v4er9ll1!ss</mark>  

Criando um valor de cookie com a parte fixa, é possível utilizar-se dela para realizar uma troca de sessão entre usuários  
Para _**mcinventory**_, temos: <mark>bWNpbnZlbnRvcnl2NGVyOWxsMSFzcw==</mark>  
Substituindo na aba de cookies ao abrir as ferramentas de programador, é possível obter acesso administrativo pela conta de mcinventory ao recarregar a página
