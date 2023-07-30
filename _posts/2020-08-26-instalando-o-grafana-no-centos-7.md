---
title: 'Instalando o Grafana no CentOS 7'
date: '2020-08-26T14:43:49-03:00'
type: post
image: /assets/img/thumb/grafana-dashboard-e1605992643650.png
tag:
    - 'grafana dashboard centos'
    - 'instalando grafana centos'
    - 'instalando grafana centos 7'
    - 'instalando grafana linux'
    - 'instalando o grafana no centos'
    - 'instalando o grafana no centos 7'
---

- - - - - -

Fala povo, bão? Tutorial de hoje mostra como instalar o poderoso Grafana no CentOS 7.

O Grafana é uma aplicação web de análise de código aberto multiplataforma e visualização interativa da web. Ele fornece tabelas, gráficos e alertas para a Web quando conectado a fontes de dados suportadas. É expansível através de um sistema de plug-in.

Fonte:[ Wikipédia](https://pt.wikipedia.org/wiki/Grafana)


##### Passo 1: Desabilitando o SELinux

Para desabilitar o SELinux é simples, basta digitar o comando abaixo: 

```
setenforce 0
```

Em seguida precisamos alterar no arquivo de configuração do mesmo, para que quando o servidor for reiniciado, o SELinux volte desabilitado por padrão. 

```
vim /etc/selinux/config
```

Altere a linha 7 que corresponde a SELINUX=enforcing para SELINUX=disabled 

```
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=disabled
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```

*Caso queira deixar o SELinux habilitado fica a seu critério ajusta-lo.* Após essas alterações, reinicie o servidor para efetivar a alteração do SELinux. 

```
reboot
```


##### Passo 2: Configurando o repositório do Grafana

O primeiro passo para instalação do Grafana é criar o repositório do mesmo: 

```
vim /etc/yum.repos.d/grafana.repo
```

Copie o conteúdo abaixo e cole no seu grafana.repo: 

```
[grafana]
name=grafana
baseurl=https://packages.grafana.com/oss/rpm
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
```

Salve o arquivo acima. 


##### Passo 3: Instalando o Grafana

Agora vamos instalar o grafana: 

```
yum -y install grafana
```

Precisamos instalar algumas dependências para o Grafana funcionar de maneira adequada também. 

```
yum -y install fontconfig urw-fonts freetype*
```

Em seguida vamos habilitar o serviço do Grafana, assim se o server for reiniciado, o serviço não precisa ser iniciado na mão, irá startar automaticamente junto ao boot do S.O. 

```
systemctl enable grafana-server
```

Temos que iniciar o serviço do Grafana em nosso Linux CentOS 7 também: 

```
systemctl start grafana-server
```

Verificando o status do serviço: 

```
systemctl status grafana-server
```

Retorno do comando acima: 

```
● grafana-server.service - Grafana instance
   Loaded: loaded (/usr/lib/systemd/system/grafana-server.service; enabled; vendor preset: disabled)
   Active: active (running) since Qua 2020-08-26 14:22:43 -03; 21s ago
     Docs: http://docs.grafana.org
 Main PID: 9541 (grafana-server)
   CGroup: /system.slice/grafana-server.service
           └─9541 /usr/sbin/grafana-server --config=/etc/grafana/grafana.ini --pidfile=/var/run/grafana/grafana-server.pid
```

  
##### Passo 4: Ajustando o Firewall

Precisamos efetuar a liberação da porta 3000/TCP que é onde o Grafana irá responder as conexões de acesso web. 

```
firewall-cmd --zone=public --add-port=3000/tcp --permanent
```

Se o comando acima for executado com sucesso, vai receber no console um “success” Em seguida, recarregue as configurações de Firewall com o comando abaixo, se tudo ocorrer bem, também vai receber um “success”. 

```
firewall-cmd --reload
```

  
##### Passo 5: Acessando o Grafana pelo navegador

Abra o seu navegador de preferência e digite o endereço IP do servidor seguido da porta 3000, por exemplo: http://endereco ip:3000 

[![](/site/assets/img/uploads/2020/08/grafana-1.png)](/site/assets/img/uploads/2020/08/grafana-1.png)

Para efetuar o acesso pela primeira vez, o login é: admin e a senha também é: admin 
**Login:** admin 
**Senha:** admin 

Ao efetuar o login pela primeira vez, será solicitado a troca da senha padrão, conforme a tela abaixo, informa uma nova senha, confirme-a e clique em **Submit** para validar a alteração. 

[![](/site/assets/img/uploads/2020/08/grafana-2.png)](/site/assets/img/uploads/2020/08/grafana-2.png)

Após isso, você será redirecionado a tela principal do Grafana. 

[![](/site/assets/img/uploads/2020/08/grafana-3.png)](/site/assets/img/uploads/2020/08/grafana-3.png)


  
##### Passo 6: Instalando Plugin do Zabbix no Grafana

Para que possamos ter ainda mais recursos no Grafana, é possível realizar a instalação de plugins, esses plugins facilitam a configuração e criação de Dashboards de serviços de monitoramento, como por exemplo, o plugin do Zabbix. 

```
grafana-cli plugins install alexanderzobnin-zabbix-app
```

É necessário reiniciar o serviço do Grafana. 

```
systemctl restart grafana-server
```

 Abaixo listo o site de documentação oficial do Grafana que pode lhe ajudar a criar mais dashboards, ou resolver problemas com o Grafana. 
 <https://grafana.com/docs/grafana/latest/> 

 Dúvidas, comentário e sugestões postem nos comentários… 

 👋🏼 Até a próxima!

- - - - - -

![](/site/assets/img/uploads/2019/02/foto-redonda.png)  

**Johnny Ferreira** 
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>  