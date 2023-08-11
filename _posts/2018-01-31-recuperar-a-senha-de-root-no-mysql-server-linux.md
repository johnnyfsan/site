---
title: 'Recuperar a senha de root no MySQL Server [Linux]'
date: '2018-01-31T15:27:48-02:00'
type: post
image: ../assets/img/thumb/recuperar-senha-de-root-no-mysql-server-linux.png
tag:
    - 'mysql senha recuperar'
    - 'recovery mysql password'
    - 'recovery password root mysql'
    - 'recuperar mysql login'
    - 'recuperar senha mysql'
    - 'recuperar senha root mysql'
    - 'redefinir mysql root'
    - 'redefinir senha mysql'
    - 'senha root mysql'
---
- - - - - -

Opa, beleza? As vezes temos alguns problemas com a senha do usuário “root” no MySQL Server, ou esquecemos, ou simplesmente precisamos recuperar a senha que foi implementada por outro profissional de TI. Por esse motivo resolvi compartilhar com você esse tutorial.

A primeira coisa a se fazer para recuperar a senha do “root” é parar o serviço do MySQL Server no servidor ou host Linux.

Em ambientes baseados em Debian:

```
service mysql stop
```

Para CentOS 6:

```
/etc/init.d/mysqld stop
```

Já no CentOS 7 a sintaxe para parar o serviço muda:

```
systemctl stop mysqld
```

O próximo passo é adicionar um parâmetro ao arquivo de configuração do MySQL, o “my.cnf”:

Em ambientes baseados no Debian, pode ser que o arquivo principal esteja em “/etc/mysql/”

```
vi /etc/mysql/my.cnf
```

Já no CentOS é direto na estrutura do “/etc”

```
vi /etc/my.cnf
```

Insira a linha abaixo no final do arquivo:

```
skip-grant-table
```

Em seguida, após salvar o arquivo com o parâmetro acima adicionado, inicie novamente o serviço do MySQL.

Debian:

```
service mysql start
```

CentOS 6:

```
/etc/init.d/mysqld start
```

CentOS 7:

```
systemctl start mysqld
```

Agora vamos logar no console do MySQL, sem o parâmetro “-p” responsável pela autenticação por senha:

```
mysql -u root
```

Após logar no console do MySQL, execute o processo abaixo para recuperar a senha do “root”.

```
mysql> USE mysql;
mysql> UPDATE user set password=PASSWORD('senha') WHERE user='root';
mysql> FLUSH PRIVILEGES;
mysql> quit
```

Agora edite novamente o arquivo “my.cnf” e comente ou exclua a linha com o parâmetro “skip-grant-table”, agora reinicie o serviço do mysql.

Debian:

```
service mysql restart
```

CentOS 6:

```
/etc/init.d/mysqld restart
```

CentOS 7:

```
systemctl restart mysqld
```

Agora pode logar novamente com o root e a senha que você definiu.

```
mysql -u root -psenha
```

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Valeu! e até a próxima!

- - - - - -

![](http://tidahora.com.br/wp-content/uploads/2017/11/foto-perfil-redondo-johnny.png)

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -
