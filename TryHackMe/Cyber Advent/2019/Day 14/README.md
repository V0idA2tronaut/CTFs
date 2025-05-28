# _**Contexto de AWS s3 Bucket**_
Veremos o armazenamento em nuvem inseguro, mais especificamente, os buckets S3 inseguros da Amazon Web Services (AWS)  
Hoje, muito mais empresas estão migrando sua computação e infraestrutura para a "nuvem":
* **Escalável:** A maioria dos provedores de serviços em nuvem tem a capacidade não apenas de criar uma grande quantidade de recursos sob demanda, mas também de gerenciar automaticamente a criação desses recursos
* **Alta disponibilidade:** Para garantir que os recursos não fiquem inativos, os provedores de nuvem permitem que as empresas gerenciem o failover duplicando recursos em diferentes regiões

Veja aqui uma análise das seguintes permissões:
* **Listar objetos:** usuários com permissões podem listar os arquivos no _bucket_
* **Escrever objetos:** usuários com permissões podem adicionar/remover arquivos no _bucket_
* **Leitura de permissões no bucket:** usuários com permissões podem ler arquivos no _bucket_
* **Escrita de permissões no bucket:** usuários com permissões podem editar arquivos no _bucket_

As permissões acima se aplicam ao _bucket_, mas um administrador também pode atribuir permissões específicas a arquivos/objetos no bucket  
Um administrador pode atribuir permissões das seguintes maneiras:
* Para usuários específicos
* Para todos
Anteriormente, as permissões padrão do S3 eram fracas e os buckets do S3 eram acessíveis publicamente, mas a AWS alterou isso para bloquear o acesso público por padrão

***

**ENUMERAÇÃO**  
A primeira parte da enumeração de buckets S3 é ter um nome de bucket S3  
Como você encontraria o nome de um bucket S3:
* Código-fonte em repositórios Git
* Analisando requisições em páginas web
* Algumas páginas recuperam recursos estáticos de buckets S3

Nome de domínio de produtos:
* Se um produto ou domínio for chamado de "servicename", o bucket S3 também poderá ser chamado de "servicename"

Assim que tivermos um bucket S3, podemos verificar se ele é acessível publicamente navegando até a URL  
O formato da URL é: <mark>bucketname.s3.amazonaws.com</mark>  
Já falamos sobre o suporte da AWS a múltiplas regiões  
Mesmo que os buckets S3 sejam globais, ainda podemos acessá-los em sua região: <mark>bucketname.region-name.amazonaws.com</mark>  

Embora incomum, os _buckets S3_ podem ser configurados de forma que qualquer usuário autenticado possa acessá-los  
Nesse caso, você poderá listar os objetos em um bucket usando a AWS CLI  
Se você encontrar objetos em um bucket S3, poderá baixá-los para visualizar seu conteúdo  
Isso é feito usando a AWS CLI  

Para usar a AWS CLI, você precisa criar uma conta  
Após criar uma conta AWS, você pode verificar o conteúdo do bucket usando o comando: ```aws s3 ls s3://bucket-name```  
Para baixar os arquivos, você pode usar o comando: ```aws s3 cp s3://bucket-name/file-name local-location```  
Como alternativa, você também pode usar o seguinte método para acessar um arquivo: <mark>bucketname.region-name.amazonaws.com/file-name</mark>  

# _**Execução**_
> ```bash
> aws s3 ls s3://advent-bucket-one
> ```
Encontra-se um arquivo de nome employee_names.txt

> ```bash
> aws s3 cp s3://advent-bucket-one/employee_names.txt
> ```
