---
title: 'Como adicionar e remover usuários de grupos no Linux'
date: '2018-04-24'
image: ../assets/img/uploads/2017/11/dicas-de-linux.jpg
category:
    - 'Dicas de Linux'
    - 'Linux'
tag:
    - 'add user group'
    - 'add usuario group'
    - 'adicionando usuario a um grupo no linux'
    - 'adicionar usuario grupo linux'
    - 'como adicionar grupo usuario linux'
    - 'dicas de linux'
    - 'dicas linux'
    - 'gerenciamento linux grupos usuarios'
    - 'group user linux'
    - 'grupo usuario linux'
    - 'linux basico'
    - 'linux iniciante'
    - 'linux para iniciantes'
---

- - - - - -


🐧 Olá meu nobre leitor.  
Uma dica rápida de como estar realizando o gerenciamento de usuário a grupos em sistemas linux.

Normalmente usamos o controle por grupo, quando temos um servidor de arquivos, onde criamos os grupos e adicionamos usuários a esses grupos, para facilitar na gestão do samba.

Mas normalmente precisamos alterar ou incluir um usuário a um determinado grupo, e eis a questão, como executar isso de forma correta e rápida.

Para adicionar um usuário já existente no sistema linux, a um determinado grupo, usamos o comando abaixo:

```
usermod -a -G grupo usuario
```

Exemplo:

```
# usermod -a -G Comercial joao
```

Agora, se você precisa remover um usuário de um determinado grupo no linux, o comando é o seguinte:

```
gpasswd -d usuario grupo
```

Exemplo:

```
# gpasswd -d joao Comercial
```

Você também pode consultar as alterações realizadas com o comando ‘groups’, veja abaixo:

```
groups usuario
```

Exemplo:

```
# groups joao
joao : contab
```

Bem simples né, comandos pra você colocar na lista de boas práticas para um gerenciamento de alto nível em servidores Linux.

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Valeu! e até a próxima!

- - - - - -


**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -