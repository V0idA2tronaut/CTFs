# _**Contexto do Metasploit**_
O <mark>Metasploit</mark> é um framework de testes de penetração que facilita o "hacking" e é uma ferramenta importantíssima no setor de segurança  
Com o Metasploit, você pode escolher seu _exploit_ e _payload_ e executá-los no alvo escolhido  
O Metasploit vem com muitas outras ferramentas, como o <mark>MSFVenom</mark>, para criar _shellcode_ personalizado, que, quando executado com sucesso, fornecerá um shell na máquina alvo  
Você pode baixar o Metasploit do GitHub ou ele vem pré-instalado em todas as máquinas com Kali Linux  

***

**Introdução ao Metasploit**
Após a instalação do Metasploit, digite ```msfconsole``` no seu console para iniciar a interface do console do <mark>Metasploit Framework</mark>  
Se você identificou um serviço em execução e encontrou uma vulnerabilidade online para essa versão do serviço ou software em execução, você pode pesquisar todos os nomes e descrições dos módulos do Metasploit para verificar se há código de exploração pré-escrito disponível  

Por exemplo, se você quiser pesquisar todos os módulos do Metasploit para o IIS, execute o seguinte comando dentro do msfconsole: ```search iis```  
Podemos selecionar um módulo usando o seguinte comando: ```use [nome_do_módulo]```  
Assim que o módulo for carregado, podemos visualizar suas opções executando o comando ```show options```  
Normalmente, serão exibidos **RHOST(S)** e **RPORT**, onde você especifica o nome do host/IP de destino e a porta associada  
Também serão exibidos **LHOST** e **LPORT**, onde você especifica os detalhes de conexão da sua própria máquina para que o _payload_ do Metasploit saiba onde se conectar novamente  
Dependendo do módulo, haverá outras opções que serão usadas durante sua execução, como o URI de destino  

*** 

Você tem um aplicativo vulnerável e o módulo para explorá-lo  
Agora é preciso selecionar um _payload_ compatível com o sistema do aplicativo vulnerável  
Também precisamos levar em consideração os diferentes tipos de shell:
* **Podemos ter um <mark>shell reverso</mark>, que é onde, quando o sistema vulnerável é comprometido, ele estabelece uma conexão de volta com a sua máquina, que está escutando conexões de entrada**
* **Também podemos ter um <mark>shell normal</mark>, onde, quando o sistema é comprometido, ele escuta conexões de entrada e nos permite estabelecer uma conexão com ele**

***

Podemos listar todos os payloads disponíveis para todas as plataformas com o comando ```show payloads```  
Você pode selecionar _payloads_ que apenas concedem acesso ao shell ou executam comandos  
Um <mark>payload do Metasploit (meterpreter)</mark> oferece acesso interativo não apenas para controlar uma máquina por meio de um shell, mas também para tirar capturas de tela da máquina, carregar/baixar arquivos facilmente e muito mais  
Ao pesquisar os payloads, encontre onde diz 'meterpreter'  
O Meterpreter é implantado inteiramente na memória e se insere em outros processos existentes do sistema  

# _**Execução**_
