---
title: 'Servidor Web com Apache, PHP e MySQL no Ubuntu Linux'
date: '2017-12-21T15:24:33-02:00'
type: post
image: /assets/img/thumb/servidor-web-ubuntu-linux.png
tag:
    - 'apache ubuntu'
    - 'como criar um servidor apache'
    - 'como instalar servidor linux web'
    - 'dicas de linux'
    - 'dicas de ti'
    - 'dicas profissionais de linux'
    - 'dicas profissionais ti'
    - 'instalar servidor linux web'
    - 'instalar servidor web linux'
    - 'instalar servidor web ubuntu'
    - 'lamp server'
    - 'linux apache mysql php ubuntu'
    - 'mysql ubuntu'
    - 'servidor web'
    - 'servidor web linux'
    - 'servidor web ubuntu'
    - 'tutorial instalacao servidor web linux'
    - 'tutorial instalacao servidor web ubuntu'
    - 'ubuntu servidor web'
---

- - - - - -

Fala galera, beleza? 😎 Hoje vou abordar a instalação de um Servidor Web completo no Linux Ubuntu. aquele famoso servidor LAMP (Linux + Apache + MySQL + PHP). Com essa instalação que veremos abaixo, você estará com um ambiente linux web apto para instalar um WordPress, o PHPMyAdmin, Criar um sistema web com integração de PHP e o MySQL para armazenar as informações do sistema.

Se você não possui o Ubuntu instalado, ou não sabe como instalar, clica 👇🏼 aqui embaixo que temos um vídeo bem rápido de como fazer a instalação do Ubuntu no VirtualBox para você aprender cada vez mais trabalhar com sistemas Linux.  
👉🏼 Link do vídeo de instalação do Ubuntu no VirtualBox: <https://www.youtube.com/watch?v=HUvP8LvEaws>

Confira abaixo esse tutorial que é muito simples e objetivo.

#### Passo 1: Atualizando o Ubuntu

```
sudo apt-get update
```

![](/site/assets/img/uploads/2017/12/1.png)

#### Passo 2: Instalando e Configurando o Apache2

```
sudo apt-get install apache2
```

![](/site/assets/img/uploads/2017/12/2.png)

Após a instalação do Apache no Servidor Linux Ubuntu, vamos efetuar uma configuração para o que servidor não tenha erros na inicialização e no seu funcionamento.  
Precisamos definir o parâmetro “ServerName”, nesse parâmetro precisamos definir o nome do servidor que estamos utilizando ou o endereço IP.

Para fazer a edição do arquivo você pode utilizar o “nano” ou o “vim”, no meu caso utilizei o “vim”

Veja abaixo como instalar o “VIM”:

```
sudo apt-get install vim
```

![](/site/assets/img/uploads/2017/12/3.png)

Na sequência podemos alterar o arquivo “/etc/apache/apache2.conf” e inserir no final do arquivo o parâmetro “ServerName”.

```
sudo vim /etc/apache/apache2.conf
```

![](/site/assets/img/uploads/2017/12/4.png)

Adicione no final do arquivo (Se estiver utilizando o VIM, pressione as teclas: ESC + SHIFT + G para direto ao final do arquivo).

```
[...]
ServerName ubuntu
```

![](/site/assets/img/uploads/2017/12/5.png)

Salve o arquivo e saia da edição. (ESC + : + wq)

Agora vamos efetuar um teste no arquivo de configuração do Apache para verificar se o mesmo está correto.

```
sudo apache2ctl configtest
```

![](/site/assets/img/uploads/2017/12/6.png)

Em seguida vamos iniciar o serviço do Apache2.

```
sudo systemctl start apache2
```

![](/site/assets/img/uploads/2017/12/7.png)

Para verificar se o serviço do Apache2 encontra-se operando corretamente digite:

```
sudo systemctl status apache2
```

![](/site/assets/img/uploads/2017/12/8.png)

Agora abra o navegador e digite http://127.0.0.1 ou o endereço do Host que você está instalando o serviço.

![](/site/assets/img/uploads/2017/12/9.png)

#### Passo 3: Instalando o MySQL

Agora vamos continuar com a instalação do nosso famoso servidor LAMP, dando sequência no serviço de banco de dados, com o MySQL Server.

```
sudo apt-get install mysql-server
```

![](/site/assets/img/uploads/2017/12/10.png)

Após confirmar a instalação do MySQL, será solicitado que você digite a senha do usuário “root” para utilizar dentro do MySQL.

![](/site/assets/img/uploads/2017/12/11.png)

Repita a senha digitado anteriormente.

![](/site/assets/img/uploads/2017/12/12.png)

#### Passo 4: Instalando o PHP

```
sudo apt-get install php libapache2-mod-php php-mcrypt php-mysql
```

![](/site/assets/img/uploads/2017/12/13.png)

Após realizarmos a instalação do Apache2, vamos criar o arquivo de informações do PHP, para que possamos ver a versão instalada, os módulos ativos, etc.

Crie um arquivo no diretório Web do Apache2. ( “/var/www/html/info.php”)

```
sudo vim /var/www/html/info.php
```

![](/site/assets/img/uploads/2017/12/14.png)

E insira dentro do arquivo o seguinte conteúdo:

```
<?php
     phpinfo();
?>
```

![](/site/assets/img/uploads/2017/12/15.png)

Agora que terminamos nossa configuração e instalação dos pacotes para o Servidor Web, vamos reiniciar novamente o serviço do Apache, para que as configurações do PHP entrem em vigor.

```
sudo systemctl restart apache2
```

![](/site/assets/img/uploads/2017/12/16.png)

Em seguida podemos digitar no navegador o arquivo que criamos sobre as informações do PHP, digite http://127.0.0.1/info.php.

![](/site/assets/img/uploads/2017/12/17.png)

Pronto, agora você já possui um servidor web em Linux Ubuntu para implementar seus projetos, hospedar suas páginas web. 😎

Dúvidas, comentário e sugestões postem nos comentários.
👋🏼 Até a próxima!

- - - - - -

![](/site/assets/img/uploads/2017/11/foto-perfil-redondo-johnny.png)  

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -