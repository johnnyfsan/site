---
title: 'Desativando a Complexidade de Senha no Samba 4'
date: '2018-10-11T14:12:58-03:00'
image: ../assets/img/uploads/2018/10/desativando-a-complexidade-de-senha-no-samba-4.png
tag:
    - 'complexidade senha samba'
    - 'complexidade senha samba 4'
    - 'complexidade smb4'
    - 'desativar complexidade senha samba'
    - 'desativar complexidade senha samba 4'
    - 'desativar senha complexa samba'
    - 'desativar senha complexa samba 4'
    - 'remover senha complexa samba'
    - 'remover senha complexa samba 4'
    - 'senha samba'
    - 'senhas samba'
    - 'senhas samba4'
---

- - - - - -

Olá, 🚀  
Mais uma dica aqui para quem gerencia servidores rodando Samba 4.

Para um usuário comum, definir uma senha complexa, envolvendo caracteres especiais, números, letras em maiúsculos e minúsculos é muito ruim, certamente o administrador do domínio terá grandes demandas para redefinição de senhas, pois os usuários vão simplesmente esquecer a senha que definiram ou até mesmo bloquear a conta de usuário por tentativa e erro na senha.

Para diminuir essa complexidade nas senhas dos perfis dos usuários, criei esse artigo que vai te ajudar a deixar a sua vida e dos usuários muito mais simples. 😎

A primeira coisa a se fazer é visualizar as informações presentes no servidor Samba 4.

Execute o comando abaixo:

```bash
samba-tool domain passwordsettings show
```

Se você compilou o Samba 4, e não sabe onde está o seu executável, é provável que ele esteja dentro do diretório “/usr/local/samba”.

Você pode procurar utilizando o comando “whereis” 😉

```bash
# whereis samba
samba: /usr/local/samba
```

No caso de servidores rodando em CentOS 6 ou CentOS 7 o caminho é:

```bash
/usr/local/samba/bin/samba-tool domain passwordsettings show
```

Retorno do comando acima:

```bash
Password informations for domain 'DC=tidahora,DC=local'

Password complexity: on
Store plaintext passwords: off
Password history length: 24
Minimum password length: 7
Minimum password age (days): 1
Maximum password age (days): 42
Account lockout duration (mins): 30
Account lockout threshold (attempts): 0
Reset account lockout after (mins): 30
```

Para desativarmos a complexidade da senha, digite o comando abaixo:

```bash
/usr/local/samba/bin/samba-tool domain passwordsettings set --complexity=off
```

O comando acima vai retornar a mensagem abaixo, informando que a complexidade foi desativar e as configurações já foram salvas.

```bash
Password complexity deactivated!
All changes applied successfully!
```

Agora, se você desejar alterar a quantidade minima de caracteres para a senha dos usuários, utilize o comando abaixo, com o parâmetro “–min-pwd-lenght=x”, onde X é o número de caracteres que precisa ter na senha, por padrão a quantidade requerida é de 7 caracteres.

```bash
/usr/local/samba/bin/samba-tool domain passwordsettings set --min-pwd-length=4
```

Retorno do comando acima, informando o sucesso na execução.

```bash
Minimum password length changed!
All changes applied successfully!
```

Agora vamos visualizar as alterações feitas, utilizando o comando com parâmetro “show”.

```bash
/usr/local/samba/bin/samba-tool domain passwordsettings show
```

Retorno do comando acima:

```bash
Password informations for domain 'DC=tidahora,DC=local'

Password complexity: off
Store plaintext passwords: off
Password history length: 24
Minimum password length: 4
Minimum password age (days): 1
Maximum password age (days): 42
Account lockout duration (mins): 30
Account lockout threshold (attempts): 0
Reset account lockout after (mins): 30
```

Vlw’s, Flw’s.

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Valeu! e até a próxima!

- - - - - -


**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -