# _**W1seGuy**_
![](guy.jpg)

## _**Enumeração**_
Vamos nos conectar via ```netcat``` na porta 1337  
Temos uma string de texto  
Vamos analisar o código fornecido por pontos-chave  
> ```bash
> def setup(server, key):
>     flag = 'THM{thisisafakeflag}'  # Known plaintext
>     xored = ""
>     for i in range(0,len(flag)):
>         xored += chr(ord(flag[i]) ^ ord(key[i%len(key)]))
>     return xored.encode().hex()
> 
> def start(server):
>     key = ''.join(random.choices(string.ascii_letters + string.digits, k=5))
>     hex_encoded = setup(server, key)
>     send_message(server, f"This XOR encoded text has flag 1: {hex_encoded}\n")
> ```

Através deste trecho, sabemos que:
* Utiliza XOR de chave repetida com chave alfanumérica de 5 caracteres
* Criptografa uma estrutura de texto simples conhecida (THM{...})
* Fornece a segunda flag após o envio correto da chave

Conseguimos concluir também que o código é vulnerável, pois:
* Estrutura de texto simples conhecida (prefixo THM e sufixo })
* Chave curta (5 caracteres)
* Padrão de chave repetitivo

Procurando pelo código de como é possível explorar isso, encontramos este
> ```
> from pwn import *
> 
> def solve():
>     # Connect to target server
>     conn = remote('[ip_address]', 1337)
>     
>     # Extract ciphertext
>     conn.recvuntil(b"flag 1: ")
>     hex_data = conn.recvline().strip().decode()
>     cipher = bytes.fromhex(hex_data)
>     
>     # Recover 5-byte key
>     key_part1 = bytes([cipher[i] ^ ord('THM{'[i]) for i in range(4)])
>     key_part2 = cipher[-1] ^ ord('}')
>     key = key_part1 + bytes([key_part2])
>     
>     # Decrypt Flag 1
>     flag1 = bytes([cipher[i] ^ key[i%5] for i in range(len(cipher))]).decode()
>     
>     # Get Flag 2
>     conn.sendline(key)
>     flag2 = conn.recvuntil(b'}').decode()
>     
>     print(f"[+] Flag 1: {flag1}")
>     print(f"[+] Flag 2: {flag2}")
>     conn.close()
> 
> if __name__ == "__main__":
>     solve()
> ```
O código:
* Conecta ao servidor	remote(...)
* Recebe flag codificada em XOR	recvuntil, recvline, fromhex()
* Quebra a chave XOR	usando bytes conhecidos da flag fake
* Decifra flag 1	com cipher[i] ^ key[i % 5]
* Envia chave correta	sendline(key)
* Recebe flag real (flag 2)	recvuntil(b'}')

Basta inserir o endereço IP e conseguir as flags
