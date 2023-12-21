---
title: 'Bash Personalizado Colorido no CentOS'
date: '2017-12-05'
image: ../assets/img/uploads/2017/11/dicas-de-linux.jpg
category:
    - 'Dicas de Linux'
    - 'Linux'
tag:

    - 'bash centos'
    - 'bash color'
    - 'bash colorido'
    - 'bash personalizado'
    - 'bash personalizado colorido linux'
    - 'centos terminal colorido'
    - 'centos terminal colors'
    - 'bash colorido'
    - 'terminal centos colorido'
---

- - - - - -

Olá 🖖🏼  
Uma dica da hora para quem administra servidores CentOS, é a personalização do Bash.  
Quem trabalha com diversas telas de prompt’s de servidores sabe do que estou falando, muitas vezes acabamos confundindo o prompt da maquina local com o do servidor ou com outros servidores, e ai executando um determinado comando em outro host.

Então hoje resolvi trazer uma solução bem simples.

A configuração é a seguinte:

Editar o arquivo /root/.bashrc

```
root@centos-1 [~] # vi /root/.bashrc
```

Adicione o conteúdo abaixo na parte onde aparece “User specific aliases and functions”

```
alias ls='ls --color'
alias vi='vim'
export PS1='\[\033[01;31m\][\[\033[01;37m\]\t\[\033[01;31m\]] \[\033[01;32m\]\u\[\033[01;31m\]@\[\033[01;32m\]\h \[\033[01;31m\][\[\033[01;33m\]\w\[\033[01;31m\]] \[\033[01;37m\]# \[\033[00m\]'
```

Obs.: Para o VIM funcionar colorido no CentOS, atualize o ‘vim’ e instale o pacote ‘vim-enhanced’.

Só reiniciar ou sair da sua sessão e acessar novamente e pronto!

👋🏼 Até a próxima!

- - - - - -

![](http://tidahora.com.br/wp-content/uploads/2017/11/foto-perfil-redondo-johnny.png)

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -