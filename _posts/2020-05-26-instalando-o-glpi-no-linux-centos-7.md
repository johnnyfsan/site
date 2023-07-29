---
layout: post
date: '2020-05-26T16:58:51-03:00'
title: Instalando o GLPI no Linux CentOS 7
subtitle:
image: /assets/img/uploads/2020/05/beneficios-GLPI.png
share-img: /assets/img/uploads/2020/05/beneficios-GLPI.png
tags: [como instalar glpi, instalar glpi no centos, instalar glpi no centos 7, instalar glpi no linux, ]
comments: true
---
- - - - - -



#### Conteúdo de instalação do GLPI com base no Linux CentOS 7

O GLPI é um gerenciamento de ativos de TI de código aberto, sistema de rastreamento de problemas e sistema de service desk. Este software é escrito em PHP e distribuído sob a GNU General Public License. Como uma tecnologia de código aberto, qualquer pessoa pode executar, modificar ou desenvolver o código. [Wikipedia (inglês)](https://en.wikipedia.org/wiki/GLPi)

[![]( /assets/img/uploads/2020/05/GLPI-banner-Athena-Security.png)]( /assets/img/uploads/2020/05/GLPI-banner-Athena-Security.png)

Site oficial: <https://glpi-project.org/pt-br/>

#### Preparando o Ambiente do Sistema Operacional

O primeiro passo para obter sucesso em qualquer instalação de aplicação ou serviço no Linux, é manter o ambiente do sistema operacional atualizado.

E atualizar o sistema operacional no Linux é muito fácil e rápido, obviamente você vai precisar de um link de internet de boa qualidade.

Execute o comando abaixo para atualizar o CentOS.


```
yum -y update
```


Aguarde a finalização do update acima.

#### Instalando o Repositório Epel no Linux CentOS 7

O repositório EPEL fornece uma variedade de pacotes ao Linux CentOS, por esse motivo estaremos instalando e ativando o repositório em nosso CentOS, para que possamos baixar pacotes sem dificuldades.


```
yum -y install epel-release
```


Agora vamos limpar o cache do yum e solicitar a atualização novamente.

```
yum clean all & yum -y update
```

#### Instalando o Repositório REMI no Linux CentOS 7

Assim como o repositório EPEL, o repositório REMI fornece uma variedade de pacotes para que possamos baixar e utilizar em nossas aplicações de servidores Linux. O REMI é popular por hospedar uma variedade de pacotes de PHP, sempre que preciso atualizar alguma aplicação em PHP ou web, o remi é o primeiro repositório que vem a minha mente para executar tal necessidade.

Utilize o comando abaixo para instalar o REMI


```
yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
```


Em seguida, instale o pacote yum-utils:


```
yum -y install yum-utils
```


#### Instalando pacotes básicos para Administração do Servidor

O próximo passo consiste em instalar alguns pacotes necessários para que tenhamos a administração do Linux de uma maneira mais efetiva.

**Obs.:** *Estarei instalando o “vim” para edição de arquivos, mas se você preferir utilizar outro de sua preferência fique a vontade para mudar a instalação.*


```
yum -y install vim htop wget telnet tcpdump
```


Eu realizo a instalação dos pacotes acima para facilitar a administração e uma possível resolução de problemas no servidor Linux, então por padrão, sempre realizo a instalação dos pacotes no comando acima praticamente em todos os servers que instalo.

#### Instalando o MariaDB (Bando de Dados)

O MariaDB é o serviço responsável pelo banco de dados local que iremos utilizar para hospedar nossa aplicação do GLPI.

Vamos criar o repositório para que possamos instalar o MariaDB na versão correta para o nosso GLPI. (&gt;10.2)

Crie o arquivo MariaDB.repo no diretório /etc/yum.repos.d/:


```
vim /etc/yum.repos.d/MariaDB.repo
```


Copie e cole o conteúdo da estrutura abaixo:

```
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.2/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```

Salve o arquivo, (*ESC* + *:wq* no vim)

Agora vamos instalar o MariaDB na versão 10.2:


```
yum -y install mariadb-server
```


Ajustando o MariaDB para iniciar junto ao boot do sistema operacional:


```
systemctl enable mariadb
```


Iniciando o MariaDB:


```
systemctl start mariadb
```


Agora vamos executar o script de configuração inicial do MariaDB, digite o comando abaixo no seu prompt de comandos do servidor Linux CentOS 7:


```
mysql_secure_installation 
```


Conteúdo de saída do comando *mysql\_secure\_installation*:

```
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] Y
New password: 
Re-enter new password: 
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] Y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] Y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] Y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] Y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

#### Criando a base de dados do GLPI para a instalação

Para que tenhamos sucesso na instalação do GLPI, vamos criar a base de dados e efetuar o a criação de um usuário para acesso a essa base, que estaremos utilizando na instalação do GLPI via web.

Conecte-se ao console do bando, utilizando o comando abaixo:


```
mysql -u root -psenha
```


Criando a base de dados chamada “glpi”:


```
CREATE DATABASE glpi;
```


Criando o usuário “glpi” com a senha “senhadodbglpi” (altere conforme sua necessidade).


```
CREATE USER 'glpi'@'%' IDENTIFIED BY 'senhadodbglpi';
```


Executando a permissão ao usuário “glpi” ter acesso a base “glpi”:


```
GRANT ALL PRIVILEGES ON `glpi`.* TO 'glpi'@'%';
FLUSH PRIVILEGES;
```


Saia do console do MariaDB:


```
exit
```


Agora vamos efetuar um teste de conexão com o usuário “glpi”:


```
mysql -u glpi -psenhadodbglpi
```


Execute o comando abaixo, para visualizar as bases de dados:


```
SHOW DATABASES;
```


Note a saída do comando acima se apareceu a base de dados para o usuário.

O Comando nos mostra a base de dados “glpi”, que criamos anteriormente, se você pode visualizar o mesmo, tudo certo até o momento.

#### Instalando o GLPI no Linux CentOS 7

Agora que já efetuamos a instalação de repositórios, criação e configuração do banco de dados, vamos dar andamento na instalação do que realmente importa, a aplicação GLPI no Linux CentOS 7.

Para começarmos, execute os comandos abaixo para habilitarmos os repositórios referente a php na versão 7.3 e do GLPI na versão 9.4.


```
yum-config-manager --enable remi-php73
yum-config-manager --enable remi
yum-config-manager --enable remi-glpi94
```


Agora os comandos abaixo para instalar os pacotes de Apache, PHP e GLPI:


```
yum -y install httpd php php-opcache php-apcu php-pear-CAS glpi
```


Ajustando o Apache para iniciar junto ao boot do sistema operacional:


```
systemctl enable httpd.service
```


Iniciando o Apache:


```
systemctl start httpd.service
```


#### Ajustando o FirewallD e o SELinux para permitir o funcionamento do GLPI

Esse passo consiste em executar a liberação das portas de funcionamento do protocolo http no FirewallD, serviço responsável pelo Firewall do servidor Linux CentOS 7.

Para permitir o funcionamento correto dos pacotes na porta 80 e 443, execute os comandos abaixo:


```
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --zone=public --add-service=https --permanent
```


Recarregue a configuração do FirewallD, para que as alterações tenham validade.


```
firewall-cmd --reload
```


Agora o próximo ajuste, é no SELinux, vamos permitir que o serviço httpd funcione adequadamente, execute os comandos abaixo:


```
setsebool -P httpd_can_network_connect on
setsebool -P httpd_can_network_connect_db on
setsebool -P httpd_can_sendmail on
```


#### Configurando o GLPI no Apache

Para que possamos continuar a instalação de forma segura do GLPI, vamos precisar ajustar o arquivo de configuração do glpi dentro do apache.

Abra o arquivo abaixo, com seu editor de texto:


```
vim /etc/httpd/conf.d/glpi.conf
```


Procure pela linha 29 do arquivo, referente ao módulo abaixo:

```
<IfModule mod_authz_core.c>
    # Apache 2.4
    Require local
    # Require ip x.x.x.x
</IfModule>
```

Note, onde temos uma linha comentada referente a Require IP, vamos inserir o endereço do computador ao qual estamos realizando a instalação, ou seja, no meu caso eu vou inserir o IP 10.1.3.15, que é o meu IP, do meu computador, não esqueça de mudar para o seu endereço IP, do seu computador.

 ```
<IfModule mod_authz_core.c> 
    # Apache 2.4 
    Require local 
    Require ip 10.1.3.15 
</IfModule>
```

Depois de inserir seu IP, salve o arquivo, e reinicie o Apache:


```
systemctl restart httpd.service
```


#### Instalando o GLPI pela Interface Web

Agora que já efetuamos todos os ajustes e configurações necessários, vamos seguir com a instalação do GLPI pela interface WEB.

Então, pelo endereço IP do seu servidor Linux vamos digitar em nosso navegador o seguinte: http://endereco\_ip/glpi/

![]( /assets/img/uploads/2020/05/image.png) 

URL de instalação do GLPI via Internet WEB  Selecione o idioma desejado, por aqui será o Português do Brasil:

![]( /assets/img/uploads/2020/05/image-1.png) 

Aceite os termos da licença do software:

![]( /assets/img/uploads/2020/05/image-2.png) 

Agora vamos selecionar a opção **Instalar**:

![]( /assets/img/uploads/2020/05/image-3.png) 

O SETUP inicial do GLPI irá realizar um teste, verificando se todos os pacotes e configurações necessárias estão ok, como já fizemos tudo nos passos anteriores, teremos sucesso em todos os itens, pelo menos na versão 9.4 do glpi.

![]( /assets/img/uploads/2020/05/image-4.png) 

Informando os dados do Banco de Dados, lembrando que fizemos um usuário exclusivo para o GLPI.

![]( /assets/img/uploads/2020/05/image-6.png) 

Agora precisamos selecionar o banco de dados, o que criamos e ajustamos as permissões é o DB glpi:

![]( /assets/img/uploads/2020/05/image-7.png) 

Em seguida, o instalado do GLPI nos retorna com uma mensagem de sucesso na criação do DB:

![]( /assets/img/uploads/2020/05/image-8.png) 

A próxima tela nos pede se desejamos enviar estatísticas de uso da ferramenta, não é obrigatório, no meu caso eu deixei desmarcado está opção:

![]( /assets/img/uploads/2020/05/image-9.png) 

Em seguida será questionado se você deseja doar algum valor a equipe de desenvolvimento do GLPI ou se deseja continuar a instalação é só clicar em Continuar.

![]( /assets/img/uploads/2020/05/image-10.png) 

Mensagem de conclusão de instalação do GLPI, será informando os usuários com senha default para acesso a ferramenta:

![]( /assets/img/uploads/2020/05/image-11.png) 

Tela de Login:

![]( /assets/img/uploads/2020/05/image-12.png)   

Informe o Login “glpi” e senha “glpi” para obter o primeiro acesso a plataforma de chamados, e começar os ajustes conforme sua necessidade.

![]( /assets/img/uploads/2020/05/image-13.png)   

Tela principal pós login:

![]( /assets/img/uploads/2020/05/image-14-1024x403.png)   


#### Removendo o diretório de instalação do GLPI pós instalação

Acesse o diretório abaixo:


```
cd /usr/share/glpi/
```


E vamos remover o diretório “install”, execute:


```
rm -rf install/
```


Feito, instalação da plataforma de chamados e inventário GLPI realizada com sucesso.

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Até a próxima!

- - - - - -

![]( /assets/img/uploads/2019/02/foto-redonda.png)   **Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>