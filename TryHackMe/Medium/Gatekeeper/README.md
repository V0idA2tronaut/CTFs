# _**Gatekeeper CTF**_

## _**Write-up**_
Primeiro, vamos começar com um scan de rede com <mark>Nmap</mark>
> ```bash
> nmap -p- -sS -T3 [ip_address]
> nmap -p [ports_discovered] --min-rate 1000 --max-rtt-timeout 1000ms --max-retries 5
> ```
![](scan_nmap.jpg)

Parece que temos alguns serviços em diveras portas como:
* 135 --> MSRPC
* 139 --> NetBIOS
* 445 --> Micrososft-ds
* 3389 --> RDP

Começando pela primeira porta, utilizamos o comando abaixo para enumerar MSRPC
> ```bash
> nmap -p 135 --script msrpc [ip_address]
> rpclient -U "" -N [ip_address]
> ```
![](scan_rpc.jpg)

Temos login anônimo com sucesso!  
Continuando nossa enumeração, investigamos a porta 139 com os comandos abaixo
> ```bash
> smbclient -L //[ip_address] -N
> nmap -p 139,445 --script smb-enum-shares,smb-enum-users,smb-vuln* [ip_address]
![](scan_netbios.jpg)

Parece que temos leitura em **IPC$** e **Users**  
Prosseguindo, enumeramos o serviço na porta 3389 com os comandos abaixo:
> ```bash
> nmap -p 3389 --script rdp-enum-encryption,rdp-ntlm-info [ip_address]
> xfreerdp /v:[ip_address] /cert:ignore
> ```
![](rdp_enum.jpg)

