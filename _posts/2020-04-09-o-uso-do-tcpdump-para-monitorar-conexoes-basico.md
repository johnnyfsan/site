---
layout: post
date: '2020-04-09T16:56:05-03:00'
title: 'O uso do TCPDUMP para monitorar conexões [básico]'
subtitle:
image: /assets/img/thumb/tcpdump-1.png
tag:
    - 'aprenda tcpdump'
    - 'como monitorar linux com tcpdump'
    - 'como usar tcpdump'
    - 'monitorar rede tcpdump'
    - tcpdump
    - 'tcpdump 21'
    - 'tcpdump 22'
    - 'tcpdump centos'
    - 'tcpdump debian'
    - 'tcpdump ftp'
    - 'tcpdump linux'
    - 'tcpdump ubuntu'
    - 'tcpump ssh'
comments: true
---
- - - - - -

Quando se trata em monitoramento de conexões o tcpdump é um forte aliado aos administradores de redes, no meu ponto de vista é um dos mais famosos ‘sniffers’ para Linux.  
Com o uso de tcpdump podemos realizar análises de redes e resolver grandes problemas. Ele mostra os cabeçalhos dos pacotes que foram capturados pela sua interface de rede.[![](/site/assets/img/uploads/2020/04/tcpdump-1.png)](/site/assets/img/uploads/2020/04/tcpdump-1.png)

Abaixo segue o link do Manual em Português do TCPDUMP:

<http://www.kernelhacking.com/rodrigo/docs/Tcpdump.pdf>

Vamos realizar o uso dessa ferramenta no Debian.  
IP: 10.106.0.200

Exemplos:

Primeiro vamos ver o que está passando pela nossa interface de rede:  
com o comando:

```
tcpdump -i eth0
```

[![](/site/assets/img/uploads/2020/04/tcpdump-2.png)](/site/assets/img/uploads/2020/04/tcpdump-2.png)

Note que ao executar o comando ele passa informações que não notamos em primeiro momento devido a velocidade em que os pacotes são analisados. Informações de como Hora, Interface, Mac-Address, etc.

Podemos monitorar conexões especificando um host de destino.  
Vamos monitorar as conexão que vem do IP 10.106.0.1 para o nosso servidor com IP 10.106.0.250.

O Comando fica assim:

```
tcpdump -i eth0 src 10.106.0.1
```

Onde ‘src’ é o parâmetro que diz qual a origem que está vindo os pacotes a serem analisados.

[![](/site/assets/img/uploads/2020/04/tcpdump-3.png)](/site/assets/img/uploads/2020/04/tcpdump-3.png)

Podemos também monitorar especificando um host de destino.

O comando abaixo mostra todo o tráfego do Servidor 10.106.0.250 passando pelo Gateway no caso 10.106.0.1.

```
tcpdump -i eth0 dst host 10.106.0.1
```

[![](/site/assets/img/uploads/2020/04/tcpdump-4.png)](/site/assets/img/uploads/2020/04/tcpdump-4.png)

Com o tcpdump também podemos especificar exceções.

Com o parâmetro ‘not host’, podemos ver todo os pacotes que se passam na interface, exceto o 10.106.0.1 por exemplo:

```
tcpdump -i eth0 not host 10.106.0.1
```

[![](/site/assets/img/uploads/2020/04/tcpdump-5.png)](/site/assets/img/uploads/2020/04/tcpdump-5.png)

Podemos especificar portas de origem e destino com os comandos ‘src porta’ e ‘dst port’. Vamos ver como monitorar o tráfego com destino a porta 80 (http).

Com o comando abaixo vamos analisar os pacotes com origem a porta 80 de algum servidor:

```
tcpdump -i eth0 dst port 80
```

[![](/site/assets/img/uploads/2020/04/tcpdump-6.png)](/site/assets/img/uploads/2020/04/tcpdump-6.png)

Agora vamos monitorar os pacotes com origem a porta 22 por exemplo, porta de comunicação do SSH:

Veja o comando abaixo:

```
tcpdump -i eth0 src port 22
```

[![](/site/assets/img/uploads/2020/04/tcpdump-9.png)](/site/assets/img/uploads/2020/04/tcpdump-9.png)

Agora em um terminal vamos realizar a conexão ssh ao nosso servidor para ver os pacotes com o comando acima.

[![](/site/assets/img/uploads/2020/04/tcpdump-10.png)](/site/assets/img/uploads/2020/04/tcpdump-10.png)

Só pelo fato de executar a conexão, a resposta do comando tcpdump foi o seguinte, ele já mostrou qual IP estava solicitando a conexão na porta 22, e mostrou também toda a parte de comunicação do protocolo TCP.

[![](/site/assets/img/uploads/2020/04/tcpdump-11.png)](/site/assets/img/uploads/2020/04/tcpdump-11.png)

Bom aqui vimos alguns exemplos do uso do tcpdump, podemos realizar fortes monitoramentos em diversos tipos de portas e protocolos.  
Para maiores informações veja o manual no link abaixo:

<http://www.kernelhacking.com/rodrigo/docs/Tcpdump.pdf>

Dúvidas, comentário e sugestões postem nos comentários… 👋🏼 Até a próxima!

- - - - - -

![](/site/assets/img/uploads/2019/02/foto-redonda.png)
**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>  