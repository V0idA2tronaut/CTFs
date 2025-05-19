# _**ENUMERAÇÃO → REDES**_

## _**NMAP**_
| firewall | nmap -p [port_range] -A -T[1-5] [ip_address] | 
| --------- |---------- 
| no firewall | nmap -p [port_range] -Pn -A -T[1-5] [ip_address] | 
| all ports | nmap -p- -A -T[1-5] [ip_address] | 

