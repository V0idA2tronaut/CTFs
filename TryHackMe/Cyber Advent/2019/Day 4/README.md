# _**Contexto de Arquivos de Sistemas Linux**_

O <mark>Linux</mark> utiliza o padrão de hierarquia do sistema de arquivos, que define a forma como os diretórios/pastas são dispostos  
É importante entender essa estrutura para entender como o sistema operacional funciona  
O diretório mais alto é chamado de diretório raiz (/)  
Como o sistema de arquivos é organizado em uma hierarquia, todos os arquivos e pastas estão localizados no diretório raiz  

Aqui está o layout mais comum do diretório raiz:
* **/bin** -> contém programas na forma de arquivos binários que o usuário pode executar
* **/boot** -> contém os arquivos necessários para iniciar o sistema
* **/dev** -> usado para representar dispositivos (principalmente virtuais) que correspondem a funções específicas
* **/etc** -> contém arquivos de configuração para serviços em um computador e alguns arquivos de sistema
* **/usr** -> contém arquivos executáveis ​​(programas) para a maioria dos programas do sistema
* **/home** -> contém diretórios home para usuários pessoais
* **/lib** -> contém bibliotecas (que contêm funções extras) que são usadas por arquivos executáveis ​​nos diretórios /bin
* **/var** -> contém principalmente arquivos que armazenam informações sobre como os serviços são executados (também conhecidos como arquivos de log)
* **/proc** -> contém informações sobre os processos em execução no sistema
* **/mnt** -> usado para montar sistemas de arquivos. Montagens são geralmente usadas quando os usuários desejam acessar outros sistemas de arquivos em seu sistema
* **/opt** -> contém software opcional
* **/media** -> contém hardware removível, por exemplo, USB
* **/tmp** -> contém arquivos temporários Esta pasta geralmente é limpa na reinicialização, portanto, não armazena arquivos persistentes.
* **/root** -> contém arquivos criados pelo superusuário (mais sobre isso depois

Para aprender os <mark>comandos básicos</mark>, vamos supor o cenário do que um invasor faria se obtivesse acesso a um sistema Linux  
A primeira coisa que um invasor faria seria obter acesso ao sistema  
Isso pode ser feito de muitas maneiras diferentes, mas a mais comum é usando um _shell_  
Um shell é usado para executar comandos do sistema  
Shells também são programas e os shells mais comuns são:
* **/bin/sh**
* **/bin/bash**

Embora os shells sejam bastante semelhantes, eles têm nuâncias na forma como operam  
Você pode executar programas a partir do shell, mas o shell precisa saber onde esses programas estão realmente armazenados  
Ele usa a variável <mark>$PATH</mark> para fazer isso  
Os usuários podem adicionar e remover a localização no caminho  

Agora que um invasor tem um shell no sistema, ele normalmente acessaria o diretório home do usuário  
Os usuários armazenam muitos arquivos no diretório <mark>/home</mark>, então um invasor gostaria de ver quais arquivos estão lá  
Eles fazem isso usando o comando ```ls```

Os comandos do Linux podem ser bastante intimidadores, mas a melhor coisa sobre eles são suas _man pages_  
Uma _man page_ fornece informações sobre um comando específico e como ele é usado  
A maioria dos comandos possui páginas de manual anexadas  

A próxima coisa que um invasor faria seria tentar encontrar arquivos de interesse  
Um arquivo de interesse comum é o _.bash_history_, que registra quais comandos o usuário executou  

Outro arquivo que um invasor acessaria seria o _/etc/passwd_  
Este arquivo fornece informações sobre todos os usuários da máquina  

Um invasor pode encontrar um arquivo grande que deseja pesquisar  
Um invasor pode usar a ferramenta <mark>grep</mark> para extrair um valor específico de um arquivo usando o comando ```grep ‘string’ nome_do_arquivo```  
O grep é ainda mais poderoso e pode pesquisar recursivamente em arquivos com correspondência de padrões mais robusta usando expressões regulares  

É importante que um invasor consiga ler, gravar e acessar arquivos específicos  
Para isso, ele precisa verificar as <mark>permissões de arquivo</mark>  
As permissões de arquivo são exibidas como:
> drwx rwx rwx [nome de usuário] [nome do grupo] [número de arquivos] [tamanho] [data da última modificação] [nome do arquivo]

Um arquivo pode ter permissões de **leitura**, **gravação** ou **executável**  
As permissões são para:
* o usuário que possui os arquivos
* o usuário que faz parte de um grupo com permissões específicas
* qualquer outra pessoa que não se enquadre nesses critérios

Como em qualquer sistema de computador, é necessário um <mark>usuário administrador</mark>  
No Linux, esse usuário é chamado de superusuário e geralmente tem o nome de usuário _root_  
O usuário _root_ sempre terá o ID 0  
Usuários normais também podem receber privilégios de administrador, e isso é mostrado no arquivo _/etc/sudoers_  
Os arquivos contêm entradas no formato: <mark>user (host)=(user:group) commands</mark>  

# _**Execução**_
Primeiro, determina-se em qual diretório nos encontramos com o comando ```pwd```: _/home/mcsysadmin_  
Após, com o comando ```ls``` para verificar quantos arquivos tem neste diretório: 8  
Um simples ```cat file5``` releva o conteúdo do arquivo: _recipes_  
Para encontrar a palavra _password_ dentro de 8 arquivos com longas strings, é preciso utilizar o comando ```grep``` dentro do diretório com todos os arquivos: _grep “password” *_  

Para encontrar o endereço IP escondido dentro de um destes arquivos, utiliza-se dois comandos, ```grep``` e ```cat```
O comando ```cat``` irá mostrar a mensagem na tela e grep irá encontrar o que desejamos
> ```bash
> cat * grep -Eo "([0-9]{1,3}[\.]){3}[0-9]{1,3}"
> ```

Para encontrar a quantidade de usuários que podem realizar login, um simples comando ```cat``` e uma contagem resolve: _cat /etc/passwd_  
<mark>3 usuários: root, mcsysadmin e ec2-user</mark>  

Para encontrar a _hash_ de senha do **mcsysadmin**, primeiro tenta-se o comando ```cat```, mas temos a mensagem _Permission denied_  
É necessário um contorno  
É possível tentar adicionar o usuário _mcsysadmin_ à lista de _sudoers_ com o comando: _sudo usermod -aG sudo mcsysadmin_
A resposta é _Permission denied_  

Sem opções, descobre-se que o arquivo _/shadow_ na maioria das vezes possui um backup _.bak_
Para tentar encontrar este arquivo no sistema, basta usar um dos comandos abaixo
> ```bash
> locate shadow | grep .bak
> ```
> ```bash
> find / -type f -name  *.bak 
> ```
> ```bash
> /var/shadow.bak
> ```
> ```bash
> cat /var/shadow.bak
> ```
