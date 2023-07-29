---
title: 'Conhecendo o Docker – Parte #2'
date: '2018-01-26T10:51:56-02:00'
type: post
thumbnail-img: /assets/img/uploads/2018/01/conhecendo-docker-parte-2.png
share-img: /assets/img/uploads/2018/01/conhecendo-docker-parte-2.png
tag:
    - 'comandos docker'
    - 'começando com docker'
    - 'comecar com docker'
    - 'conhecendo o docker'
    - 'conhecer docker'
    - 'conhecer o docker'
    - 'devops'
    - 'dicas devops'
    - 'dicas docker'
    - 'dicas docker dicas de linux'
    - 'dicas linux'
    - 'docker'
    - 'docker brasil'
    - 'docker devops'
    - 'docker e containers'
    - 'docker iniciante'
    - 'docker linux'
    - 'docker para iniciantes'
    - 'implementando docker'
    - 'infraestrutura agil'
    - 'iniciando com docker'
    - 'instalando docker'
    - 'linux containers'
    - 'Sem categoria'
---

- - - - - -

Fala galera, beleza? 👍🏼  
Dando continuidade na série de posts sobre o Docker , vamos para a parte 2, onde vamos conhecer um pouco mais sobre essa incrível e poderosa solução.

Na parte 2 iremos ver um pouco do gerenciamento dos container, como limitar a memória, realizar auditoria no container, executar comandos remotamente, e muito mais.

No post anterior vimos a diferença entre maquina virtual e container, vimos também a instalação e como subir um container do zero.  
Se você é novo por aqui e ainda não viu o post anterior, que é parte #1 clica👉🏼 [aqui](http://tidahora.com.br/conhecendo-o-docker-parte-1/) , é 👉🏼 [aqui](http://tidahora.com.br/conhecendo-o-docker-parte-1/) mesmo! 🐳 👀

### Subindo um Container 🐳

O primeiro passo é subir um container, vamos subir um container com a imagem do CentOS 6.9.

```
sudo docker run -it centos:6.9 /bin/bash
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-1.png)

Vamos também verificar a versão do container criado.

```
cat /etc/issue
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-2.png)

### Verificando Alterações no Container 🐳

Um dos parâmetros mais útil do docker, é o “diff”, com ele é possível verificar alterações realizadas nos containers.  
Vamos sair do nosso container, lembre-se, para sair do container utilize as teclas “ctrl”+”p”+”q”:

![](/assets/img/uploads/2017/12/conhecendo-docker-saindo.jpg)

Pronto, voltamos ao Host e vamos verificar se nosso container está em execução:

```
sudo docker ps
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-3.png)

Isso indica que saimos do container da maneira correta 🙂

Agora vamos executar o comando abaixo para verificar as alterações que fizemos até o momento em nosso container.

```
sudo docker diff container-id
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-4.png)

Nenhuma alteração, vamos voltar ao console do container e instalar o pacote do “vim” por exemplo.

```
sudo docker attach container-id
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-6.png)

Agora vamos instalar o “vim” e na sequência limpamos o cache do yum.

```
yum -y install vim && yum clean all
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-7.png)

Voltamos ao console principal do Docker, pressione as teclas “ctrl”+ “p” + “q”.

E vamos executar novamente o comando com o parâmetro “diff”.

```
sudo docker diff container-id
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-8.png)

Reparem na imagem acima, que temos vários arquivos que foram adicionados ou alterados em nosso container.  
O parâmetro “diff” é muito útil quando precisamos analisar certas alterações nos containers.

### Executando comandos no Container pelo Host Principal

No docker temos um parâmetro muito interessante que nos possibilita executar comandos no container sem precisar conectar ao console do container, muito útil quando queremos ver um determinado processo rodando, ou executar alguma rotina.  
Esse parámetro é o “exec”, veja abaixo como podemos utiliza-lo.

Pegue o container-id do container desejado.

```
sudo docker ps
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-9.png)

Verificando os processos rodando no container.

```
sudo docker exec container-id ps -ef
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-10.png)

Podemos também visualizar arquivos.

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-11.png)

### Inspecionando o Container

Com o parâmetro “inspect” é possível verificarmos todas as configurações de um container, informações sobre configurações de rede, volumes, memória, cpu, data de criação do container, status e etc.

```
sudo docker inspect container-id
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-12.png)

Podemos utilizar o ” | grep ” para filtrar por alguma configuração em especifico, abaixo estarei filtrando o IP do container.

```
sudo docker inspect container-id | grep IPAddress
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-13.png)

### Verificando o Processamento e Desempenho do Container

Para ver como anda o processamento, memória e desempenho de um container, utilizamos o parâmetro “stats”.

```
sudo docker stats container-id
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-14.png)![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-15.png)

O consumo do container acima é bem baixo pois o mesmo foi levantando para criarmos esse tutorial, mas é interessante analisar um container em produção, rodando com serviços de alta performance por exemplo.

### Definindo Limite de Memória ao Container

Para limitar a memória de um container, ou seja, configurar uma memória fixa a um determinado container é necessário que o parâmetro “-m” seja passado logo na criação do container.

Por exemplo, abaixo criamos um container com limite de 512Mb com Ubuntu.

```
sudo docker run -it -m 512M debian /bin/bash
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-16.png)

Agora voltamos ao console principal e utilizamos um comando que vimos anteriormente, que é o parâmetro “stats”

Para voltar ao console do Host Principal utilize “ctrl” + “p” + “q”

```
sudo docker stats container-id
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-17.png) ![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-18.png)

### Reiniciando um Container

Se você instalou um serviço ou um pacote no seu container, e por algum motivo quer reiniciar o mesmo, saiba que o parâmetro “stop” e “start” não vai te ajudar nesse sentido. Para fazer um reboot no container, temos o parâmetro “restart”, veja abaixo:

```
sudo docker restart container-id
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-19-1.png)

E podemos utilizar o parâmetro “exec” que vimos hoje para verificar o “uptime” do container.

```
sudo docker exec container-id uptime
```

![](/assets/img/uploads/2018/01/conhecendo-o-docker-parte2-20.png)

Espero ter contribuído de alguma forma para passar um pouco de informação pra quem está começando a entender o mundo DevOps e Docker. Existem diversos sites na internet com conteúdo muito mais avançado que esse que preparei, porém quis abordar assunto para o pessoal que está iniciando a entender toda essa nova metodologia.

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Valeu! e até a próxima!

- - - - - -

![](/assets/img/uploads/2017/11/foto-perfil-redondo-johnny.png)

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -