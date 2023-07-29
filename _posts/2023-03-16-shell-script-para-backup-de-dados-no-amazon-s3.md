---
title: 'Shell script para backup de dados no Amazon S3'
date: '2023-03-16T16:53:03-03:00'
type: post
image: /site/site/assets/img/uploads/2023/03/shell-script-backup-amazon-s3.png
share-img: /site/site/assets/img/uploads/2023/03/shell-script-backup-amazon-s3.png
tag:
    - 'Amazon S3'
    - 'backup linux shell script s3 aws'
    - 'shell script aws s3'
    - 'shell script backup s3 aws'
---

- - - - - -

Tutorial de exemplo para criar um shell script em Linux para fazer backup de dados no Amazon S3.

Considerando que você esteja utilizando um Ubuntu Linux, execute os comandos abaixo.

O primeiro passo é instalar o AWS CLI em sua máquina/servidor:

```
sudo apt-get install awscli
```

Agora que já temos o ‘awscli’ instalado, vamos configurar as credenciais no seu ambiente linux.

Para configurar suas credenciais do AWS CLI rode o comando:

```
aws configure
```

Na sequência, crie um arquivo de shell script, com a extensão .sh, escolha um nome para o seu script, por exemplo: **backup\_para\_s3.sh**.

Dentro do arquivo de script, adicione o seguinte código:  

```
#!/bin/bash
# VARIAVEIS #
S3_BUCKET="seu_bucket_s3"
BACKUP_DIR="/caminho/do/diretorio/de/backup"
NOW=$(date +"%Y-%m-%d_%H-%M-%S")
BACKUP_FILE="$BACKUP_DIR/backup_$NOW.tar.gz"

# CRIANDO O ARQUIVO DE BACKUP COM EXTENSAO TAR.GZ #
tar -czvf "$BACKUP_FILE" /caminho/do/diretorio/a/ser/backupado

# FAZ O UPLOAD PARA O AWS S3 #
aws s3 cp "$BACKUP_FILE" "s3://$S3_BUCKET/backup_$NOW.tar.gz"
```

Explicando o script:  

- Substitua **seu\_bucket\_s3** pelo nome do seu bucket do Amazon S3.  
- Substitua /caminho/do/diretorio/de/backup pelo caminho para o diretório onde deseja armazenar os arquivos de backup.  
- Substitua /caminho/do/diretorio/a/ser/backupado pelo caminho para o diretório que deseja fazer o backup.  

Salve o arquivo.

Depois disso, é preciso fornecer a permissão de execução para o arquivo de script:

```
chmod +x backup_para_s3.sh
```

Para executar o script, rode o comando:

```
./backup_para_s3.sh
```

O script irá criar um arquivo de backup do diretório especificado e enviá-lo para o seu bucket no Amazon S3 com um nome único baseado na data e hora atual.

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Valeu! e até a próxima!

- - - - - -

![](/site/site/assets/img/uploads/2017/11/foto-perfil-redondo-johnny.png)

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -