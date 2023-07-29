---
title: 'Conhecendo o Docker - Parte #1'
date: '2017-12-29T15:35:42-02:00'
type: post
image: /assets/img/uploads/2017/12/conhecendo-docker-parte-1.png
share-img: /assets/img/uploads/2017/12/conhecendo-docker-parte-1.png
tag:
    - 'começando com docker'
    - 'conhecendo o docker'
    - 'conhecer docker'
    - 'dicas devops'
    - 'dicas docker'
    - 'dicas linux'
    - 'docker'
    - 'docker e containers'
    - 'docker para iniciantes'
    - 'implementando docker'
    - 'infraestrutura agil'
    - 'iniciando com docker'
    - 'instalando docker'
    - 'linux containers'
---

- - - - - -

Fala galera, beleza? 😎  
Hoje vamos conhecer um pouco sobre Docker 🐳 e como essa nova tecnologia vem para revolucionar o mercado de TI, trazendo mais agilidade e segurança nas operações de infraestrutura.

### O que é Docker? 🐳

Antes de começarmos a falarmos sobre Docker, vamos definir o conceito sobre ele.  
**Docker** não é um sistema de **virtualização tradicional**, podemos dizer que o Docker opera de uma forma mais inteligente que os sistemas de virtualização tradicionais que conhecemos.

Ambientes de virtualização tradicionais necessitam de um sistema operacional completo e isolado para seu funcionamento, já no Docker temos recursos isolados utilizando bibliotecas do kernel em comum, ou seja, utilizando o mesmo kernel do Host e no container.  
Tudo isso é possível, pois o Docker utiliza como backend o famoso LXC (Linux Containers).

Veja na imagem abaixo como é o funcionamento de um sistema de virtualização e do Docker.

![](/assets/img/uploads/2017/12/explicando-conceito-docker-tidahora.png)  
Imagem criada por Johnny Ferreira – Créditos: Johnny Ferreira

Notem que nas maquinas virtuais temos uma camada a mais, que é o sistema operacional das maquinas virtuais, chamados normalmente de Guest OS, sendo instalados no Hypervisor, ocupando memória, processamento, disco e rede. Tudo isso para que possamos instalar nossos servidores web e rodarmos o apache/php com mysql.

Agora reparem na opção de containers do Docker, o mesmo elimina a camada do Guest OS e compartilha o Kernel de um servidor Linux, reduzindo o uso de memória, disco, processamento e rede. Rodando de forma mais simplificada e customizada uma imagem com o mesmo apache/php e mysql entre outras aplicações.

Você pode instalar o Docker em Linux (Kernel 3.8 ou superior), Windows ou Mac.

Abaixo temos o link oficial do Docker sobre instalação em diversos sistemas operacionais.  
[Instalando Docker no Microsoft Windows](https://docs.docker.com/docker-for-windows/)  
[Instalando Docker no Mac OS X](https://docs.docker.com/docker-for-mac/)  
[Instalando Docker no Linux Ubuntu](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/)  
[Instalando Docker no Linux Debian](https://docs.docker.com/engine/installation/linux/docker-ce/debian/)  
[Instalando Docker no Linux CentOS](https://docs.docker.com/engine/installation/linux/docker-ce/centos/)  
[Instalando Docker no Linux Fedora](https://docs.docker.com/engine/installation/linux/docker-ce/fedora/)

### Começando com o Docker

Nosso ambiente aqui no [TI da Hora](http://www.tidahora.com.br) para realizar esse tutorial é uma maquina com Ubuntu 16.04.  
Se você deseja instalar o Ubuntu e não sabe por onde começar, confira o vídeo de instalação clicando [aqui.](https://www.youtube.com/watch?v=HUvP8LvEaws)

O kernel para o Docker precisa ser superior ao kernel 3.8.  
Verificando o kernel no Linux.

```
uname -r
```

![](/assets/img/uploads/2017/12/conhecendo-docker-1.png)

### Instalando o Docker

Essa instalação pode ser efetuada em qualquer sistema Linux, você pode executar o comando abaixo em ambientes Red Hat, CentOS, Debian, etc.

Verifique se possui o ‘curl’ instalado no seu sistema, caso não tenha efetue a instalação do mesmo.

Instalando o Docker, o comando abaixo realiza a instalação do docker no seu ambiente, seja ele um Ubuntu, Debian ou CentOS.

```
curl -sSL https://get.docker.com | sh
```

![](/assets/img/uploads/2017/12/conhecendo-docker-2.1.png)

Mudando para ‘root’:

```
sudo su -
```

![](/assets/img/uploads/2017/12/conhecendo-docker-2.png)

Verificando a versão do Docker:

```
docker -v
```

![](/assets/img/uploads/2017/12/conhecendo-docker-3.png)

Verifique se após a instalação do Docker o mesmo encontra-se em execução no sistema:

```
ps -ef |grep docker
```

![](/assets/img/uploads/2017/12/conhecendo-docker-2.2.png)

### Comandos básicos no Docker

#### Listando os processos em execução:

```
docker ps
```

![](/assets/img/uploads/2017/12/conhecendo-docker-3.1.png)

Note que não temos nenhum processo rodando ainda, vamos fazer isso mais pra frente.

#### Listando as Imagens:

```
docker images
```

![](/assets/img/uploads/2017/12/conhecendo-docker-3.2.png)

#### Criando seu Primeiro Container no Docker

Nosso primeiro container será um CentOS 6.9, vamos efetuar a instalação de um ambiente web no mesmo, contendo Apache, PHP e MySQL.

Você pode escolher sua imagem no site oficial do Docker: <https://hub.docker.com/>

Criando o container com CentOS 6.9

```
docker run -it centos:6.9 /bin/bash
```

![](/assets/img/uploads/2017/12/conhecendo-docker-4.png)

*Parâmetros:*  
*-i = interativo*  
*-t = terminal*

Reparem que o Docker já fez a instalação do sistema que pedimos e na sequência já nos jogou direto ao prompt de comandos do container com CentOS 6.9.

Verifique o ambiente desse container, com o comando abaixo:

```
cat /etc/issue
```

![](/assets/img/uploads/2017/12/conhecendo-docker-4.1-1.png)

Atualizando o CentOS 6.9

```
yum -y update && yum clean all
```

***Observação:** O **yum clean all** limpa os pacotes baixados no update.*

![](/assets/img/uploads/2017/12/conhecendo-docker-4.2.png)

### Saindo do Container e Voltando ao Host Principal

Para que possamos sair do container sem encerra-lo, precisamos digitar no teclado as seguintes sequência no teclado: *ctrl + p + q*

![](/assets/img/uploads/2017/12/conhecendo-docker-saindo.jpg)  
Teclado Apple

No Host principal digite novamente o comando “docker ps”:

```
docker ps
```

![](/assets/img/uploads/2017/12/conhecendo-docker-4.5.png)

Vejam que temos nosso Container rodando CentOS 6.9 que acabamos de subir, bem como as informações de qual comando estamos executando, ou seja, o /bin/bash e quanto tempo estamos com ela rodando.

### Voltando ao Console do Container

Para voltarmos ao console do Container, precisamos pegar o **Container ID,** aquele que está listado na imagem acima, gerada pelo comando “docker ps”:

Vamos utilizar o comando abaixo para voltar ao console do container:

```
docker attach 020b238d63e7
```

![](/assets/img/uploads/2017/12/conhecendo-docker-4.6.png)

E na sequência já ganhamos acesso ao console. 🐧

### Instalando Serviços no Container

Certo, agora vamos instalar o Apache, PHP e MySQL em nosso container rodando CentOS 6.9.

```
yum -y install httpd php mysql-server php-mysql
```

![](/assets/img/uploads/2017/12/conhecendo-docker-4.3.png)

Iniciando os serviços:

```
/etc/init.d/httpd start
/etc/init.d/mysqld start
```

Verificando o IP do Container:

```
ifconfig
```

![](/assets/img/uploads/2017/12/conhecendo-docker-4.4.png)

Observem que temos um IP diferente da nossa rede local, isso porque o Docker cria um pool de endereço gerenciados pelo próprio docker, ou seja, para podermos acessar nossa aplicação que está rodando ai nesse container, é necessário que na hora da criação do container, passarmos um parâmetro de portas na hora da criação do container.

Saia do container e vamos refaze-lo da maneira com que possamos acessar nossa aplicação web através dos parâmetros corretos.

*Para sair do container utilize a sequência do teclado: ctrl + p + q*

### Parando um Container

Agora precisamos parar nosso container, para que possamos subi-lo da maneira correta.

```
docker stop CONTAINER-ID
```

ou seja

```
docker stop 020b238d63e7
```

Verificando se a imagem foi encerrada com sucesso:

```
docker ps
```

![](/assets/img/uploads/2017/12/conhecendo-docker-4.7.png)

Notem que o container não se encontra mais rodando em nosso docker.

### Subindo um container e encaminhando portas ao host principal

Para que possamos inicialmente visualizar nosso diretório web instalado em um container é necessário direcionarmos uma porta do host principal a uma porta do serviço do container.

Porta 88 do Host Principal -&gt; Porta 80 do Container

```
docker run -it -p 88:80 centos:6.9 /bin/bash
```

![](/assets/img/uploads/2017/12/conhecendo-docker-4.8.png)

Agora vamos instalar o Apache no novo container.

Basicamente o mesmo processo que realizamos anteriormente sem a configuração de porta no Container.

```
yum -y update && yum clean all
```

```
yum -y install httpd
```

Iniciando os serviços instalados:

```
/etc/init.d/httpd start
```

Verificando os processos rodando no Container.

```
ps -ef
```

![](/assets/img/uploads/2017/12/conhecendo-docker-4.9.png)  

Reparem que os processos que estão rodando é apenas do bash e do apache, ou seja, o Docker nos da uma possibilidade de redução muito grande nas performances de servidores.

Agora vamos saindo do Container, sem encerra-lo, (ctrl + p + q) e vamos visualizar os processos do Docker no Host principal.

Reparem na marcação abaixo a configuração do parâmetro (-p) que passamos no comando na hora da criação do container, esse parâmetro irá nos permitir acessar o Container através da porta 88 do Host principal.

![](/assets/img/uploads/2017/12/conhecendo-docker-5.png)  

Pegando o IP do Host Principal (Docker)

```
ip addr show
```

Você pode filtrar a sua placa de rede, no meu caso minha placa de rede é “enp0s3”

```
ip show addr |grep enp0s3
```

![](/assets/img/uploads/2017/12/conhecendo-docker-5.1.png)  

Agora vamos acessar esse endereço IP da seguinte forma em nosso browser: http://iphost:88

![](/assets/img/uploads/2017/12/conhecendo-docker-5.2.png)  

Página Inicial do Apache2 no CentOS 6.9

Acessamos o nosso servidor Web hospedado no container que subimos, legal não? 😎

Essa foi uma breve introdução sobre o Docker e de como ele poder ser bem útil em diversos ambientes de infraestrutura de TI.

Espero ter lhe auxiliado de alguma forma, trazendo um pouco de conhecimento sobre Docker e mostrando um pouco do que as grandes corporações como Google, Facebook estão utilizando em suas plataformas.

Em breve **[Conhecendo o Docker – Parte #2](http://tidahora.com.br/conhecendo-o-docker-parte-2/).**

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Até a próxima!

- - - - - -

![](/assets/img/uploads/2017/11/foto-perfil-redondo-johnny.png)  

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -