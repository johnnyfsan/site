---
title: 'Como Criar um novo usuário  e Conceder Permissões no MySQL'
date: '2017-12-15'
image: ../assets/img/uploads/2017/12/como-criar-usuario-e-conceder-permissoes-no-mysql.png
category:
    - 'Dicas de Linux'
    - 'Linux'
    - 'MySQL' 
tag:
    - 'adicionar usuario mysql'
    - 'como criar um login no mysql'
    - 'como criar um usuario no mysql'
    - 'criando login no mysql'
    - 'criando usuario mysql'
    - 'criar login mysql'
    - 'criar usuario mysql e permissoes'
    - 'how to create a user mysql'
    - 'permissoes mysql'
    - 'permissoes no mysql'
    - 'redefinir login mysql'

---

- - - - - -


Olá, como vai? 😎

Hoje vou passar uma dica bem rápida, porem valiosa para quem gerencia servidores linux, principalmente servidores web. Que é a criação de um usuário no banco de dados MySQL e na sequência atribuir as permissões ao usuário criado.

Primeiramente conecte-se ao console do MySQL Server:

```
mysql -u root -p
```

Agora vamos criar um novo usuário no MySQL:

```
CREATE USER 'novousuario'@'localhost' IDENTIFIED BY 'password';
```

Após criarmos o usuário, precisamos definir qual banco ele poderá acessar.

Suponhamos que ele terá acesso ao banco de dados chamado “tidahora\_db”, vamos efetuar as permissões para o usuário **novousuario**” acessar o banco "**tidahora\_db**"

```
GRANT ALL PRIVILEGES ON  tidahora_db.* TO 'novousuario'@'localhost';
```

Os asteriscos no comando acima, significa que estamos liberando acesso a todas as tabelas do banco de dados “tidahora\_db”.

O próximo passo agora é recarregar os privilégios ajustados.

```
FLUSH PRIVILEGES;
exit;
```

Para testar o procedimento realizado, basta acessar o console do MySQL com o usuário criado e verificar se o mesmo possui acesso ao banco de dados “tidahora\_db”.

```
mysql -u novousuario -p
```

```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| tidahora_db        |
+--------------------+
2 rows in set (0,16 sec)
```

```
mysql> USE tidahora_db;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SHOW TABLES;
+---------------------------+
| Tables_in_tidahora_db     |
+---------------------------+
| db_producao               |
| db_usuarios               |
| db_permissoes             |
+---------------------------+
3 rows in set (0,00 sec)
```


Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Valeu! e até a próxima!

- - - - - -

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -