---
layout: post
title: 'Substituindo textos em arquivos no Linux'
date: '2017-11-23T14:09:34-02:00'
image: /assets/img/thumbdicas-de-linux.jpg
tags: [sed, sed linux, como usar o comando sed, substituir string linux, substituir texto arquivo linux]
---


- - - - - -


Olá, 🖖🏼

Essa dica é muito útil e rápido de fazer, principalmente quando estamos com aquela pressa de terminar uma configuração e iniciar o serviço com mais rapidez.

Vamos utilizar o comando *“sed”:*

A sintaxe do comando é a seguinte:

```
sed -i 's,Texto Antigo,Texto Novo,g' arquivo
```

Abaixo temos um arquivo de testes chamado “lista-de-sites.txt” o conteúdo do arquivo é o seguinte:

```
root@fsociety [~] # cat lista-de-sites.txt 
tidahora.com.br
google.com.br
vivaolinux.com.br
centos.org
debian.org
linkedin.com
facebook.com
```

Vamos substituir com o comando “sed” o site “tidahora.com.br” para “WWW.TIDAHORA.COM.BR”:

```
sed -i 's,tidahora.com.br,WWW.TIDAHORA.COM.BR,g' lista-de-sites.txt
```

Vamos ver o conteúdo do arquivo após alteração:

```
root@fsociety [~] # cat lista-de-sites.txt 
WWW.TIDAHORA.COM.BR
google.com.br
vivaolinux.com.br
centos.org
debian.org
linkedin.com
facebook.com
```

É possível alterar utilizando o “VI” ou o “VIM”.

Veja abaixo, abra o arquivo desejado:

```
vim lista-de-sites.txt
```

Vamos substituir o texto, *facebook.com* por *INSTAGRAM.COM*

Após abrir o arquivo pressione a tecla *“ESC”* e digite o comando abaixo:

```
:%s/Palavra Antiga/Palavra Nova
```

```
:%s/facebook.com/INSTAGRAM.COM
```

Não esqueça de pressionar *“Enter”* para validar a alteração.

Em seguida podemos salvar o arquivo e sair do modo edição.

Para salvar e sair pressione: *“ESC”* + as teclas *:wq* em seguida *“Enter”*.  
Vamos conferir o conteúdo do arquivo que alteramos com o *“VIM”.*

```
root@fsociety [~] # cat lista-de-sites.txt 
WWW.TIDAHORA.COM.BR
google.com.br
vivaolinux.com.br
centos.org
debian.org
linkedin.com
INSTAGRAM.COM
```

Fácil né? Isso ajuda muito no dia-a-dia na administração de servidores linux.

Dúvidas? Postem nos comentários!  
👋🏼 Até a próxima!

- - - - - -

![](http://tidahora.com.br/wp-content/uploads/2017/11/foto-perfil-redondo-johnny.png)

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -