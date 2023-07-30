---
title: 'Instalando o Bacula Backup no CentOS 7'
date: '2018-03-19T16:02:43-03:00'
type: post
image: /assets/img/thumb/instalando-o-bacula-backup-no-centos-7.png
tag:
    - 'bacula'
    - 'bacula backup'
    - 'bacula backup centos'
    - 'bacula backup centos 7'
    - 'bacula centos'
    - 'bacula centos 7'
    - 'bacula opensource'
    - 'how to install bacula backup centos 7'
    - 'instalar bacula backup centos'
    - 'instalar bacula backup centos 7'
    - 'instalar bacula centos'
    - 'instalar bacula centos 7'
---


- - - - - -

Opa, tudo certo? 🙂 Hoje vou mostrar como instalar o Bacula Backup no CentOS 7.

O Bacula é uma das melhores ferramentas de Backup Open Source, aliado a um sistema operacional de confiança como CentOS 7 que tem sua base no Red Hat, conseguimos obter confiança, bom desempenho e economia, não precisando desembolsar valores absurdos por uma ferramenta que talvez nem atenda as necessidades de um cliente ou de determinado projeto.

[![Instalando o Bacula Backup no CentOS 7](/site/assets/img/uploads/2018/03/bacula2.png "Instalando o Bacula Backup no CentOS 7")](/site/assets/img/uploads/2018/03/bacula2.png)

Irei abordar somente a configuração do servidor de backup com Bacula.

Iremos realizar no tutorial as tarefas abaixo:

1. Instalação do Bacula
2. Configuração do Banco de Dados
3. Configuração do Bacula Backup
4. Agendamento de Trabalhos de Backup
5. Fazer Backup
6. Restaurar o Backup

Componentes do Bacula Backup
----------------------------

O Bacula trabalha no modo cliente-servidor, para que toda a comunicação seja realizada com o cliente de forma correta e segura, a ferramenta trabalha com a seguinte arquitetura:

- Bacula Director Daemon
- Bacula Storage Daemon
- Bacula File Daemon
- Bacula Catálogo
- Bacula Console

Instalando o Bacula Backup e o Banco de Dados
---------------------------------------------

O Bacula utiliza estrutura de banco de dados SQL, para o armazenamento das informações em seu catálogo, banco de dados como MySQL, PostgreSQL, para nosso ambiente vamos utilizar o MariaDB, um substituto para o MySQL.

```
yum -y update
yum install -y bacula-director bacula-storage bacula-console bacula-client mariadb-server
```

Quando finalizar a instalação dos pacotes acima, vamos iniciar o Banco de Dados

```
systemctl start mariadb
```

Agora que o MySQL (MariaDB) está instalado e funcionando, vamos criar o usuário e as tabelas de banco de dados Bacula, com esses scripts:

```
/usr/libexec/bacula/grant_mysql_privileges
/usr/libexec/bacula/create_mysql_database -u root
/usr/libexec/bacula/make_mysql_tables -u bacula
```

Na sequência vamos executar um script de segurança que irá remover algumas configurações padrões que vem por default. Com o script de segurança vamos bloquear ações perigosas e bloquear o acesso ao banco de dados.

```
mysql_secure_installation
```

Nosso próximo passo é definir a senha para o usuário bacula, para que o mesmo tenha acesso ao catálogo, e faça seu trabalho direito.

Logue no console do MariaDB ou MySQL, como quiser chamar, rs!

```
mysql -u root -p
```

Defina a senha para o usuário do banco de dados Bacula. Utilizando o comando abaixo, mas altere o campo “SENHA” para uma senha de sua preferência.

```
UPDATE mysql.user SET Password=PASSWORD('SENHA') WHERE User='bacula';
FLUSH PRIVILEGES;
exit
```

Agora vamos deixar o serviço do MariaDB ativo no boot do sistema.

```
systemctl enable mariadb
```

Ajustando os serviços do Bacula para iniciar junto ao boot do sistema linux.

```
systemctl enable bacula-dir
systemctl enable bacula-sd
systemctl enable bacula-fd
```

Precisamos fazer um pequeno ajuste no Bacula, o mesmo vem configurado por padrão para utilizar as bibliotecas do PostgreSQL. Precisamos definir as bibliotecas para o MySQL.

Execute o comando abaixo:

```
alternatives --config libbaccats.so
```

Selecione a opção **1**.

```
There are 3 programs which provide 'libbaccats.so'.

  Selection    Command
-----------------------------------------------
   1           /usr/lib64/libbaccats-mysql.so
   2           /usr/lib64/libbaccats-sqlite3.so
*+ 3           /usr/lib64/libbaccats-postgresql.so

Enter to keep the current selection[+], or type selection number: 1
```

Criando Diretório de Backup e Restore
-------------------------------------

O Bacula precisa de um diretório de armazenamento de volumes e outro diretório para restaurações. É possível utilizar diversos diretórios para armazenamento, desde que a configuração dos volumes e permissões estejam de acordo, a gravação ira funcionar perfeitamente.

Agora vamos criar os diretórios.

```
mkdir /bacula/
mkdir /bacula/backup
mkdir /bacula/restore
```

Ajustando as Permissões nos diretórios, realizando a segurança onde somente o processo do Bacula e o usuário root tenham acesso.

```
chown -R bacula:bacula /bacula
chmod -R 700 /bacula
```

Configurando o Arquivo Hosts
----------------------------

Vamos utilizar a resolução de nomes nos arquivos de configuração do Bacula, precisamos ajustar o arquivo hosts de maneira apropriada.

```
vim /etc/hosts
```

```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

# IP LOOPBACK SERVIDOR BACULA BACKUP #
127.0.0.1       bacula bacula.tidahora.com.br
```

Configuração do Bacula Director
-------------------------------

O arquivo bacula-dir é o principal arquivo de configuração para a implementação do Bacula. Vamos acessa-lo em seu diretório e realizar uma cópia de backup.

```
cd /etc/bacula
cp -Rfa bacula-dir.conf{,.bkp}
rm -rf bacula-dir.conf
```

Criando o arquivo “bacula-dir.conf” com as configurações já ajustadas: Por padrão o Bacula vem com as opções de FileSets, Shedules, e clientes no seu arquivo principal, o bacula-dir.conf, para melhor administração do serviço nós vamos separar essas opções para um novo arquivo, cada função terá seu arquivo de configuração, mantendo assim o gerenciamento de forma mais limpa.

```
vim /etc/bacula/bacula-dir.conf
```

```
############################################
# ARQUIVO DE CONFIGURACAO PADRAO DO BACULA #
############################################

# DEFININDO O 'DIRECTOR' #
Director { 
 Name = bacula-dir
 DIRport = 9101 # Porta de Comunicacao do Bacula
 QueryFile = "/etc/bacula/query.sql" # Script de Query
 WorkingDirectory = "/var/spool/bacula" # Diretório de Trabalho do Bacula
 PidDirectory = "/var/run" # PID do Bacula
 Maximum Concurrent Jobs = 1 # Maximo de Backups em Execucao
 Password = "Cv70F6pf1t6pBopT4vQOnigDrR0v3L" # Senha para ajustar no Bconsole
 Messages = Daemon # Nivel de mensagens
 DirAddress = 127.0.0.1
}

# DEFINICOES DE CLIENTES E JOBS #
# ARQUIVO ONDE SERA CONFIGURADO E AJUSTADO OS CLIENTES E JOBS DO BACULA #
@/etc/bacula/bacula-dir-clients-and-jobs.conf

# DEFINICOES DE ARQUIVOS PARA BACKUP - (FILE SETS)#
# ARQUIVO ONDE SERA CONFIGURADO E AJUSTADO OS 'FILE SETS' DO BACULA #
@/etc/bacula/bacula-dir-filesets.conf

# DEFINICOES DE AGENDAMENTO DOS BACKUPS #
@/etc/bacula/bacula-dir-agendamento.conf

# DEFINICOES DE DISPOSITIVO DE ARMAZENAMENTO #
# AQUI DEFINIMOS O STORAGE A SER UTILIZADO PELO BACULA #
Storage {
 Name = File
 Address = bacula.tidahora.com.br # Pode ser usado Nome ou IP
 SDPort = 9103 # Porta de Comunicação do Storage
 Password = "LE_T_-c_55qu1f777Dm52map-s3xpgR4q" # Senha Storage Bacula
 Device = FileStorage # Device de Storage
 Media Type = File # Tipo de Midia (Fita, DVD, HD)
 Maximum Concurrent Jobs = 10 # Num. Maximo de Jobs executados nessa Storage.
}
# Generic catalog service
Catalog {
 Name = Catalogo # Nome do Catalogo
 dbname = "bacula"; dbuser = "bacula"; dbpassword = "SENHA" # Configuracoes do MySQL
}
# Reasonable message delivery -- send most everything to email address
# and to the console

Messages {
 Name = Standard
# ABAIXO E POSSIVEL AJUSTAR COMO O BACULA ENVIARA MENSAGENS AO ADMINISTRADOR #
 mailcommand = "/usr/sbin/bsmtp -h localhost -f \"\(Bacula\) \<%r\>\" -s \"Bacula: %t %e of %c %l\" %r"
 operatorcommand = "/usr/sbin/bsmtp -h localhost -f \"\(Bacula\) \<%r\>\" -s \"Bacula: Intervention needed for %j\" %r"
 mail = root@localhost = all, !skipped
 operator = root@localhost = mount
 console = all, !skipped, !saved
 append = "/var/log/bacula/bacula.log" = all, !skipped
 catalog = all
}

# Message delivery for daemon messages (no job).
Messages {
 Name = Daemon
 mailcommand = "/usr/sbin/bsmtp -h localhost -f \"\(Bacula\) \<%r\>\" -s \"Bacula daemon message\" %r"
 mail = root@localhost = all, !skipped
 console = all, !skipped, !saved
 append = "/var/log/bacula/bacula.log" = all, !skipped
}

# POOL PADRAO DEFINIDO
Pool {
 Name = File # o Job de Backup por padrao aponta para o 'File'
 Pool Type = Backup # O Tipo do Pool = Backup, Restore, Etc.
 Recycle = yes # Bacula can automatically recycle Volumes
 AutoPrune = yes # Prune expired volumes
 Volume Retention = 1 month # Retencao de Volume = 1 Mes
 Volume Use Duration = 23 hours # Duracao de um volume aberto
 Maximum Volume Bytes = 20 Gb # Tamanho maximo de um volume
 Maximum Volumes = 10 # Volume Bytes X Volumes <= Dispositivo de backup
 LabelFormat = "volume" # Nome Default do Volume
}
# Scratch pool definition
# Volumes que serao emprestado a alguma Pool temporariamente
Pool {
 Name = Scratch
 Pool Type = Backup
}
#

# Restricted console used by tray-monitor to get the status of the director
#
#Console {
# Name = bacula-mon
# Password = "S91uKqybG5NXwiCbzPYMg5Cfp0o7mW4Ze"
# CommandACL = status, .status
#}
```

O Próximo passo é ajustar os arquivos que fazem referências para FileSets, Agendamentos e Clientes.

Arquivo que contém informações de clientes e jobs do Bacula: – /etc/bacula/bacula-dir-clients-and-jobs.conf

```
vim /etc/bacula/bacula-dir-clients-and-jobs.conf
```

```
###################################################
## ARQUIVO DE CONFIGURACAO PARA CLIENTES E JOBS  ##
###################################################
 
# JOB PADRAO PARA O BACULA SERVER #
JobDefs {
        Name = "DefaultJobs"                            # Nome do Job Padrao
        Type = Backup                                   # Tipo de Job (Backup, Restore, Verificacao)
        Level = Incremental                             # Nivel do Job (Full, Incremental, Diferencial)
        Client = bacula-fd                              # Nome do Cliente FD
        FileSet = "Full Set"                            # File Set Definido para Esse Job
        Schedule = "WeeklyCycle"                        # Agendamento Definido para Esse Job
        Storage = File                                  # Define Storage
        Messages = Standard                             # Nivel de mensagens
        Pool = File                                     # Define a Pool   
        Priority = 10                                   # Qual o nivel de Prioridade
        Write Bootstrap = "/var/spool/bacula/%c.bsr"      # Arquivo gerado pelo Bacula para armazenar informacoes de backups feitos em seus clientes.
# AS OPCOES ABAIXO EVITAM QUE SEJAM DUPLICADO JOBS NO SERVIDOR, CASO TENHA UM JOB DUPLICADO O MESMO E CANCELADO
        Allow Duplicate Jobs = no                       # Permite Jobs Duplicados
        Cancel Lower Level Duplicates = yes             # Cancela niveis inferiores duplicados 
}
 
# JOB DE BACKUP PARA OS SERVIDORES WINDOWS SERVER #
JobDefs {
        Name = "DefaultWindows"                         # Nome do Job Para Servidores Windows
        Type = Backup                                   # Tipo de Job (Backup, Restore, Verificacao)
        Level = Incremental                             # Nivel do Job (Full, Incremental, Diferencial)
        Client = bacula-fd                              # Nome do Cliente FD
        FileSet = "Full Set"                            # File Set Definido para Esse Job
        Schedule = "WeeklyCycle"                        # Agendamento Definido para Esse Job
        Storage = File                                  # Define Storage
        Messages = Standard                             # Nivel de mensagens
        Pool = File                                     # Define a Pool
        Priority = 10                                   # Qual o nivel de Prioridade
        Write Bootstrap = "/var/spool/bacula/%c.bsr"      # Arquivo gerado pelo Bacula para armazenar informacoes de backups feitos em seus clientes.
# AS OPCOES ABAIXO EVITAM QUE SEJAM DUPLICADO JOBS NO SERVIDOR, CASO TENHA UM JOB DUPLICADO O MESMO E CANCELADO
        Allow Duplicate Jobs = no                       # Permite Jobs Duplicados
        Cancel Lower Level Duplicates = yes             # Cancela niveis inferiores duplicados
}
 
# JOB DE BACKUP DO CATALOGO #
Job {
        Name = "BackupCatalogo"                                                    # Nome do Job Para Backup do Catalogo
        JobDefs = "DefaultJobs"                                                    # JobDefs usado para operacao
        Level = Full                                                               # Nivel do Job (Full, Incremental, Diferencial)
        FileSet = "Catalogo"                                                       # File Set Definido para Esse Job
        Schedule = "WeeklyCycleAfterBackup"                                        # Agendamento Definido para Esse Job
        RunBeforeJob = "/usr/libexec/bacula/make_catalog_backup.pl Catalogo"       # Acao executada antes da operacao
        Write Bootstrap = "/var/spool/bacula/%n.bsr"                               # Arquivo gerado pelo Bacula para armazenar informacoes de backups feitos em seus clientes.
        Priority = 11                                                              # Executar depois do Backup - ajustar prioridade                   
}
 
# JOB DE RESTAURACAO - (RESTORE) - SO E PRECISO ESSE JOBS PARA RESTAURACAO DE BACKUP #
Job {
        Name = "RestoreFiles"                                                 # Nome do Job Para Restore
        Type = Restore                                                        # Tipo de Job (Backup, Restore, Verificacao)
        Client = bacula-fd                                                    # Nome do Cliente FD
        FileSet = "Full Set"                                                  # File Set Definido para Esse Job
        Storage = File                                                        # Agendamento Definido para Esse Job
        Pool = File                                                           # Define a Pool
        Messages = Standard                                                   # Nivel de mensagens
        Where = /bacula/restore                                               # Diretorio onde o bacula ira restaurar os arquivos nos clientes
}
 
#######################################################################
## AQUI VAMOS DEFINIR OS CLIENTES E JOBS PARA CADA CLIENTE ADICIONADO #
#######################################################################
 
## ------------------------------------------------------------------- ##
# JOB DE BACKUP PARA O DIRECTOR DO BACULA
# HOSTNAME: bacula.tidahora.com.br
 
Job {
        Name = "BackupDirector"                           # Nome do Job para Backup do Director (Proprio Servidor Bacula)
        JobDefs = "DefaultJobs"                           # JObDefs Definido
        Client = bacula-fd                                # Cliente fd
}
 
Client {
  Name = bacula-fd                                        # Cliente fd
  Address = bacula.tidahora.com.br                     # Ajustado no /etc/hosts
  Password = "ExgYbEinJWMf7lsWPBcpffZaNMmiGcbgp"          # Senha do Director do Bacula
  @/etc/bacula/clientes/bacula.tidahora.com.br.client  # Arquivo onde contem informacoes sobre o cliente.
}
## -------------------------------------------------------------------- ##
```

Arquivo onde contém informações de ‘File Sets’ do Bacula: – /etc/bacula/bacula-dir-filesets.conf

```
vim /etc/bacula/bacula-dir-filesets.conf
```

```
###################################################
## ARQUIVO DE CONFIGURACAO PARA CLIENTES E JOBS  ##
###################################################
# LISTA DOS ARQUIVOS QUE SERAO COPIADOS 

FileSet {
        Name = "Full Set"                                       # Nome do FileSets
# Arquivos que serao incluidos para serem copiados ao backup
        Include {                                          
                Options {
                        signature = MD5
                        compression = GZIP
                        verify = pin1
                        onefs = no
                }
                File = /etc
                File = /root
                File = /var/log
                File = /var/www
                File = /home
                File = /usr/sbin
                File = /etc/bacula/
                }
# Arquivos que serao ignorados ao backup
        Exclude {
                File = /var/lib/bacula
                File = /proc
                File = /tmp
                File = /.journal
                File = /.fsck
                }
}
# LISTA DOS ARQUIVOS QUE SERAO COPIADOS - CATALOGO #

FileSet {
        Name = "Catalogo"
# Arquivos que serao incluidos para serem copiados ao backup
        Include {
                Options {
                        signature = MD5
                }
                File = "/var/lib/bacula/bacula.sql"
                }
}

# LISTA DOS ARQUIVOS QUE SERAO COPIADOS - SISTEMA WINDOWS SERVER #

FileSet {
        Name = "WindowsFS"
# Arquivos que serao incluidos para serem copiados ao backup
        Include {
#               Plugin = "alldrivers"
                        Options {
                                signature = MD5
                                Compression = GZIP1
                                OneFS = no
                                }
                        File = "E:/"
                }
}
```

Vamos criar o arquivo onde vamos ajustar os agendamentos de backup, esse arquivo contém informações de horários de rotinas de backups.

```
vim /etc/bacula/bacula-dir-agendamento.conf
```

```
#################################################################
## ARQUIVO DE CONFIGURACAO DE AGENDAMENTO DE TAREFAS DO BACULA ##
#################################################################
# DEFINICOES DE AGENDAMENTO DOS BACKUPS #

# AGENDAMENTO PADRAO DO BACULA - CICLO SEMANAL DE BACKUP #
Schedule {
  Name = "WeeklyCycle"                          # Ciclo Semanal de Backup
  Run = Full 1st sun at 23:05                   # Backup Full no Primeiro Domingo do Mes as 23:05 hrs
  Run = Differential 2nd-5th sun at 16:50       # Backup Diferencial de Seg. a Sabado as 23:05 hrs
  Run = Incremental mon-sat at 10:30            # Backup Incremental de Seg. a Sabado as 23:05 hrs
}

# AGENDAMENTO PARA SERVIDOR LINUX - CICLO SEMANAL DE BACKUP #
Schedule {
  Name = "CicloBackupLinux"
  Run = Level=Full 1st sun at 23:00
  Run = Level=Full mon-sat at 10:00
}

# AGENDAMENTO PARA SERVIDOR WINDOWS SERVER - CICLO SEMANAL DE BACKUP #
Schedule {
  Name = "CicloBackupWindows"                          # Ciclo Semanal de Backup
  Run = Full 1st sun at 23:00                          # Backup Full no Primeiro Domingo do Mes as 23:05 hrs
  Run = Differential 2nd-5th sun at 14:10              # Backup Diferencial de Seg. a Sabado as 11:40 hrs
  Run = Full mon-sat at 10:00                          # Backup Incremental de Seg. a Sabado as 11:25
}

# DEFINICOES DE AGENDAMENTO DO BACKUP DOS CATALOGOS #
#        FEITO SEMPRE DEPOIS DOS BACKUPS            #
Schedule {
  Name = "WeeklyCycleAfterBackup"
  Run = Level=Full sun-sat at 09:15
}
```

Vamos agora criar o diretório onde vai ficar armazenados os arquivos com informações básicas de nossos clientes:

Crie o diretório “clientes”:

```
mkdir /etc/bacula/clientes
```

Antes de adicionar um cliente ao bacula vamos criar o arquivo de storage de backup. Criando o arquivo de Storage do backup:

```
cp -Rfa /etc/bacula/bacula-sd.conf{,.bkp}
rm -rf /etc/bacula/bacula-sd.conf
vim /etc/bacula/bacula-sd.conf
```

```
##############################################################
# ARQUIVO PADRAO DE CONFIGURACAO DE STORAGE DO BACULA	     #
##############################################################

Storage {                             
  Name = bacula-sd                                # Nome do Storage
  SDPort = 9103                                   # Porta do Director    
  WorkingDirectory = "/var/spool/bacula"          # Diretorio de Trabalho
  Pid Directory = "/var/run"                      # Pid do Bacula
  Maximum Concurrent Jobs = 20                    # Maximo de Backups em Execucao
  SDAddress = bacula.tidahora.com.br              # Nome ou IP do Storage do Bacula
}

#
# List Directors who are permitted to contact Storage daemon
#
Director {
  Name = bacula-dir
  Password = "LE_T_-c_55qu1f777Dm52map-s3xpgR4q"
}
#
# Restricted Director, used by tray-monitor to get the
#   status of the storage daemon
# Usado pelo tray-monitor do bacula para obter status do servidor
Director {
  Name = bacula-mon
  Password = "kNuOgEZUq2pqP6hRqPHMHm7UyxCOG1LBf"
  Monitor = yes
}

# ABAIXO DEFINIMOS O NOME, TIPO, E O DIRETÓRIO DE ARQUIVOS DO BACULA

Device {
  Name = FileStorage                      # Nome do Device
  Media Type = File                       # Tipo de Midia (DVD, CD, HD, FITA)
  Archive Device = /bacula/backup         # Diretorio onde serao salvos os volumes de backup
  LabelMedia = yes;                       # Midias de Etiquetamento do Bacula
  Random Access = Yes;                    # 
  AutomaticMount = yes;                   # Montar Automaticamente
  RemovableMedia = no;                    # Midia Removivel
  AlwaysOpen = no;                        # Manter Sempre Aberto
}

# 
# Send all messages to the Director, 
# mount messages also are sent to the email address
#
Messages {
  Name = Standard
  director = bacula-dir = all
}
```

Após criado o arquivo vamos criar o diretório de backup e restore ajustado no arquivo acima:

```
mkdir -p /bacula/backup /bacula/restore
chown -R bacula:bacula /bacula
chmod -R 700 /bacula
```

Para que o bacula consiga fazer seu próprio backup, precisamos ajustar o arquivo de cliente para ele: Crie o arquivo abaixo:

```
cp -Rfa /etc/bacula/bacula-fd.conf{,.bkp}
rm -rf /etc/bacula/bacula-fd.conf
vim /etc/bacula/bacula-fd.conf
```

```
##############################################################
# ARQUIVO DE CONFIGURACAO DO FILE DAEMON DO BACULA           #
##############################################################

#
# List Directors who are permitted to contact this File daemon
#
Director {
  Name = bacula-dir                                  # Nome do Director
  Password = "ExgYbEinJWMf7lsWPBcpffZaNMmiGcbgp"     # ESTA SENHA ESTA DEFINIDA NO ARQUIVO DE CLIENTE EM /ETC/BACULA/BACULA-DIR-CLIENTS-AND-JOBS.CONF
}

#
# Restricted Director, used by tray-monitor to get the
#   status of the file daemon
#
Director {
  Name = bacula-mon
  Password = "z27BNYXA9dx1SZWk1vp-kSZ8azwz2HMS8"    # ESTA SENHA E UTILIZADO PELO BACULA-MONITOR
  Monitor = yes
}
#
# "Global" File daemon configuration specifications
#
FileDaemon {                          
  Name = bacula-fd                                  # Nome do Bacula-fd
  FDport = 9102                                     # Porta de Comunicacao do bacula-fd
  WorkingDirectory = /var/spool/bacula              # Diretorio de trabalho
  Pid Directory = /var/run                          # Diretorio de Pid 
  Maximum Concurrent Jobs = 20                      # Numero maximo de jobs executados no bacula
#  FDAddress = 127.0.0.1	                    # COMENTAR OU REMOVER ESSA LINHA PARA QUE ELE POSSA 'OUVIR' CONEXOES EM TODAS AS INTERFACES
}

# Send all messages except skipped files back to Director
Messages {
  Name = Standard
  director = bacula-dir = all, !skipped, !restored        # AS MENSAGEM SAO ENCAMINHADAS PARA O 'BACULA-DIR' DEFINIDO NESSA LINHA
}
```

Feito isso vamos criar o arquivo de cliente com as informações de catalogos para o nosso bacula:

```
vim /etc/bacula/clientes/bacula.tidahora.com.br.client
```

```
##########################################################
## ARQUIVO PARA CONFIGURACAO DE CLIENTE LINUX NO BACULA ##
##########################################################
        Catalog = Catalogo				# Nome do Catalogo definido
        File Retention = 30 days                        # Tempo de Retencao do Backup
        Job Retention = 6 months                        # Tempo de Retencao do Job
        AutoPrune = yes                                 # Prune de Jobs/Arquivos Expirados
```

Agora que temos os arquivos criados, vamos alterar as permissões deles e do diretório onde o Bacula irá armazenar os Volumes de backup.

Abaixo, segue lista dos arquivos que criamos:

- bacula-dir.conf ← Arquivo Director do Bacula
- bacula-sd.conf ← Arquivo de Definições de Storage
- bacula-fd.conf ← Arquivo de cliente para o bacula
- bacula-dir-clients-and-jobs.conf ← Informações de Clientes e Jobs
- bacula-dir-filesets.conf ← Informações de FileSets
- bacula-dir-agendamento.conf ← Arquivo de horários de Jobs de Backups clientes/bacula.tidahora.com.br ← Informações de Catalogo

Diretório onde o Bacula irá armazenar os Volumes de Backup: “/bacula/backup/”

Vamos ajustar as permissões aos mesmos:

```
chown bacula:bacula bacula-dir-clients-and-jobs.conf
chown bacula:bacula bacula-dir-filesets.conf
chown bacula:bacula bacula-dir-agendamento.conf
chown bacula:bacula bacula-dir.conf 
chown bacula:bacula bacula-fd.conf 
chown bacula:bacula bacula-sd.conf
chown bacula:bacula clientes
chown bacula:bacula clientes/*
```

Iniciando os Serviços do Bacula
-------------------------------

```
systemctl start bacula-dir
systemctl start bacula-sd
systemctl start bacula-fd
```

Verificando os processos rodando…

```
ps aux |grep bacula
```

Ajustando o SELinux e o FirewallD
---------------------------------

Precisamos definir permissão no SELINUX para que possamos gravar nos diretórios de Backup e Restore. Execute o comando abaixo para habilitar a permissão de gravação nos diretórios. <span class="" id="result_box" lang="pt">O comando abaixo irá restaurar de forma recursiva o contexto do SELinux para o diretório<span class="hps"> /bacula e seus subdiretórios.</span>  
</span>

```
restorecon -R -v /bacula/
```

```
restorecon reset /bacula context unconfined_u:object_r:default_t:s0->unconfined_u:object_r:bacula_store_t:s0
restorecon reset /bacula/backup context unconfined_u:object_r:default_t:s0->unconfined_u:object_r:bacula_store_t:s0
restorecon reset /bacula/restore context unconfined_u:object_r:default_t:s0->unconfined_u:object_r:bacula_store_t:s0
```

Em seguida, vamos ajustar o FirewallD no CentOS 7 para permitir as conexões ao Bacula.

```
firewall-cmd --permanent --zone=public --add-port=9101/tcp
firewall-cmd --permanent --zone=public --add-port=9102/tcp
firewall-cmd --permanent --zone=public --add-port=9103/tcp
firewall-cmd --reload
```

Ajustando o Bacula Console
--------------------------

Precisamos passar a senha do Bacula Director para arquivo de configuração do Bacula Console.

Vamos editar o arquivo bconsole.conf.

Pegando a senha.

```
grep -R "Password" /etc/bacula/bacula-dir.conf
Password = "Cv70F6pf1t6pBopT4vQOnigDrR0v3L" # Senha para ajustar no Bconsole
Password = "LE_T_-c_55qu1f777Dm52map-s3xpgR4q" # Senha Storage Bacula
# Password = "S91uKqybG5NXwiCbzPYMg5Cfp0o7mW4Ze"
```

Abra o arquivo bconsole.conf

```
vim /etc/bacula/bconsole.conf
```

E insira no arquivo de configuração a senha do director.

```
#
# Bacula User Agent (or Console) Configuration File
#

Director {
  Name = bacula-dir
  DIRport = 9101
  address = localhost
  Password = "Cv70F6pf1t6pBopT4vQOnigDrR0v3L"
}
```

Reinicie os serviços do Bacula.

```
systemctl restart bacula-dir
systemctl restart bacula-sd
systemctl restart bacula-fd
```

Criando os Volumes
------------------

Em seguida vamos criar os volumes no bacula.

Abra o bacula console.

```
bconsole
```

Digite **‘label’** e na linha **Enter new Volume name:** digite o nome do volume, no meu caso coloquei *“volume-backup”,* em seguida selecione o tipo do Pool para **File**.

```
*label
Automatically selected Catalog: Catalogo
Using Catalog "Catalogo"
Automatically selected Storage: File
Enter new Volume name: volume-backup
Defined Pools:
     1: File
     2: Scratch
Select the Pool (1-2): 1
Connecting to Storage daemon File at bacula.tidahora.com.br:9103 ...
Sending label command for Volume "volume-backup" Slot 0 ...
3000 OK label. VolBytes=191 DVD=0 Volume="volume-backup" Device="FileStorage" (/bacula/backup)
Catalog record for Volume "volume-backup", Slot 0  successfully created.
Requesting to mount FileStorage ...
3906 File device ""FileStorage" (/bacula/backup)" is always mounted.
```

Executando Backup Local
-----------------------

No Bacula Console.

```
bconsole
```

```
*run
A job name must be specified.
The defined Job resources are:
     1: BackupCatalogo
     2: RestoreFiles
     3: BackupDirector
Select Job resource (1-3): 3
Run Backup job
JobName:  BackupDirector
Level:    Incremental
Client:   bacula-fd
FileSet:  Full Set
Pool:     File (From Job resource)
Storage:  File (From Job resource)
When:     2015-12-15 10:35:33
Priority: 10
OK to run? (yes/mod/no): yes
Job queued. JobId=2
You have messages.
*m
15-Dec 10:35 bacula-dir JobId 2: No prior Full backup Job record found.
15-Dec 10:35 bacula-dir JobId 2: No prior or suitable Full backup found in catalog. Doing FULL backup.
*status dir
bacula-dir Version: 5.2.13 (19 February 2013) x86_64-redhat-linux-gnu redhat (Core)
Daemon started 15-Dec-15 10:32. Jobs: run=0, running=1 mode=0,0
 Heap: heap=270,336 smbytes=91,641 max_bytes=96,108 bufs=286 max_bufs=287

Scheduled Jobs:
Level          Type     Pri  Scheduled          Name               Volume
===================================================================================
Full           Backup    11  16-Dec-15 09:15    BackupCatalogo     volume
Incremental    Backup    10  16-Dec-15 10:30    BackupDirector     volume
====

Running Jobs:
Console connected at 15-Dec-15 10:33
 JobId Level   Name                       Status
======================================================================
     2 Full    BackupDirector.2015-12-15_10.35.36_03 is running
====
No Terminated Jobs.
====
*
```

Verificando o Backup…

```
*status dir
bacula-dir Version: 5.2.13 (19 February 2013) x86_64-redhat-linux-gnu redhat (Core)
Daemon started 15-Dec-15 10:32. Jobs: run=1, running=0 mode=0,0
 Heap: heap=270,336 smbytes=89,500 max_bytes=99,629 bufs=265 max_bufs=298

Scheduled Jobs:
Level          Type     Pri  Scheduled          Name               Volume
===================================================================================
Full           Backup    11  16-Dec-15 09:15    BackupCatalogo     volume
Incremental    Backup    10  16-Dec-15 10:30    BackupDirector     volume
====

Running Jobs:
Console connected at 15-Dec-15 10:33
No Jobs running.
====

Terminated Jobs:
 JobId  Level    Files      Bytes   Status   Finished        Name 
====================================================================
     2  Full      1,643    25.53 M  OK       15-Dec-15 10:35 BackupDirector

====
```

Primeiro volume de Backup criado no diretório de backups.

```
[root@centos bacula]# cd backup/
[root@centos backup]# ls -la
total 32768
drwx------. 2 bacula bacula       19 Dez 15 10:33 .
drwx------. 4 bacula bacula       33 Dez 15 09:38 ..
-rw-r-----. 1 bacula tape   25794527 Dez 15 10:35 volume-backup
```

Restaurando Backup
------------------

Abrindo o bacula console.

```
bconsole
```

```
[root@centos backup]# bconsole
Connecting to Director 127.0.0.1:9101
1000 OK: bacula-dir Version: 5.2.13 (19 February 2013)
Enter a period to cancel a command.
*restore
Automatically selected Catalog: Catalogo
Using Catalog "Catalogo"

First you select one or more JobIds that contain files
to be restored. You will be presented several methods
of specifying the JobIds. Then you will be allowed to
select which files from those JobIds are to be restored.

To select the JobIds, you have the following choices:
     1: List last 20 Jobs run
     2: List Jobs where a given File is saved
     3: Enter list of comma separated JobIds to select
     4: Enter SQL list command
     5: Select the most recent backup for a client
     6: Select backup for a client before a specified time
     7: Enter a list of files to restore
     8: Enter a list of files to restore before a specified time
     9: Find the JobIds of the most recent backup for a client
    10: Find the JobIds for a backup for a client before a specified time
    11: Enter a list of directories to restore for found JobIds
    12: Select full restore to a specified Job date
    13: Cancel
Select item:  (1-13): 5
Automatically selected Client: bacula-fd
Automatically selected FileSet: Full Set
+-------+-------+----------+------------+---------------------+------------+
| JobId | Level | JobFiles | JobBytes   | StartTime           | VolumeName |
+-------+-------+----------+------------+---------------------+------------+
|     2 | F     |    1,643 | 25,531,312 | 2015-12-15 10:35:39 | volume-backup     |
+-------+-------+----------+------------+---------------------+------------+
You have selected the following JobId: 2

Building directory tree for JobId(s) 2 ...  +++++++++++++++++++++++++++++++++++++++++++++
1,495 files inserted into the tree.

You are now entering file selection mode where you add (mark) and
remove (unmark) files to be restored. No files are initially added, unless
you used the "all" keyword on the command line.
Enter "done" to leave this mode.

cwd is: /
$ ls
etc/
home
root/
usr/
var/
$ mark etc home root 
1,159 files marked.
$ done
Bootstrap records written to /var/spool/bacula/bacula-dir.restore.1.bsr

The job will require the following
   Volume(s)                 Storage(s)                SD Device(s)
===========================================================================
   
    volume                    File                      FileStorage              

Volumes marked with "*" are online.


1,159 files selected to be restored.

Run Restore job
JobName:         RestoreFiles
Bootstrap:       /var/spool/bacula/bacula-dir.restore.1.bsr
Where:           /bacula/restore
Replace:         always
FileSet:         Full Set
Backup Client:   bacula-fd
Restore Client:  bacula-fd
Storage:         File
When:            2015-12-15 10:42:14
Catalog:         Catalogo
Priority:        10
Plugin Options:  *None*
OK to run? (yes/mod/no): yes
Job queued. JobId=3
You have messages.
```

Verificando as Mensagens…

```
*m
15-Dec 10:42 bacula-dir JobId 3: Start Restore Job RestoreFiles.2015-12-15_10.42.17_05
15-Dec 10:42 bacula-dir JobId 3: Using Device "FileStorage" to read.
15-Dec 10:42 bacula-sd JobId 3: Ready to read from volume "volume-backup" on device "FileStorage" (/bacula/backup).
15-Dec 10:42 bacula-sd JobId 3: Forward spacing Volume "volume-backup" to file:block 0:191.
15-Dec 10:42 bacula-sd JobId 3: End of Volume at file 0 on device "FileStorage" (/bacula/backup), Volume "volume-backup"
15-Dec 10:42 bacula-sd JobId 3: End of all volumes.
15-Dec 10:42 bacula-dir JobId 3: Bacula bacula-dir 5.2.13 (19Jan13):
  Build OS:               x86_64-redhat-linux-gnu redhat (Core)
  JobId:                  3
  Job:                    RestoreFiles.2015-12-15_10.42.17_05
  Restore Client:         bacula-fd
  Start time:             15-Dec-2015 10:42:19
  End time:               15-Dec-2015 10:42:20
  Files Expected:         1,159
  Files Restored:         1,159
  Bytes Restored:         20,740,415
  Rate:                   20740.4 KB/s
  FD Errors:              0
  FD termination status:  OK
  SD termination status:  OK
  Termination:            Restore OK

15-Dec 10:42 bacula-dir JobId 3: Begin pruning Jobs older than 6 months .
15-Dec 10:42 bacula-dir JobId 3: No Jobs found to prune.
15-Dec 10:42 bacula-dir JobId 3: Begin pruning Files.
15-Dec 10:42 bacula-dir JobId 3: No Files found to prune.
15-Dec 10:42 bacula-dir JobId 3: End auto prune.
```

Obtendo os arquivos de Restores.

Abra o diretório de restauração.

```
[root@centos backup]# cd /bacula/restore/
[root@centos restore]# ls -l
total 12
drwxr-xr-x. 77 root root 4096 Dez 15 10:42 etc
drwxr-xr-x.  2 root root    6 Ago 12 11:22 home
dr-xr-x---.  2 root root 4096 Dez 15 10:31 root
```

Pronto! restore realizado com sucesso.

Nao irei abordar aqui a instalação de clientes no bacula, vou deixar isso para um outro tutorial.

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Valeu! e até a próxima!

- - - - - -

![](/site/assets/img/uploads/2017/11/foto-perfil-redondo-johnny.png)

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -