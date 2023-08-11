---
title: 'Criando um Servidor Web com Docker e Nginx'
date: '2023-03-12T16:09:56-03:00'
image: ../assets/img/uploads/2023/03/docker-nginx.png
category:
    - 'DevOps'
    - 'Docker'
    - 'Nginx'
tag:
    - 'Ambiente de Desenvolvimento'
    - 'Container'
    - 'criar dockerfile nginx'
    - 'criar servidor web nginx docker'
    - 'criar webserver docker e nginx'
    - 'Desenvolvimento de Aplicativos'
    - 'docker webserver dockerfile nginx'
    - 'docker webserver nginx'
    - 'Dockerfile'
    - 'dockerfile nginx'
    - 'Escalabilidade'
    - 'Imagem Docker'
    - 'Nginx'
    - 'Portabilidade'
    - 'servidor web'
---

- - - - - -

O Docker é uma ferramenta de virtualização de containers que permite a criação de ambientes isolados, leves e portáteis para o desenvolvimento e execução de aplicativos. Um dos principais benefícios do Docker é a facilidade em criar imagens de containers personalizadas, utilizando o Dockerfile. Neste artigo, vamos criar um Dockerfile para um servidor web com Nginx.


### Passo 1: Escolhendo a imagem base

O primeiro passo na criação de um Dockerfile é escolher uma imagem base que possa ser usada como ponto de partida. No nosso caso, vamos escolher a imagem base `nginx` do Docker Hub, que já possui o servidor web Nginx instalado.


### Passo 2: Definindo o ambiente

Agora que temos a imagem base escolhida, vamos definir o ambiente que será utilizado no container. Neste exemplo, vamos definir uma variável de ambiente `ENV` com o nome `APP_HOME` e o valor `/var/www/html`. Este valor será utilizado para definir o diretório raiz do servidor web.


```bash
ENV APP_HOME /var/www/html
```

### Passo 3: Copiando o código fonte

O próximo passo é copiar o código fonte do nosso servidor web para dentro do container. Para isso, vamos utilizar a instrução `COPY`. No exemplo abaixo, estamos copiando o conteúdo da pasta `./app` para dentro do diretório `APP_HOME` do container.


```bash
COPY ./app ${APP_HOME}
```

### Passo 4: Configurando o servidor web

Agora que o código fonte foi copiado para dentro do container, vamos configurar o servidor web Nginx para que ele possa servir o nosso aplicativo. Para isso, vamos criar um arquivo de configuração `nginx.conf` e copiá-lo para dentro do container.

O arquivo `nginx.conf` deve conter as configurações necessárias para que o Nginx possa servir o nosso aplicativo. No exemplo abaixo, estamos configurando o Nginx para servir arquivos estáticos e encaminhar todas as outras requisições para o arquivo `index.html`.


```bash
server {
    listen       80;
    server_name  localhost;

    root   ${APP_HOME};
    index  index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /assets/ {
        alias ${APP_HOME}/assets/;
    }
}
```

### Passo 5: Expondo a porta

O próximo passo é expor a porta do servidor web para que possa ser acessada a partir do host. Para isso, vamos utilizar a instrução `EXPOSE`. No exemplo abaixo, estamos expondo a porta `80` do container.


```bash
EXPOSE 80
```

### Passo 6: Iniciando o servidor web

Por fim, vamos definir o comando que será executado quando o container for iniciado. Neste caso, vamos utilizar o comando `nginx`, que inicia o servidor web Nginx.


```bash
CMD ["nginx", "-g", "daemon off;"]
```

### Dockerfile completo

Abaixo está o Dockerfile completo para um servidor web com Nginx.

```bash
FROM nginx

ENV APP_HOME /var/www/html

COPY ./app ${APP_HOME}

COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```


### Construindo a imagem

Para construir a imagem, basta executar o seguinte comando na pasta onde o Dockerfile foi criado:

```bash
docker build -t meu-servidor-web-nginx .
```

Este comando irá criar uma imagem com o nome `meu-servidor-web-nginx` a partir do Dockerfile presente na pasta atual.

### Executando o container

Uma vez que a imagem foi criada, podemos executar o container utilizando o seguinte comando:

```bash
docker run -p 8080:80 meu-servidor-web-nginx
```

Este comando irá executar o container a partir da imagem `meu-servidor-web-nginx` e irá expor a porta `80` do container para a porta `8080` do host.

### Conclusão

Neste artigo, vimos como criar um Dockerfile para um servidor web com Nginx. Utilizamos a imagem base `nginx` do Docker Hub e configuramos o servidor web para servir o nosso aplicativo. Com o Docker, podemos criar imagens personalizadas de containers com facilidade, garantindo ambientes isolados, portáteis e escaláveis para o desenvolvimento e execução de aplicativos.


Dúvidas, comentário e sugestões postem nos comentários.
👋🏼 Até a próxima!

- - - - - -


**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>
