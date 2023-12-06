---
title: 'Conexão SSH sem Senha em Ambientes Linux'
date: '2023-12-06'
image: ../assets/img/uploads/2017/11/dicas-de-linux.jpg
tag:
    - 'conectar linux sem senha'
    - 'conectar ssh sem senha'
    - 'conexao ssh sem senha'
    - 'dicas de linux'
    - 'dicas de servidor linux'
    - 'dicas de tecnologia'
    - 'dicas de ti'
    - 'dicas linux'
    - 'linux tips'
    - 'password ssh'
    - 'ssh no linux sem senha'
    - 'ssh no passwd'
    - 'ssh no password linux'
    - 'ssh sem senha'
    - 'ssh senha linux'
---

- - - - - -

Olá, 🐧

Mais uma dica da hora sobre ambientes Linux, aprenda aqui nesse post como ter uma conexão sem senha entre dois servidores linux, ou até mesmo um host e um servidor, 😉

Nosso ambiente de implementação utilizado é o seguinte:  
 **Um Cliente:** Debian Squeeze  
**IP:** 10.106.0.100

**Um Servidor:** CentOS 6.3  
**IP:** 10.106.0.200

#### Gerando as Chaves no Cliente (Debian Squeeze – IP: 10.106.0.100)

```
ssh-keygen -t rsa
```

![](../assets/img/uploads/2017/12/ssh-sem-senha-01.png)

Note que será pedido para digitar com a “passphrase” apenas tecle ‘enter’ para que fique em branco, se digitar algo está frase precisará ser digita quando solicitada. Feito isso, temos as chaves criadas.

Chave Pública encontra-se em *“/root/.ssh/id\_rsa.pub”* Chave Privada encontra-se em *“/root/.ssh/id\_rsa”*

![](../assets/img/uploads/2017/12/ssh-sem-senha-02.png)

#### Ajustando o Servidor

Para efetuar a conexão SSH sem a senha entre cliente e server, vamos precisar divulgar para o servidor a Chave Pública gerado em nosso cliente.

Vamos então copia-lá para o servidor:

```
scp /root/.ssh/id_rsa.pub root@10.106.0.200:/root
```

![](../assets/img/uploads/2017/12/ssh-sem-senha-03.png)

Agora vamos ao console do Servidor para continuar o procedimento:

```
cat /root/id_rsa.pub >> /root/.ssh/authorized_keys
```

![](../assets/img/uploads/2017/12/ssh-sem-senha-04.png)

Acabamos de fazer a cópia do arquivo `id\_rsa.pub` do cliente para o servidor, e na sequencia jogamos a chave para o arquivo `authorized\_keys`.

Com isso já podemos efetuar as conexões do cliente para o servidor sem senha.

![](../assets/img/uploads/2017/12/ssh-sem-senha-05.png)

**Observações:** Não é totalmente seguro. Sempre bom trocar a chave entre cliente e servidor.

👋🏼 Até a próxima!

- - - - - -


**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -