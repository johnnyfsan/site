---
title: 'Instalando Ubiquiti Unifi Controller no CentOS 7'
date: '2019-05-23T22:16:38-03:00'
type: post
image: /assets/img/thumb/Instalando-Ubiquiti-Unifi-Controller-no-CentOS-7.png
tag:
    - 'controlador ubiquiti centos'
    - 'controlador ubiquiti linux'
    - 'controlador ubiquiti unifi centos 7'
    - 'controlador ubiquiti unifi linux'
    - 'controlador ubiquiti unifi linux centos'
    - 'controlador ubiquiti unifi linux install'
    - 'controller install ubiquiti unifi'
    - 'controller ubiquiti linux'
    - 'controller ubiquiti linux centos'
    - 'instalar controlador ubiquiti unifi centos 7'
    - 'instalar controlador ubiquiti unifi linux'
    - 'instalar controller ubiquiti linux'
    - 'instalar ubiquiti linux'
---

- - - - - -

Tutorial de instalação do Controlador de Acesso e Configurações do Ubiquiti Unifi no Linux CentOS 7.

Com certeza você já ouviu falar do Ubiquiti, um dos fortes concorrentes da Cisco.  
Se você pensa em instalar um equipamento desse em sua rede, saiba que você precisará de um controlador na rede para gerenciar os dispositivos, seja ele um Access Point ou um USG (Unifi Security Gateway).

Com o controller é possível manter a organização e centralização da rede Wi-Fi, criando várias redes, e segmentando de acordo com a sua necessidade.

Sem mais delongas, vamos instalar o nosso Controlador no bom e velho Linux.

Atualizando o Ambiente &amp; Sistema Operacional:


```
yum -y update
```


Desativando SELinux:


```
vim /etc/selinux/config
```


Altere a linha 7 de “enforcing” para “disabled”.


```
SELINUX=disabled
```


Agora vamos desativar o Firewalld:


```
systemctl stop firewalld
systemctl disable firewalld
```


Após isso, vamos reiniciar o servidor.


```
reboot
```


Depois do reboot instale o repositório EPEL no Linux CentOS 7.


```
yum -y install epel-release
```


Vamos criar um usuário para gerenciar o diretório do Ubiquiti dentro do sistema Linux.


```
useradd -r ubnt
```


Instalando o MongoDB Server e Java para implementação do controller.


```
yum -y install mongodb-server java-1.8.0-openjdk
```


Vamos precisar instalar o UnZip e o Waget também:


```
yum -y install unzip wget
```


O próximo passo é baixar os arquivos de configuração e instalação do Contoller.


```
cd /tmp
wget http://dl.ubnt.com/unifi/5.6.39/UniFi.unix.zip
unzip -q UniFi.unix.zip -d /opt
chown -R ubnt:ubnt /opt/UniFi
```


Criando o serviço que será gerenciado e executado pelo systemd:


```
vim /etc/systemd/system/unifi.service
```



```
# Systemd unit file for UniFi Controller
 [Unit]
 Description=UniFi AP Web Controller
 After=syslog.target network.target
 #
 [Service]
 Type=simple
 User=ubnt
 WorkingDirectory=/opt/UniFi
 ExecStart=/usr/bin/java -Xmx1024M -jar /opt/UniFi/lib/ace.jar start
 ExecStop=/usr/bin/java -jar /opt/UniFi/lib/ace.jar stop
 SuccessExitStatus=143
 #
 [Install]
 WantedBy=multi-user.target
```


Após isso, salve o arquivo e vamos habilitar o serviço para inicializar junto ao sistema operacional, e também iremos iniciar o serviço:


```
systemctl enable unifi.service
systemctl start unifi.service
```


Feito, isso reinicie o server.


```
reboot
```


Depois do reboot acesse em seu navegador o <https://enderecoip:8443> e siga o passo de criação de conta.

Ao acessar o endereço pelo navegador, irá aparecer a mensagem abaixo, referente ao erro do certificado de segurança do site. Clique em **Avançado.**

![](/site/assets/img/uploads/2019/05/Instalando-Ubiquiti-Unifi-Controller-no-CentOS-7-1-1-1024x594.png)

Em seguida, clique em **Aceitar o risco e continuar.**

![](/site/assets/img/uploads/2019/05/Instalando-Ubiquiti-Unifi-Controller-no-CentOS-7-2-1024x593.png)

Selecione o pais e o timezone da sua região.

![](/site/assets/img/uploads/2019/05/Instalando-Ubiquiti-Unifi-Controller-no-CentOS-7-3-1024x594.png)

Na imagem abaixo, vai aparecer todos os dispositivos Ubiquiti que estão conectados em sua rede, nesse exemplo eu não conectei nenhuma dispositivo, por isso o mesmo está vazio 🙁

![](/site/assets/img/uploads/2019/05/Instalando-Ubiquiti-Unifi-Controller-no-CentOS-7-4-1024x594.png)

Na tela abaixo você pode fornecer o SSID e a chave de segurança da Rede Wifi já existente, ou pode pular a etapa também, clicando em **SKIP.**

![](/site/assets/img/uploads/2019/05/Instalando-Ubiquiti-Unifi-Controller-no-CentOS-7-5-1024x596.png)

O próximo passo é criar o login de acesso ao Controller, crie uma senha forte, e anote a senha em algum lugar antes de prosseguir o passo-a-passo.

![](/site/assets/img/uploads/2019/05/Instalando-Ubiquiti-Unifi-Controller-no-CentOS-7-6-1024x595.png)

Confirmando as configurações realizadas, clique em **FINISH.**

![](/site/assets/img/uploads/2019/05/Instalando-Ubiquiti-Unifi-Controller-no-CentOS-7-7-1024x594.png)

Caso você tenha acesso ao Cloud da Ubiquiti para gerenciar os dispositivos remotamente, insira seu e-mail/username e senha de acessos.

![](/site/assets/img/uploads/2019/05/Instalando-Ubiquiti-Unifi-Controller-no-CentOS-7-8.png)

Abaixo a tela de login do Contoller Ubiquiti Unifi.

![](/site/assets/img/uploads/2019/05/Instalando-Ubiquiti-Unifi-Controller-no-CentOS-7-9-1024x594.png)

Dashboard do Controller após o login. 🙂

![](/site/assets/img/uploads/2019/05/Instalando-Ubiquiti-Unifi-Controller-no-CentOS-7-10-1024x593.png)

No dashboard vai aparecer todas as opções de gerenciamento dos equipamentos Ubiquiti.  
Não estarei abordando nesse tutorial, apenas a instalação do Controller mesmo. 😉

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Até a próxima!

- - - - - -

![](/site/assets/img/uploads/2019/02/foto-redonda.png)  
**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>