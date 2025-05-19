# _**Contexto de OSINT**_
OSINT são dados coletados de fontes publicamente disponíveis para serem usados ​​em um contexto de inteligência  
Se um invasor executar uma <mark>campanha de phishing de alvo</mark>, isto é, enviar e-mails fraudulentos fingindo ser de uma empresa respeitável, para que eles revelem informações pessoais ou cliquem em um link malicioso  
É incrível a quantidade de informações que as pessoas compartilham sobre si mesmas em plataformas de mídia social, intencionalmente e não intencionalmente  
A estrutura [OSINT](https://osintframework.com/) é uma coleção de recursos e ferramentas que você pode usar para sua coleta de inteligência  

<mark>Metadados</mark> de imagem são informações de texto que pertencem a um arquivo de imagem, que são incorporadas ao arquivo  
Esses dados incluem detalhes relevantes para a imagem em si, bem como informações sobre sua produção  
Por exemplo, se você tirar uma foto no parque, seu smartphone anexará metadados de localização GPS à imagem  
Antigamente, as redes sociais não retiravam os metadados de uma imagem, o que significava que uma celebridade poderia tirar uma foto em casa e enviá-la, revelando sua localização  
Metadados de imagem também podem incluir detalhes da câmera, como abertura, velocidade do obturador e DPI  
Também pode incluir o criador (autor) ou o indivíduo que tirou a imagem  

<mark>Exiftool</mark> é um programa gratuito e de código aberto para ler metadados em arquivos  

O <mark>WayBackMachine</mark> é um arquivo digital da World Wide Web  
Ele tira um instantâneo de um site e o salva para que possamos ver no futuro  
Isso pode ser usado para reunir informações sobre como um site costumava ser  

Não seria legal se você pudesse pesquisar na internet com uma imagem? Bem, nós podemos!  
O Google realmente permite que você pesquise na rede por uma imagem que você tenha  
Se um usuário tem uma foto de perfil de si mesmo em uma mídia social, é mais provável que ele tenha reutilizado a mesma foto em muitos outros sites de mídia social diferentes  
Você pode pegar essa imagem, pesquisar em todos os outros sites por essa imagem e identificar onde esse usuário também se inscreveu  
Também pode ser usado para identificar quem ou o que está em uma imagem  
Então, se você não tiver certeza de quem é alguém em uma imagem (desde que seja uma imagem clara apenas daquele indivíduo), o Google provavelmente lhe dirá  

# _**Execução**_
Utilizando a ferramenta <mark>Exiftool</mark>, é possível encontrar quem criou a imagem: _JLolax1_
Jogando este nome encontrado no Google, temos uma conta no Twitter  
A partir desta conta, é possível determinar data de nascimento, ocupação atual, tipo de telefone que ela produz e um _website/blog_
Neste blog, outras informações pessoais e de contato podem ser encontradas e também, diversas fotos como a de Ada Lovelace
Através do site wayback machine, vemos a data de criação do site e juntando informações do blog (data de criação do site) com informações do twitter, podemos determinar que ela começou a fotografar em _23/04/2014_
