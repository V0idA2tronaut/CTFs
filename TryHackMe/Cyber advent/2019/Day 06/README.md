# _**Contexto de Exfiltração**_
<mark>Exfiltração de dados</mark> é a técnica de transferir dados não autorizados para fora da rede, é por isso que as empresas geralmente têm sistemas de Prevenção de Perda de Dados em vigor  
Infelizmente, os _Sistemas de Prevenção de Perda de Dados_ não são perfeitos e podem permitir que dados classificados saiam da rede  
É aqui que entra o Centro de Operações de Segurança  

Parte do trabalho de um <mark>Analista de Nível 1 do SoC</mark> é determinar se um ataque realmente ocorreu, isso pode ser identificado por muitos meios  
Uma maneira fácil de saber é revisar os **logs do wireshark** da rede e procurar por técnicas de exfiltração de dados  
É muito mais fácil identificar dados que você sabe que não deveriam sair da rede do que determinar se um _APT_ está lá dentro  
Os _APTs_ geralmente podem estar escondidos nas profundezas da rede, muitas vezes abusando do Kerberos e seu Sistema de Concessão de Tickets para obter acesso permanente  

Algumas técnicas de Exfiltração de Dados incluem, mas não se limitam a:
* **DNS**
* **Transferência de arquivos baseada em FTP/SFTP**
* **Transferência de arquivos baseada em SMB**
* **HTTP/HTTPS**
* **Métodos esteganográficos, como ocultar dados em imagens**
* **ICMP**

<mark>Como identificamos tentativas de Exfiltração de Dados?</mark>  
Pode ser incrivelmente simples com o **Wireshark**, desde que não esteja ocorrendo por meio de um protocolo criptografado  
O **DNS** é a técnica mais comum usada em Exfiltração de Dados, principalmente porque se mistura ao tráfego normal  
Este protocolo é usado em quase todos os dispositivos da sua rede, e pode ser incrivelmente difícil determinar tráfego malicioso  
Um invasor, por exemplo, pode ter seu site configurado, ouvindo dados em qualquer subdomínio diferente de _www_ e gravando-os em um arquivo para examinar o conteúdo posteriormente  

Outra técnica é usar fotos para transferir dados por meio de métodos de **esteganografia**  
O invasor pode incorporar um arquivo oculto em uma imagem usando um utilitário como o <mark>Steghide</mark>  
Se notarmos que um IP específico solicitou uma imagem aparentemente aleatória específica, podemos optar por investigar  

<mark>Um processo similar pode ser feito com todos os tipos de arquivo</mark>  
Desde que o arquivo esteja em HTTP, TFTP, FTP ou SMB, os dados podem ser extraídos da captura de pacotes  
Alguns invasores podem tentar ofuscar os dados ainda mais colocando-os em um arquivo **zip criptografado**  
Felizmente para nós, existem ferramentas como o <mark>fcrackzip</mark> que nos permitem forçar arquivos _zip_  

A sintaxe é a seguinte
> ```bash
> fcrackzip -b --method 2 -D -p /usr/share/wordlists/rockyou.txt -v ./file.zip
> ```

Temos as seguintes flags:
* -b especifica força bruta,
* --method 2 especifica um arquivo Zip
* -D especifica um Dicionário
* -V verifica se a senha está realmente correta

# _**Execução**_
Realizando o download do arquivo _.pacp_ e abrindo no aplicativo <mark>Wireshark</mark>, coloca-se um filtro DNS para investigar os pacotes capturados deste protocolo de rede  

A requisição está criptografada em código HEX: **43616e64792043616e652053657269616c204e756d6265722038343931**  
Tradução: <mark>Candy Cane Serial Number 8491</mark>  

Mudando o parâmetro de filtros para HTTP, tem-se dois tipos de arquivos, um _.zip_ e outro _.jpg_  
É possível extrair estes arquivos seguindo os passos abaixo:
* Arquivo -> Exportar objetos -> HTTP -> [file_name].jpg
* Arquivo -> Exportar objetos -> HTTP -> [file_name].zip

O primeiro arquivo (.jpg) é uma simples imagem do TryHackMe  
Com o comando abaixo através da ferramenta, tenta-se extrair as informações ocultas da imagem  
> ```bash
> steghide extract -sf ./<arquivo para extrair>.jpg
> ```

O arquivo encontra-se criptografado, porém, a senha é apenas um enter  
O conteúdo é salvo em um arquivo _.txt_ e um simples ```cat [file_name]``` revela seu conteúdo: <mark>RFC527</mark>  

O arquivo _.zip_ também se encontra criptografado  
Podemos utilizar a ferramenta <mark>fcrackzip</mark> para tentar um ataque de força bruta e encontrar a senha de criptografia  
Segue o comando abaixo
> ```bash
> fcrackzip -b --method 2 -D -p /usr/share/wordlists/rockyou.txt -v ./file.zip
> ```
Senha: <mark>december</mark>  

Extraindo o conteúdo do arquivo _.zip_, observa-se que são diversos arquivos _.txt_ de lista de presentes e desejos para o natal  
Para o timmy, ele deseja ser um _PenTester_
