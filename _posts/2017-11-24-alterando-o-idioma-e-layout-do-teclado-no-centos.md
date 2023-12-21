---
title: 'Alterando o Idioma e Layout do Teclado no CentOS'
date: '2017-11-24'
image: ../assets/img/uploads/2017/11/dicas-de-linux.jpg
category:
    - 'Dicas de Linux'
    - 'Linux'
tag:
    - 'alterar idioma teclado centos'
    - 'alterar idioma teclado linux'
    - 'change layout keyboard centos'
    - 'layout teclado centos'
    - 'mudar teclado centos'
    - 'mudar teclado linux'
    - 'teclado abnt linux'

---

- - - - - -


Olá, 🖖🏼

Uma boa dica para quem as vezes fica quebrando a cabeça para trocar o idioma e layout do teclado no CentOS.

Antes de iniciar a configuração no arquivo desejado, vamos fazer um backup do mesmo.

```
cp -Rfa /etc/sysconfig/keyboard{,.bkp}
```

Em seguida podemos edita-lo com mais segurança, abra o arquivo com um editor de texto de sua preferência, vou utilizar o *“VIM”*.

```
vim /etc/sysconfig/keyboard
```

Vamos supor que seu teclado esteja com o formato americano.

A configuração que estará no mesmo é a seguinte:

```
KEYTABLE="us"
MODEL="pc105+inet"
LAYOUT="us"
```

Mas queremos configurar para o ABNT-2 (Brasil sil sil sil).

Altere conforme abaixo:

```
KEYTABLE="br-abnt2"
MODEL="abnt2"
LAYOUT="br"
KEYBOARDTYPE="pc"
```

Salve seu arquivo ( “*ESC :wq*“).

Em seguida podemos reiniciar o host linux, para validar as nossas alterações no layout do teclado. ⌨️

Se você gosta de atalhos, pode gostar de conhecer esse tipo de teclado que pode te gerar mais produtividade.  
[Confira aqui](https://www.digitow.com.br/blog/teclados-mecanicos/).

Dúvidas? Postem nos comentários!  
👋🏼 Até a próxima!

- - - - - -

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -