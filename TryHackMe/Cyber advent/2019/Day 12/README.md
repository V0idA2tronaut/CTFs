# _**Contexto de criptografia**_
Criptografia é um método de embaralhar dados em um formato que somente indivíduos autorizados podem entender  
Diferentemente do hash, dados criptografados podem ser revertidos para revelar a mensagem original, enquanto o hash não pode ser revertido (descriptografado) para mostrar os dados originais  
Agora que você entende o conceito geral de criptografia, vamos tentar criptografar e descriptografar alguns dados usando criptografia assimétrica e simétrica

*** 

**Criptografia Simétrica**  
Lembre-se, a criptografia simétrica é onde você usa a mesma chave para criptografar e descriptografar dados  
Vamos usar o programa pré-instalado gpg para criptografar um arquivo. Execute o comando: ```gpg -c [file_name]```  
Ele solicitará uma senha, uma vez concluído ele criará [file_name].gpg  
Se você tentar visualizar o arquivo _.gpg_, verá que todos os dados foram embaralhados  

<mark>O GPG usa o algoritmo AES</mark> para criptografar seus arquivos  
Com a criptografia, o algoritmo usado tem um grande papel na velocidade de criptografia e descriptografia e na segurança do algoritmo (certos algoritmos podem ser facilmente quebrados)  
O Advanced Encryption Standard (AES) foi estabelecido pelo Instituto Nacional de Padrões e Tecnologia dos EUA  

Depois que você enviar seus dados criptografados para seu amigo, ele precisará descriptografá-los para revelar os dados em texto simples  
Ele também deve saber a chave para descriptografar o arquivo  
Se ele executar ```gpg -d [file_name].gpg``` e inserir a chave correta, ele descriptografará o arquivo e ele poderá visualizar sua mensagem secreta  
A integridade dos dados também é importante aqui, podemos pegar um hash do arquivo criptografado e compartilhá-lo com o remetente e o destinatário da mensagem criptografada para garantir que os dados não tenham sido adulterados em trânsito  

*** 

**Criptografia Assimétrica**  
Lembre-se, a criptografia assimétrica usa uma chave pública e privada  
Se você criptografar dados com a chave pública de outra pessoa, eles só poderão ser descriptografados usando a chave privada dessa pessoa  
Lembre-se, qualquer um pode ter acesso à sua chave pública, mas você quer manter sua chave privada segura  

Se alguém obtiver sua chave privada, eles poderão descriptografar qualquer coisa que já tenha sido criptografada usando sua chave pública  
As chaves SSH usam chaves públicas e privadas, você gera uma chave privada, e com isso você tem uma chave pública gerada  
Então você coloca sua chave pública no servidor, então quando você quer fazer SSH em uma máquina, você usa sua chave privada para se autenticar como se o servidor pudesse descriptografar sua mensagem com sua chave pública, ele sabe que o usuário tem a chave privada correspondente  

Então, se você usar sua chave pública para criptografar uma mensagem, ela só pode ser descriptografada com sua chave privada  
Se você usar sua chave privada para criptografar uma mensagem, ela só pode ser descriptografada com sua chave pública  
Vamos gerar algumas chaves públicas/privadas e criptografar/descriptografar uma mensagem:
* **Para gerar uma chave privada, usamos o seguinte comando:** ```openssl genrsa -aes256 -out private.key 8912```
* **Para gerar uma chave pública, usamos nossa chave privada gerada anteriormente:** ```openssl rsa -in private.key -pubout -out public.key```
* **Vamos agora criptografar um arquivo sando nossa chave pública:** ```openssl rsautl -encrypt -pubin -inkey public.key -in plaintext.txt -out encrypted.txt```
* **Se usarmos nossa chave privada, podemos descriptografar o arquivo e obter a mensagem original:** ```openssl rsautl -decrypt -inkey private.key -in encrypted.txt -out plaintext.txt```

# _**Execução**_
Primeira task, basta digitar o comando abaixo com o nome do arquivo:
* md5sum note1.txt.gpg: 24cf615e2a4f42718f2ff36b35614f8f

Segundo, é preciso saber a senha para quando por descriptografar gpg: 25daysofchristmas
* ```gpg note1.txt.gpg```

Terceiro, é preciso também saber a senha para quando for descriptografar o arquivo: hello
* ```openssl rsautl -decrypt -inkey private.key -in note2_encrypted.txt -out plaintext.txt```
