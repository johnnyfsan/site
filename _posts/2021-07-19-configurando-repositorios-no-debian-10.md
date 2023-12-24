---
title: 'Configurando repositórios no Debian 10'
date: '2021-07-19'
image: ../assets/img/uploads/2021/07/debian-10.jpeg
category:
    - 'Debian'
    - 'Dicas de Linux'
    - 'Linux'
tag:
    - 'configurar repositorios debian 10'
    - 'configurar sources.list debian 10'
    - 'instalar repositorios debian'
    - 'instalar repositorios debian 10'
    - 'repositorios debian 10'

---

- - - - - -


Oi Pessoal, tudo bem?

O tutorial de hoje é bem simples e objetivo, somente para mostrar como configurar os repositórios no Debian 10 (Buster).

O primeiro passo é fazer um backup do arquivo original dos repositórios do Debian.

```
sudo cp -Rfa /etc/apt/sources.list{,.bkp}
```

Agora que temos um backup do arquivo padrão, vamos excluir o arquivo e criar um do zero.

```
sudo rm -rf /etc/apt/sources.list
```

Criando outro arquivo:

```
sudo touch /etc/apt/sources.list
```

Agora com o editor de texto de sua preferência abra o arquivo que criamos acima.

No meu caso vou estar utilizando o **VI**.

```
vi /etc/apt/sources.list
```

Copie o conteúdo abaixo e cole na sua **sources.list**.

```
# deb cdrom:[Debian GNU/Linux testing _Buster_ - Official Snapshot amd64 NETINST 20190618-22:45]/ buster contrib main non-free
#deb cdrom:[Debian GNU/Linux testing _Buster_ - Official Snapshot amd64 NETINST 20190618-22:45]/ buster contrib main non-free

deb http://deb.debian.org/debian/ buster main non-free contrib
deb-src http://deb.debian.org/debian/ buster main non-free contrib

deb http://security.debian.org/debian-security buster/updates main contrib non-free
deb-src http://security.debian.org/debian-security buster/updates main contrib non-free

# buster-updates, previously known as 'volatile'
deb http://deb.debian.org/debian/ buster-updates main contrib non-free
deb-src http://deb.debian.org/debian/ buster-updates main contrib non-free

# buster-backports, previously on backports.debian.org
deb http://deb.debian.org/debian/ buster-backports main contrib non-free
deb-src http://deb.debian.org/debian/ buster-backports main contrib non-free
```

Salve o arquivo e saia do modo de edição.

Se estiver usando o VI / VIM como eu, digite a tecla **“ESC”** mais as teclas **“:wq”**.

Após isso execute o comando **“sudo apt-get update”** para atualizar seus novos repositórios.

```
sudo apt-get update
```

Feito isso, seu Debian 10 está com repositórios pronto para uso.

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Até a próxima!

- - - - - -

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -