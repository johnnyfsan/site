---
title: 'Configurando Interface de Rede no CentOS'
date: '2017-12-07'
image: ../assets/img/uploads/2017/12/configurando-interface-rede-centos-1.png
category:
    - 'CentOS'
    - 'Dicas de Linux'
    - 'Linux'
tag:
    - 'config network centos'
    - 'config network linux'
    - 'configurar eth0 centos'
    - 'configurar eth0 linux'
    - 'configurar interface centos'
    - 'configurar interface linux'
    - 'configurar interface rede centos'
    - 'interface centos'
    - 'interface linux'
    - 'network linux'
    - 'network linux centos'

---

- - - - - -


Olá! 👨🏻‍💻

Para fazer a configuração de rede em ambiente CentOS, acesse o diretório *“/etc/sysconfig/network-scripts/”:*

```
cd /etc/sysconfig/network-scripts/
```

Vamos efetuar a configuração da interface *“eth0”:* Com o editor de textos de sua preferencia, abra o arquivo *“ifcfg-eth0”,* Estarei utilizando o “vi” para editar o arquivo:

```
vi ifcfg-eth0
```

Dentro do arquivo teremos as linhas abaixo, realize a alteração conforme sua necessidade:

```
DEVICE=eth0
IPADDR=192.168.1.2
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
ONBOOT=yes
```

Salve o arquivo, no caso do editor “vi” pressione a tecla “ESC” + “:wq”.

O próximo passo é ajustar as configurações de DNS:  
Com seu editor de textos, abra o arquivo *“/etc/resolv.conf”*

```
vi /etc/resolv.conf
```

E insira as informações do servidor de DNS:

```
# DNS DA UOL
nameserver 200.221.11.100
nameserver 200.221.11.101
```

ou

```
# DNS DA GOOGLE
nameserver 8.8.8.8
nameserver 8.8.4.4
```

Salve o arquivo, para quem ta usando o “vi” pressione a tecla “ESC” + “:wq”.

Agora reinicie o serviço de rede do CentOS para validar as alterações:

```
service network restart
```

ou

```
/etc/init.d/network restart
```

Se você está utilizando o CentOS 7.

```
systemctl restart network.service
```

Agora vamos testar o acesso pingando um endereço:

```
ing www.tidahora.com.br
PING tidahora.com.br (52.4.124.38): 56 data bytes
64 bytes from 52.4.124.38: icmp_seq=0 ttl=42 time=173.976 ms
64 bytes from 52.4.124.38: icmp_seq=1 ttl=42 time=169.683 ms
64 bytes from 52.4.124.38: icmp_seq=2 ttl=42 time=170.134 ms
64 bytes from 52.4.124.38: icmp_seq=3 ttl=42 time=170.139 ms
```

Pronto, seu sistema CentOS já está pronto para responder serviços de rede.

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Valeu! e até a próxima!

- - - - - -

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -