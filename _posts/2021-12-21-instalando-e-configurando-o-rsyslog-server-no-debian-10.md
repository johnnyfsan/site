---
title: 'Instalando e Configurando o Rsyslog Server no Debian 10'
date: '2021-12-21'
image: ../assets/img/uploads/2021/12/rsyslog-features-imagemap.png
category:
    - 'Debian'
    - 'Rsyslog'
tag:
    - 'configurando rsyslog server'
    - 'instalando e configurando o rsyslog'
    - 'instalando e configurando o rsyslog server'
    - 'instalando rsyslog'
    - 'instalando servidor de logs com rsyslog'
    - 'instalar rsyslog debian'
    - 'rsyslog debian install'
    - 'servidor de logs com rsyslog linux'
    - 'servidor de logs no debian com rsyslog'

---

- - - - - -


**Objetivo:** Realizara instalação de um servidor de Logs com o Rsyslog no Debian 10.

O primeiro passo de qualquer instalação no sistema Linux é verificar se o ambiente está atualizado, para isso no Debian execute os comandos abaixo:

```
apt update
apt upgrade
```

Caso o seu ambiente precise de atualizações, basta confirma para atualizar os pacotes necessários.

Agora que o ambiente já está pronto, vamos iniciar a instalação do Rsyslog:

```
apt install rsyslog
```

Confirme a instalação pressionando a tecla S se o seu sistema está em português, ou Y se estiver em Inglês 🙂

Após a instalação, vamos seguir com a configuração do Rsyslog no Debian.  
Mas antes de colocarmos a mão na massa, vamos gerar um backup do arquivo de configuração.

Execute o comando abaixo para criar um arquivo de cópia.

```
cp -Rfa /etc/rsyslog.conf{,.bkp}
```

Vamos para a edição do arquivo de configuração, no meu caso estou utilizando o VIM como editor de textos, fique a vontade para usar o de sua preferência.

```
vim /etc/rsyslog.conf
```

Em seguida vamos precisar editar o arquivo de configuração do Rsyslog, e remover as linhas que iniciam com ‘#’ conforme abaixo:

```
# provides UDP syslog reception
#module(load="imudp")
#input(type="imudp" port="514")

# provides TCP syslog reception
#module(load="imtcp")
#input(type="imtcp" port="514")
```

Altera para ficar conforme abaixo:

```
# provides UDP syslog reception
module(load="imudp")
input(type="imudp" port="514")

# provides TCP syslog reception
module(load="imtcp")
input(type="imtcp" port="514")
```

Adicione no arquivo as seguintes linhas abaixo:

```
$AllowedSender TCP, 127.0.0.1, 10.1.0.0/24, 10.1.1.0/24
```

**Não esqueça de ajustar conforme sua rede.***

Salve o arquivo e saia do modo de edição após os ajustes.

Após isso, basta ajustar o serviço do Rsyslog conforme sua necessidade, para iniciar junto ao boot do S.O, etc:

```
systemctl start rsyslog
systemctl status rsyslog
systemctl enable rsyslog
```

Em seguida, basta configurar os clientes para enviar seus logs via syslog ao servidor Rsyslog 🙂


Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Valeu! e até a próxima!

- - - - - -

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -