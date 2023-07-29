---
layout: post
date: '2019-01-30T15:46:16-02:00'
title: 'Servidor Proxy Squid no Linux Ubuntu'
subtitle:
thumbnail-img: /assets/img/uploads/2017/12/servidor-proxy-squid-no-linux-ubuntu.png
share-img: /assets/img/uploads/2017/12/servidor-proxy-squid-no-linux-ubuntu.png
tag:
    - 'instalar servidor linux squid proxy'
    - 'proxy ubuntu'
    - 'servidor proxy linux'
    - 'servidor squid linux'
comments: true
---
- - - - - -


Fala galera, beleza? 😎 Vamos implementar hoje um servidor squid no Ubuntu 16.04, com autenticação local, esse tutorial está bem detalhado, porém o nível dessa implementação é para quem está iniciando no mundo linux e quer aprender a configurar um servidor proxy squid da maneira mais simples e objetiva possível.

Se você não possui o Ubuntu instalado, ou não sabe como instalar, clica 👇🏼 aqui embaixo que temos um vídeo bem rápido de como fazer a instalação do Ubuntu no VirtualBox para você aprender cada vez mais trabalhar com sistemas Linux.  
👉🏼 Link do vídeo de instalação do Ubuntu no VirtualBox: <https://www.youtube.com/watch?v=HUvP8LvEaws>

#### Passo 1: Atualizando o Ubuntu

```
sudo apt-get update
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-1.png) 


#### Passo 2: Instalando o Squid no Linux Ubuntu 16.04

```
sudo apt-get install squid
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-2.png) 


#### Passo 3: Configurando o Squid no Linux Ubuntu 16.04

Vamos efetuar uma cópia de backup do arquivo “squid.conf”

```
sudo cp -Rfa /etc/squid/squid.conf{,.bkp}
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-3.png) 

Acesse o diretório de configuração do do Squid:

```
cd /etc/squid
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-4.png) 

Listando os arquivos no diretório:

```
ls -l
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-5.png) 

Agora vamos apagar o arquivo “squid.conf” e criar um novo, somente com as opcoes que desejamos:

```
sudo rm -rf squid.conf
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-6.png) 

Em seguida vamos criar o nosso novo arquivo de configuração.

```
sudo touch squid.conf
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-7.png) 

Agora vamos editar o arquivo criado, vou utilizar o “vim”.

```
sudo vim squid.conf
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-8.png) 

Conteúdo do arquivo:


```
### ARQUIVO DE CONFIGURACAO DO SQUID ###

## DEFINE A PORTA DE CONEXAO DO SQUID
http_port 3128

## DEFINE O TAMANHO MAXIMO DE UM OBJETO PARA SER ARMAZENADO EM CACHE ##
maximum_object_size 4096 KB

## DEFINE O TAMANHO MINIMO DE UM OBJETO PARA SER ARMAZENADO EM CACHE ## 
minimum_object_size 0 KB

## DEFINE O TAMANHO MAXIMO DE UM OBJETO PARA SER ARMAZENADO EM CACHE DE MEMORIA ## 
maximum_object_size_in_memory 64 KB

## DEFINE A QUANTIDADE DE MEMORIA RAM A SER ALOCADA PARA CACHE ## 
cache_mem 512 MB

## AJUSTA A PERFORMANCE EM CONEXOES PIPELINE ##
pipeline_prefetch on

## CACHE DE FQDN ##
fqdncache_size 1024

## OPCOES DE REFRESH PATTERN ##
refresh_pattern ^ftp: 1440 20% 10080
refresh_pattern ^gopher: 1440 0% 1440
refresh_pattern -i (/cgi-bin/|\?) 0 0% 0
refresh_pattern . 0 20% 4320

## DEFINE A PORCENTAGEM DO USO DO CACHE ## 
cache_swap_low 90
cache_swap_high 95

## ARQUIVO DE LOGS DO SQUID ## 
access_log /var/log/squid/access.log squid
cache_log /var/log/squid/cache.log
cache_store_log /var/log/squid/store.log

## DEFINE O LOCAL DO CACHE ##
cache_dir ufs /var/spool/squid 1600 16 256

## CONTROLE DE ROTACAO DOS ARQUIVOS DE LOGS ##
logfile_rotate 10

## ARQUIVO ONDE CONTEM OS ENDERECOS LOCAIS DA REDE ##
hosts_file /etc/hosts

## ACLS - PORTAS PADROES LIBERADAS ##
acl SSL_ports port 80 #HTTP
acl SSL_ports port 443 #HTTPS
acl Safe_ports port 80 # http
acl Safe_ports port 21 # ftp
acl Safe_ports port 443 # https
acl Safe_ports port 70 # gopher
acl Safe_ports port 210 # wais
acl Safe_ports port 1025-65535 # unregistered ports
acl Safe_ports port 280 # http-mgmt
acl Safe_ports port 488 # gss-http
acl Safe_ports port 591 # filemaker
acl Safe_ports port 777 # multiling http
acl CONNECT method CONNECT

### DEFININDO MODO DE AUTENTICACAO
auth_param basic program /usr/lib/squid3/basic_ncsa_auth /etc/squid/usuarios
auth_param basic children 5
auth_param basic realm "DIGITE SEU USUARIO E SENHA PARA ACESSO A INTERNET:"
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive off

### ACL PARA GARANTIR A AUTENTICACAO DO USUARIO NOS SITES ###
acl autenticados proxy_auth REQUIRED

## BLOQUEIA O ACESSO UNSAFE PORTS ##
http_access deny !Safe_ports

## Deny CONNECT to other than secure SSL port ##
http_access deny CONNECT !SSL_ports

## SITES BLOQUEADOS PARA ACESSO ##
acl sites-bloqueados url_regex -i "/etc/squid/regras/sites_bloqueados"

## SITES LIBERADOS PARA ACESSO ##
acl sites-liberados url_regex -i "/etc/squid/regras/sites_liberados"

## DEFININDO A ORDEM DAS REGRAS - ACLS ##
http_access deny sites-bloqueados
http_access allow autenticados
http_access allow sites-liberados
http_access deny all
http_reply_access allow all
icp_access allow all
miss_access allow all

## NOME QUE IRA APARECER NA TELA DE ERRO OU BLOQUEIO DO SQUID ##
visible_hostname proxy.tidahora.com.br

## DIRETORIO DAS PAGINAS DE ERROS ##
error_directory /usr/share/squid/errors/pt-br

## OUTRAS OPCOES DE CACHE ##
cache_effective_user proxy
coredump_dir /var/spool/squid
```


Agora vamos criar o diretório onde vamos criar as listas de sites bloqueados e liberados.


```
sudo mkdir /etc/squid/regras

```


![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-9.png) 


Vamos criar o arquivo de sites liberados e bloqueados:


```
sudo touch /etc/squid/regras/sites_liberados
sudo touch /etc/squid/regras/sites_bloqueados
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-10.png) 

Vamos inserir algum site na lista de liberados e na lista de bloqueados:

```
sudo vim /etc/squid/regras/sites_liberados
```

Conteúdo:

```
.tidahora.
.uol.
```


```
sudo vim /etc/squid/regras/sites_bloqueados
```

Conteúdo:

```
.globo.
```

Criando o cache:

```
sudo chmod -Rf 774 /var/spool/squid
```


```
sudo squid -z
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-11.png) 


Inicie o serviço do Squid:

```
sudo systemctl start squid
```

Verificando o status do serviço:

```
sudo systemctl status squid
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-12.png) 


#### PASSO 4: CRIANDO OS USUÁRIOS PARA ACESSO A INTERNET

Instale o Apache no servidor Linux Ubuntu, o apache possui um programa que iremos utilizar para gerar os logins e senhas.

```
sudo apt-get install apache2
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-13.png) 

Criando os usuários com o comando “htpasswd”

Utilize o comando abaixo, somente pela primeira vez, para criar o arquivo:

```
sudo htpasswd -c /etc/squid/usuarios johnny
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-14.png) 

Para os demais usuários utilize:

```
sudo htpasswd /etc/squid/usuarios jferreira
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-15.png) 

Agora vamos recarregar as configurações do squid:

```
sudo squid -k reconfigure
```

#### PASSO 5: AJUSTANDO O ENCAMINHAMENTO DE PACOTES NO KERNEL

Para que possamos acessar a Internet através de um servidor proxy no Linux, é preciso ativarmos o encaminhamento de pacotes no kernel, para que possamos compartilhar a rede do servidor.

Esse procedimento precisa ser feito pelo usuário “root”:

Caso voce nao tenha definido a senha para o usuario root, vamos definir abaixo:

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-16.png) 

Acessando o console como “root”

```
sudo su -
```

Habilitando o encaminhamento de pacotes:

```
echo 1 >> /proc/sys/net/ipv4/ip_forward
```

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-18.png) 


#### PASSO 6: TESTANDO O SERVIDOR SQUID PROXY NO UBUNTU LINUX

Ajuste o endereço IP do servidor nas configurações de Proxy do navegador.

Se você nao sabe qual o endereço IP do seu servidor digite:

```
sudo ifconfig
```

Abra o Firefox e siga os passos abaixo:

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-19.png) 

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-20.png) 

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-21.png) 

Feche as configurações e o navegador e abra-o novamente.

Será solicitado Login e Senha para acesso a Internet:

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-22.png) 

Digite um site que está na lista de sites liberados:

www.uol.com.br

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-23.png) 

Vamos acessar um site que está na lista de Bloqueados:

www.globo.com

![]( /assets/img/uploads/2017/12/servidor-squid-proxy-ubuntu-24.png) 

Pronto, agora você já possui um servidor Proxy Squid em Linux Ubuntu. 😎

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Até a próxima!

- - - - - -

 ![]( /assets/img/uploads/2017/11/foto-perfil-redondo-johnny.png)  **Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>