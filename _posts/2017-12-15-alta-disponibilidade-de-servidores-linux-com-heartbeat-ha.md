---
layout: post
date: '2017-12-15T11:27:47-02:00'
title: 'Alta Disponibilidade de Servidores Linux com Heartbeat (HA)'
subtitle:
image: /site/assets/img/uploads/2017/12/alta-disponibilidade-servidores-linux-heartbeat-ha.png
share-img: /site/assets/img/uploads/2017/12/alta-disponibilidade-servidores-linux-heartbeat-ha.png
tag:
    - 'alta disponibilidade'
    - 'alta disponibilidade centos'
    - 'alta disponibilidade linux'
    - 'configurar alta disponibilidade linux'
    - 'configurar ha linux'
    - 'criar servidor linux heartbeat'
    - 'ha centos'
    - 'ha linux'
    - 'heartbeat centos'
    - 'heartbeat linux'
    - 'heartbeat no centos'
    - 'how to configure heartbeat on centos'
    - 'how to heartbeat on linux'
    - 'instalando heartbeat'
    - 'servidor linux alta disponibilidade'
    - 'servidor linux ha'
comments: true
---
- - - - - -

Olá, tudo bem?

Muitas vezes temos a necessidade de manter dois servidores linux sincronizados identicamente, seja para rodar serviços em um Web Server, ou até mesmo um serviço de backup, bem a variedades de serviços que tendem a funcionar em HA é extensa.

Nesse tutorial de hoje vou abordar a instalação do Heartbeat em dois servidores linux CentOS 6.

Nosso ambiente é o seguinte:

**Servidor Master:**  
**Hostname:** firewall-01  
**Endereço IP Lan:** 10.1.0.1/16

**Servidor Slave:**  
**Hostname:** firewall-02  
**Endereço IP Lan:** 10.1.0.2/16

#### Passo 1: Configurar o arquivo Hosts dos servidores

Realize a edição dos arquivos Master e Slave.

```
vim /etc/hosts
```

Adicione ao final do arquivo:

```
# NODES DE HEARTBEAT - HA
10.1.0.1	firewall-01	firewall-01.tidahora.local
10.1.0.2	firewall-02	firewall-02.tidahora.local
```

#### Passo 2: Instalando o Heartbeat

Instale o pacote “heartbeat” nos dois servidores:

```
yum -y install heartbeat
```

Caso você não localize esse pacote no seu CentOS, realize a instalação do repositório “Epel-Release”.

Ajustando a inicialização automática do Heartbeat junto ao boot do sistema.

Execute no Master e Slave:

```
chkconfig heartbeat on
```

#### Passo 3: Configurando o Heartbeat

A configuração do Heartbeat é feita somente em 3 arquivos, são eles: */etc/ha.d/ha.cf* */etc/ha.d/authkeys /etc/ha.d/haresources.*

Ajustando o arquivo ‘ha.cf’, ele deve ser idêntico nos dois servidores.

Criando o arquivo ha.cf (Master e Slave):

```
vim /etc/ha.d/ha.cf
```

Conteúdo do arquivo “ha.cf”

```
use_logd yes
keepalive 2
deadtime 10
warntime 5
initdead 120
bcast eth0
udpport 694
node firewall-01 firewall-02
auto_failback on
logfile /var/log/ha-log
```

Criando o arquivo authkeys (Master e Slave):

```
vim /etc/ha.d/authkeys
```

Dentro desse arquivo iremos colocar a senha de autenticação, esse arquivo deve ser idêntico nos dois servidores.

```
auth 1
1 sha1 SenhaAqui
```

Ajustando a permissão no arquivo:

```
chmod 600 /etc/ha.d/authkeys
```

Ajustando o arquivo ‘haresources’ (Master e Slave):  
Este arquivo deve ser igual em todos os servidores, é um arquivo bem simples, contendo o hostname do Nó Master, o IP do Master, ou seja, o IP que o Slave irá assumir em caso de queda do Master.

```
vim /etc/ha.d/haresources
```

Conteúdo do arquivo:

```
firewall-01 10.1.0.1
```

#### Passo 4. Iniciando o Heartbeat

Iniciando o Heartbeat nos dois servidores (Master e Slave):

```
/etc/init.d/heartbeat start
```

Verificando os logs:

```
tail -f /var/log/ha-log
```

Pronto, agora você já possui alta disponibilidade em servidores linux.

Pode fazer testes na sequência para garantir a boa operação do serviço, derrube o servidor master, e verifique os logs, pingue o endereço IP do Master e note que o mesmo irá subir corretamente no servidor Slave.

Lembrando que você pode utilizar o Heartbeat para implementar em qualquer serviço que veja necessidade no seu ambiente.

Espero ter ajudado de alguma forma.

👋🏼 Vlw flw e até a próxima!

- - - - - -

![](/site/assets/img/uploads/2017/11/foto-perfil-redondo-johnny.png)

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -