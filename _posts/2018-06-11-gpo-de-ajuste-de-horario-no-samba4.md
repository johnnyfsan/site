---
title: 'GPO de Ajuste de Horário no SAMBA4'
date: '2018-06-11T16:56:03-03:00'
type: post
image: /assets/img/uploads/2018/06/gpo-para-ajuste-de-horario-no-samba4.png
share-img: /assets/img/uploads/2018/06/gpo-para-ajuste-de-horario-no-samba4.png
tag:
    - 'criar gpo samba4 linux'
    - 'criar gpo windows horario samba'
    - 'gpo horario windows samba'
    - 'gpo ntp samba'
    - 'gpo ntp samba4'
    - 'gpo para ajustar horario windows no samba4'
    - 'gpo samba4'
    - 'gpo samba4 horario'
    - 'gpo smb4 horario'
    - 'ntp gpo'
---

- - - - - -

Opa, tudo bem? 😀  
Hoje vamos ver como aplicar uma GPO para corrigir o horário das estações Windows, em uma rede onde o controlador de domínio é baseado em Samba4. Eu estava tendo problemas com a sincronia de horários em hosts onde o sistema operacional é Windows 8 e Windows 10, para isso fui atras de uma solução que eu pudesse aplicar a todos os computadores do domínio, e vou compartilhar com você nesse post.

Não sei o motivo de não funcionar a sincronia de horário entre cliente/servidor com clientes Windows 8 e Windows 10 e controlador de domínio Samba4, talvez mais alguma jogada da Microsoft para cima do Software Livre? Não sabemos, é fato que muitas funcionalidades deixam de funcionar em novas versões e atualizações do windows.

Um problema tão comum, mas tão importante para o funcionamento correto de uma rede de computadores, a sincronia de data e hora é fundamental para funcionamento de alguns softwares, sejam eles para emissão de notas fiscais, conectar com bando de dados, acessar determinada aplicação bancaria via web, enfim, a lista é vasta.

Para solucionar esse “bug” de forma rápida e escalada, vamos precisar criar duas GPO’s, uma será aplicada na diretiva local dos computadores e outra será aplicada na configuração dos usuários do domínio.

Criando a Primeira GPO
----------------------

A primeira **GPO** que precisamos criar, é a **GPO** de dar a permissão para os usuários do domínio consigam alterar a hora do sistema.  
Essa **GPO** é criada nas diretivas de computadores.  
Abra o **“Gerenciamento de Diretiva de Grupo”**, localizado em **Ferramentas Administrativas** no **Controlador de Domínio.**  
Localize a **OU** de Computadores do domínio.  

![](/assets/img/uploads/2018/06/gpo-horario-1.png)  

Clique com o botão direito do mouse sobre a **OU** de Computadores, e vamos criar uma nova GPO para dar a permissão para que usuários do domínio consigam alterar a data e hora do sistema Windows.

Abaixo crie um nome para sua GPO, lembrando que teremos que ter duas GPOs para aplicar essa permissão de ajuste de hora e data, eu vou criar uma GPO chamado **Atualizar Horario Windows – PC**, para saber que o **PC** no final da GPO corresponde a GPO que está vinculada a **OU** de computadores do domínio.

![](/assets/img/uploads/2018/06/gpo-horario-2.png)  

Abra a **OU** conforme abaixo, e clique com o botão direito do mouse e vamos editar nossa GPO.

![](/assets/img/uploads/2018/06/gpo-horario-3.png)  
 
Agora siga o caminho abaixo:

**Configurações de Computador** » **Configurações do Windows** » **Configurações de Segurança** » **Diretivas locais** » **Atribuição de direitos de usuário**

![](/assets/img/uploads/2018/06/gpo-horario-4.png)  

Agora vamos abrir a opção **Alterar a hora do sistema**:

![](/assets/img/uploads/2018/06/gpo-horario-5.png)  
  
Vamos editar a opção **Alterar a hora do sistema** e incluir o grupo **Domain Users**:

Marque a opção **Definir estas configurações de diretivas**:

![](/assets/img/uploads/2018/06/gpo-horario-6.png)  

Insira o grupo de segurança **Domain Users** ou **Usuário de Domínio**:

![](/assets/img/uploads/2018/06/gpo-horario-7.png)  

Agora clique em **OK** para confirmar as alterações:

![](/assets/img/uploads/2018/06/gpo-horario-8.png)

Pronto, efetuamos a permissão em diretiva local para que todos os usuários do domínio possam ter permissão de ajuste na hora e data do sistema. Em ambientes Microsoft Windows Server esse erro não ocorre, somente com o Controladores de Domínio baseados em Samba.

![](/assets/img/uploads/2018/06/gpo-horario-9.png)



Criando a Segunda GPO
---------------------

Agora vamos criar a segunda GPO, responsável pelo ajuste transparente na hora do Logon do usuário.

Lembrando que essa GPO, vai ficar centralizar na **OU** de Usuários do domínio.

![](/assets/img/uploads/2018/06/gpo-horario-10.png)

Clique com o botão direito do mouse e selecione a opção **Criar um GPO neste domínio e fornecer um link para ele aqui…**

![](/assets/img/uploads/2018/06/gpo-horario-11.png)

Informe o nome da GPO, eu irei usar o mesmo padrão da GPO que fizemos acima, para dar permissão na diretiva local do computador, porém agora vou especificar **User** no final, para saber que essa GPO, corresponde a **OU** de usuários do domínios.

![](/assets/img/uploads/2018/06/gpo-horario-12.png)

A seguir, vamos para o mesmo procedimento de edição da GPO.

![](/assets/img/uploads/2018/06/gpo-horario-13.png)

Abra o caminho marcado, conforme abaixo:

**Configurações do Usuário** » **Diretivas** » **Configurações do Windows** » **Scripts(Logon/Logoff)**

![](/assets/img/uploads/2018/06/gpo-horario-14.png)

Agora vamos abrir a opção **Logon**, e vamos clicar em **Mostrar Arquivo…**, conforme imagem:

![](/assets/img/uploads/2018/06/gpo-horario-15.png)

Vai abrir o mapeamento da GPO dentro da pasta Sysvol.

![](/assets/img/uploads/2018/06/gpo-horario-16.png)

Aqui nessa pasta vamos criar um script .bat para definir o horário correto nas estações de trabalho assim que os usuários fizerem logon.

Crie um novo arquivo .bat no diretório da GPO, defina um nome para o mesmo, eu defini “sincroniza-horario.bat”.

O conteúdo do arquivo deve ser o seguinte:

```
@echo off
net time \\10.1.0.6 /set /yes
```

*Troque o IP 10.1.0.6 para o IP do seu controlador de domínio.*

![](/assets/img/uploads/2018/06/gpo-horario-17.png)

Agora clique em **Adicionar**:

![](/assets/img/uploads/2018/06/gpo-horario-18.png)

Clique em **Procurar**:

![](/assets/img/uploads/2018/06/gpo-horario-19.png)

Selecionando o arquivo…

![](/assets/img/uploads/2018/06/gpo-horario-20.png)

Confirmando o arquivo selecionado.

![](/assets/img/uploads/2018/06/gpo-horario-21.png)

Confirmando a configuração da GPO.

![](/assets/img/uploads/2018/06/gpo-horario-22.png)

Visualizando o relatório da GPO criada.

![](/assets/img/uploads/2018/06/gpo-horario-23.png)

Agora basta reiniciar as estações de trabalho para que peguem as configurações da GPO de horário.

Após fazer o reboot e logon nas maquinas rode o comando abaixo para verificar se a GPO foi aplicada.

Abra o **prompt de comandos** do Windows e execute o comando abaixo:

```
gpresult /h relatorio-gpo.html
```

![](/assets/img/uploads/2018/06/gpo-horario-24.png)

Abra o diretório “home” do usuário e veja se gerou o arquivo.

![](/assets/img/uploads/2018/06/gpo-horario-25.png)

Agora vamos abri-lo com duplo click sobre o mesmo.  
Veja abaixo, GPO aplicada com sucesso!

![](/assets/img/uploads/2018/06/gpo-horario-26.png)

Bem, é isso, espero te ajudar de alguma forma com esse post.

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Valeu! e até a próxima!

- - - - - -

![](/assets/img/uploads/2017/11/foto-perfil-redondo-johnny.png)  

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -