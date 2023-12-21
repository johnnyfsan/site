---
title: 'Atualizar o MariaDB 5.6 para 10.2 no CentOS 7'
date: '2019-03-15'
image: ../assets/img/uploads/2019/03/ATUALIZAR-MARIADB-5.6-PARA-10.2-CENTOS-7.png
category:
    - 'CentOS'
    - 'Linux'
tag:
    - 'atualizar mariadb server'
    - 'atualizar mariadb server linux centos 7'
    - 'atualizar mysql centos 7'
    - 'mariadb 5.6 para 10.2'
    - 'update mariadb'

---

- - - - - -


Opa, blza? 😎  
Um post rápido para atualização do MariaDB no CentOS 7.

Observação: Antes de continuar a atualização, realize o Dump de suas bases de dados. Se ocorrer algum erro durante o processo, você não corre risco de perder dados importantes.

Bora começar, antes de tudo vamos parar o serviço do MariaDB, isso é importante para que quando estiver ocorrendo a atualização, o banco de dados fique offline para as aplicações, dessa forma não teremos nada sendo gravado nesse momento, evitando assim dados corrompidos.


```
systemctl stop mariadb
```

Vamos adicionar o repositório para atualização do MariaDB, acesse o diretório abaixo:


```
cd /etc/yum.repos.d/
```

Em seguinte crie o seguinte arquivo abaixo:


```
vim MariaDB.repo
```

Conteúdo o arquivo MariaDB.repo:


```
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.2/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```


Salve o arquivo e saia do modo edição do VIM (ESC + :x)

Após isso já podemos estar efetuando o comando para realizar o update do serviço.


```
yum update mariadb-server -y
```


Após a conclusão do update, vamos iniciar o serviço:


```
systemctl start mariadb
```


Verificando a versão do MariaDB:


```
systemctl status mariadb

● mariadb.service - MariaDB 10.2.22 database server
   Loaded: loaded (/usr/lib/systemd/system/mariadb.service; disabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/mariadb.service.d
           └─migrated-from-my.cnf-settings.conf
   Active: active (running) since Fri 2019-03-15 10:19:30 -03; 3s ago
     Docs: man:mysqld(8)
           https://mariadb.com/kb/en/library/systemd/
  Process: 12327 ExecStartPost=/bin/sh -c systemctl unset-environment _WSREP_START_POSITION (code=exited, status=0/SUCCESS)
  Process: 12283 ExecStartPre=/bin/sh -c [ ! -e /usr/bin/galera_recovery ] && VAR= ||   VAR=`/usr/bin/galera_recovery`; [ $? -eq 0 ]   && systemctl set-environment _WSREP_START_POSITION=$VAR || exit 1 (code=exited, status=0/SUCCESS)
  Process: 12281 ExecStartPre=/bin/sh -c systemctl unset-environment _WSREP_START_POSITION (code=exited, status=0/SUCCESS)
 Main PID: 12295 (mysqld)
   Status: "Taking your SQL requests now..."
    Tasks: 30
   Memory: 113.1M
   CGroup: /system.slice/mariadb.service
           └─12295 /usr/sbin/mysqld

```


Após isso já temos o MariaDB rodando na versão 10.2, atualizada com sucesso.


Dúvidas? Postem nos comentários!  
👋🏼 Até a próxima!

- - - - - -

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -