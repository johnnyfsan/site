---
title: 'Comando IFCONFIG no CentOS 7'
date: '2018-04-02'
image: ../assets/img/uploads/2018/04/comando-ifconfig-centos7.png
category:
    - 'CentOS'
    - 'Dicas de Linux'
    - 'Linux'
tag:
    - 'command ifconfig centos'
    - 'command ifconfig centos 7'
    - 'ifconfig centos'
    - 'ifconfig centos 7'
    - 'instalando comando ifconfig centos'
    - 'instalando comando ifconfig centos 7'

---

- - - - - -

Fala galera, beleza? 😎

Essa dica é para quem está iniciando no CentOS 7 e está acostumado a utilizar o comando ifconfig, presente nas versões anteriores da distribuição CentOS.

Ao realizar a instalação, notei que o comando IFCONFIG no CentOS 7 não estava disponível, encontrei uma certa dificuldade ao consultar o endereço IP atribuído para a interface de rede. Ao buscar pelo comando ifconfig e route o linux retornava com o seguinte erro:

![](../assets/img/uploads/2018/04/comando-ifconfif-centos-7-1.png)



A primeira coisa que fiz foi procurar nos repositórios oficiais do CentOS 7 o comando ifconfig, para ver o que ele me retornava.

Fazendo uma simples pesquisa com o comando yum search ifconfig obtive a informação do pacote net-tools.

![](../assets/img/uploads/2018/04/comando-ifconfif-centos-7-2.png)

```
yum -y install net-tools
```

![](../assets/img/uploads/2018/04/comando-ifconfif-centos-7-3.png)


Após a instalação já é possível fazer o uso do comando ifconfig e route.

![](../assets/img/uploads/2018/04/comando-ifconfif-centos-7-4.png)


Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Até a próxima!

- - - - - -

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -
