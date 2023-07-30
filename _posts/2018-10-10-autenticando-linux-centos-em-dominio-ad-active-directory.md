---
title: 'Autenticando Linux CentOS em Domínio AD Active Directory'
date: '2018-10-10T18:13:45-03:00'
type: post
image: /assets/img/thumb/autenticando-linux-centos-em-dominio-ad-active-directory.png
tag:
    - 'autenticando linux active directoy'
    - 'autenticando linux ad'
    - 'autenticando linux no windows'
    - 'autenticar centos ad'
    - 'autenticar linux samba'
    - 'autenticar linux samba4'
    - 'centos active directory'
    - 'centos ad'
    - 'centos autenticando active directory'
    - 'centos autenticando ad'
    - 'centos login ad'
    - 'login centos active directory'
---

- - - - - -

Olá, tudo bem? 😄

Aqui vai uma dica para facilitar o gerenciamento de acessos ao servidores Linux CentOS. Se você possui uma rede corporativa com domínio em base do Active Directory, seja ele Microsoft ou Samba4, esse artigo vai lhe ajudar a centralizar o acesso via SSH aos servidores linux.

Esse tutorial funciona em CentOS 6 e CentOS 7. 😎

Nosso cenário é o seguinte:

**Controlador de Domínio: (Samba4) 🐧**  
**Endereço IP:** 10.1.0.88/16  
**Domínio:** tidahora.local  
**OS:** CentOS 7.5

**Servidor Linux (Cliente) 🐧**  
**Endereço IP:** 10.1.1.89/16  
**Domínio:** tidahora.local  
**OS:** CentOS 7.5

Com usuário “root” vamos configurar nosso servidor linux (cliente) para autenticar usuários do AD.

Instale o repositório epel:

```
yum -y install epel-release
```

Instale os pacotes abaixo:

```
yum -y install adcli samba-common oddjob-mkhomedir oddjob samba-winbind-clients samba-winbind sssd pam_krb5 authconfig
```

Precisamos fazer uma ajuste no arquivo de resolução de nomes do Linux, é necessário para que o servidor faça a busca correta do domínio e não tenha problemas ao fazer a autenticação dos usuários.

```
vim /etc/resolv.conf
```

Conteúdo do arquivo:

```
search tidahora.local
domain tidahora.local
nameserver 10.1.0.88
```

Salve o arquivo do “resolv.conf’ e em seguida vamos bloquear o arquivo para edição, dessa forma se o servidor for reiniciado ou algum serviço tentar alterar o conteúdo do arquivo não vai ter a permissão.

```
chatt +i /etc/resolv.conf
```

Ajustando o ambiente para a autenticação via kerberos no domínio:

```
authconfig --enablekrb5 --krb5kdc=tidahora.local --krb5adminserver=tidahora.local --krb5realm=TIDAHORA.LOCAL --enablesssd --enablesssdauth --update
```

Obtendo informações do domínio:

```
adcli info tidahora.local
```

Retorno do comando acima, informações sobre o domínio, controlador, etc.

```
[domain]
domain-name = tidahora.local
domain-short = TIDAHORA
domain-forest = tidahora.local
domain-controller = samba4-dc01.tidahora.local
domain-controller-site = Default-First-Site-Name
domain-controller-flags = pdc gc ldap ds kdc timeserv closest writable good-timeserv full-secret
domain-controller-usable = yes
domain-controllers = samba4-dc01.tidahora.local
[computer]
computer-site = Default-First-Site-Name
```

Agora vamos ingressar o host ao domínio “tidahora.local”

```
adcli join tidahora.local -U administrator
Password for administrator@TIDAHORA.LOCAL: ****** <== Senha do usuário Administrator do domínio.
```

Precisamos criar o arquivo “sssd.conf”, responsável por fazer a autenticação em domínios com base LDAP/AD.

No arquivo que estamos criando abaixo, também informamos qual o bash dos usuários e o diretório home que os mesmos vão possuir, no meu caso, eu ajustei para criar uma pasta com o nome do domínio dentro do /home, dessa forma consigo gerenciar melhor os usuários locais, e usuários do domínio.

Se você quiser mudar isso, altere a linha onde contém o conteúdo de “fallback\_homedir”.

```
vim /etc/sssd/sssd.conf
```

Conteúdo do arquivo:

```
[sssd]
domains = tidahora.local
config_file_version = 2
services = nss, pam

[domain/tidahora.local]
ad_domain = tidahora.local
krb5_realm = TIDAHORA.LOCAL
realmd_tags = manages-system joined-with-samba
cache_credentials = True
id_provider = ad
krb5_store_password_if_offline = True
default_shell = /bin/bash
ldap_id_mapping = True
use_fully_qualified_names = False
fallback_homedir = /home/%d/%u
access_provider = ad
```

Ajustando a permissão ao arquivo:

```
chmod 600 /etc/sssd/sssd.conf
```

Na sequência vamos ajustar no PAM, opções de criação de diretórios homes.

Vamos ajustar primeiro o arquivo “/etc/pam.d/system-auth”

```
vim /etc/pam.d/system-auth
```

No final do arquivo insira a linha abaixo:

```
session     optional      pam_mkhomedir.so skel=/etc/skel umask=077
```

Agora vamos editar o arquivo responsável por criar a sessão (home) dos usuários quando eles conectam-se via SSH.

```
vim /etc/pam.d/sshd
```

No final do arquivo insira a linha abaixo:

```
session required pam_mkhomedir.so skel=/etc/skel/ umask=0022
```

Reiniciando e ajustando os serviços para iniciarem junto ao boot do sistema:

Se você estiver utilizando CentOS 6:

```
service sssd start && chkconfig sssd on && service winbind start && chkconfig winbind on
```

Se for CentOS 7:

```
systemctl restart sssd && systemctl enable sssd && systemctl restart winbind.service && systemctl enable winbind.service
```

Configurando o SSH para permitir grupos a conectarem via SSH:

Nesse caso estarei liberando o grupo “ti” para conexão via SSH ao host.

```
vim /etc/ssh/sshd_config
```

No final do arquivo insira a linha abaixo:

```
AllowGroups ti
```

Agora vamos ajustar a permissão de sudo para os usuários.

```
visudo
```

Na linha de número 93, insira a linha abaixo, salve o arquivo e saia do mesmo

```
%ti     ALL=(ALL)       ALL
```

Agora tente se logar com um usuário do seu domínio que pertença ao grupo que ajustamos no sudo e no ssh.

Meu usuário do domínio é “johnny” ele é membro do domínio “ti”, que configuramos acima:  
[![](/site/assets/img/uploads/2018/10/1.png)](/site/assets/img/uploads/2018/10/1.png)

Informe a senha do usuário do domínio:  
[![](/site/assets/img/uploads/2018/10/2.png)](/site/assets/img/uploads/2018/10/2.png)

Veja abaixo o diretório /home do usuário, o mesmo está configurado para /home/tidahora.local.  
[![](/site/assets/img/uploads/2018/10/3.png)](/site/assets/img/uploads/2018/10/3.png)

O mesmo utilizando o comando “sudo” para executar comandos com nível de permissão avançada.  
[![](/site/assets/img/uploads/2018/10/5.png)](/site/assets/img/uploads/2018/10/5.png)


Dúvidas, comentário e sugestões postem nos comentários.
👋🏼 Valeu! e até a próxima!  

- - - - - -

![](/site/assets/img/uploads/2017/11/foto-perfil-redondo-johnny.png)  

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -