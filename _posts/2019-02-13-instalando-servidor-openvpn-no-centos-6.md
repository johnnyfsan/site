---
title: 'Instalando Servidor OpenVPN no CentOS 6'
date: '2019-02-13T13:57:56-02:00'
type: post
image:../assets/img/thumb/Instalando-Servidor-OpenVPN-no-CentOS-6-250x250.png
tag:
    - 'configurar openvpn centos'
    - 'instalar openvpn linux'
    - 'vpn linux'
    - 'openvpn linux'
---

- - - - - -

VPN é a Rede Privada Virtual é uma rede de comunicações privada normalmente utilizada por uma empresa ou um conjunto de empresas e/ou instituições, construída em cima de uma rede de comunicações pública (como por exemplo, a Internet). O tráfego de dados é levado pela rede pública utilizando protocolos padrão, não necessariamente seguros.  
VPNs seguras usam protocolos de criptografia por tunelamento que fornecem a confidencialidade, autenticação e integridade necessárias para garantir a privacidade das comunicações requeridas. Quando adequadamente implementados, estes protocolos podem assegurar comunicações seguras através de redes inseguras.

Continue lendo mais em: [Wikipédia](http://pt.wikipedia.org/wiki/Virtual_Private_Network)

Disponível para clientes:

- Windows
- Linux
- MacOS
- Android
- iOS

**Site Oficial do OpenVPN:**<http://openvpn.net/>


### 1. Configurações Iniciais
-------------------------

Alterar o Hostname do Servidor.

```
vim /etc/sysconfig/network
```

```
NETWORKING=yes  
HOSTNAME=openvpn
```

Desativar o SELinux.

```
vim /etc/selinux/config
```

```
SELINUX=disabled
```

Reinicie para validar as alterações.

```
reboot
```


### 2. Instalando os pacotes necessários
------------------------------------

```
yum -y install gcc make rpm-build autoconf.noarch zlib-devel pam-devel openssl-devel wget
```


### 3. Download da LZO RPM e Instalando repositório RPMForge
--------------------------------------------------------

```
wget http://openvpn.net/release/lzo-1.08-4.rf.src.rpm
```

```
wget http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.2-2.el6.rf.x86_64.rpm
```

Instalando…

```
rpmbuild --rebuild lzo-1.08-4.rf.src.rpm
rpm -Uvh lzo-*.rpm
rpm -Uvh rpmforge-release*
```


### 4. Instalando o OpenVPN
-----------------------

```
yum -y install openvpn
```


### 5. Ajustando o OpenVPN
----------------------

Um ajuste a ser realizado após a instalação do pacote ‘openvpn’.  
Copiar a ‘easy-rsa’ para o diretório ‘/etc/openvpn’.

```
cp -R /usr/share/doc/openvpn-2.2.2/easy-rsa/ /etc/openvpn/
```

No CentOS precisamos realizar um acerto no arquivo abaixo.

```
vi /etc/openvpn/easy-rsa/2.0/vars
```

e alterar a seguinte linha:

```
export KEY_CONFIG=`$EASY_RSA/whichopensslcnf $EASY_RSA`
```

para:

```
export KEY_CONFIG=/etc/openvpn/easy-rsa/2.0/openssl-1.0.0.cnf
```

Não esqueça de salvar o arquivo após a alteração.


### 6. Gerando certificados
-----------------------

```
cd /etc/openvpn/easy-rsa/2.0 chmod 755 * source ./vars ./vars ./clean-all
```

Build CA:

```
./build-ca
```

```
Country Name (2 letter code) [US]:BR  
State or Province Name (full name) [CA]:Parana  
Locality Name (eg, city) [SanFrancisco]:Curitiba  
Organization Name (eg, company) [Fort-Funston]:TIDAHORA Organizational 
Unit Name (eg, section) [changeme]:TIDAHORA  
Common Name (eg, your name or your server's hostname) [changeme]:TIDAHORA 
Name [changeme]:Johnny Ferreira  
Email Address [mail@host.domain]:contato@tidahora.com.br
```

Build Server.

```
./build-key-server server
```


```
Country Name (2 letter code) [US]:BR
State or Province Name (full name) [CA]:Parana
Locality Name (eg, city) [SanFrancisco]:Curitiba
Organization Name (eg, company) [Fort-Funston]:TIDAHORA
Organizational Unit Name (eg, section) [changeme]:TIDAHORA
Common Name (eg, your name or your server's hostname) [server]:TIDAHORA
Name [changeme]:Johnny Ferreira
Email Address [mail@host.domain]:johnny@TIDAHORA.com.br
 
Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:senha
An optional company name []:TIDAHORA
Using configuration from /etc/openvpn/easy-rsa/2.0/openssl-1.0.0.cnf
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
countryName           :PRINTABLE:'BR'
stateOrProvinceName   :PRINTABLE:'Parana'
localityName          :PRINTABLE:'Curitiba'
organizationName      :PRINTABLE:'TIDAHORA'
organizationalUnitName:PRINTABLE:'TIDAHORA'
commonName            :PRINTABLE:'TIDAHORA'
name                  :PRINTABLE:'Johnny Ferreira'
emailAddress          :IA5STRING:'contato@tidahora.com.br'
Certificate is to be certified until Mar  2 13:00:57 2023 GMT (3650 days)
Sign the certificate? [y/n]:y
 
 
1 out of 1 certificate requests certified, commit? [y/n]y
Write out database with 1 new entries
Data Base Updated
```


Build Diffie Hellman.

```
./build-dh
```

Só esperar….

```
Generating DH parameters, 1024 bit long safe prime, generator 2
This is going to take a long time
.........................................+.................+.......+.....+....
```


### 7. Criando o arquivo de configuração
------------------------------------

```
vim /etc/openvpn/server.conf
```

```
port 1194  
proto udp  
dev tun  
tun-mtu 1500  
tun-mtu-extra 32  
mssfix 1450  
reneg-sec 0  
ca /etc/openvpn/easy-rsa/2.0/keys/ca.crt  
cert /etc/openvpn/easy-rsa/2.0/keys/server.crt  
key /etc/openvpn/easy-rsa/2.0/keys/server.key  
dh /etc/openvpn/easy-rsa/2.0/keys/dh1024.pem  
plugin /usr/share/openvpn/plugin/lib/openvpn-auth-pam.so /etc/pam.d/login  
plugin /etc/openvpn/radiusplugin.so /etc/openvpn/radiusplugin.cnf 
client-cert-not-required  
username-as-common-name  
server 10.20.0.0 255.255.255.0  
push "redirect-gateway default"  
push "dhcp-option DNS 8.8.8.8" 
push "dhcp-option DNS 8.8.4.4"  
keepalive 5 30  
comp-lzo  
persist-key  
persist-tun  
status 1194.log  
verb 3
```


### 8. Iniciando o OpenVPN
----------------------

```
/etc/init.d/openvpn start
Iniciando o openvpn:                                       [  OK  ]
```

Agora precisamos ativar o ‘ip\_forward’.

```
vim /etc/sysctl.conf
```

Altere conforme a linha abaixo.

```
[...]
net.ipv4.ip_forward = 1
[...]
```

Rode o comando abaixo para validar as alterações no ‘ip\_forward’.

```
sysctl -p
```


### 9. Configurando clientes
-------------------------

Vamos efetuar a configuração de um cliente para a conexão VPN.

Adicionando no sistema.

```
useradd usuario -s /bin/false passwd usuario
```

Vamos criar o seguinte arquivo ‘server.ovpn’

```
vim /etc/openvpn/server.ovpn
```

```
client dev tun 
proto udp 
remote 120.120.120.120 
resolv-retry 
infinite nobind 
tun-mtu 1500 
tun-mtu-extra 32 
mssfix 1450 
persist-key persist-tun 
ca ca.crt 
auth-user-pass 
comp-lzo 
reneg-sec 0 
verb 3
```

Reinicie o serviço.

```
/etc/init.d/openvpn restart
Desligando o openvpn:                                      [  OK  ]
Iniciando o openvpn:                                       [  OK  ]
```


### 10. Ajustando o Firewall
------------------------

As configurações de firewall são de necessidade obrigatória para a configuração correta do OpenVPN. Abaixo segue as regras de iptables para liberação da conexão VPN, lembrando assim que em cada firewall terá de ser configurado a modo do administrador de redes conforme IP, políticas de acessos, etc.

```
iptables -t nat -A POSTROUTING -s 10.20.0.0/24 -o eth0 -j MASQUERADE   
iptables -t nat -A POSTROUTING -o venet0 -j SNAT --to-source 120.120.120.120  
iptables -t nat -A POSTROUTING -s 10.20.0.0/24 -j SNAT --to-source 120.120.120.120  
```


### 11. Ajustando o cliente para conectar a VPN
-------------------------------------------

Faça o Download do client no link abaixo:  
[https://openvpn.net/index.php?option=com\_content&amp;id=357](https://openvpn.net/index.php?option=com_content&id=357)

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Até a próxima!

- - - - - -

![](../assets/img/uploads/2019/02/foto-redonda.png)

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>