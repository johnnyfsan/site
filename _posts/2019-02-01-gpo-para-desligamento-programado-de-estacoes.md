---
title: 'GPO para Desligamento Programado de Estações'
date: '2019-02-01T14:59:46-02:00'
type: post
image:../assets/img/thumb/gpo-para-desligamento-programado-de-estacoes.png
tag:
    - 'gpo desligar computador'
    - 'gpo shutdown'
    - 'gpo desligamento programado'
---

- - - - - -

*GPO para realizar o desligamento automatizado e agendado das estações/computadores em rede local.*

Fala galera, beleza?

Objetivo desse post é documentar aqui uma GPO que precisei criar na rede que administro. Visando em economizar energia e reduzir custos, notei que bastante colaboradores “esqueciam” seus computadores/notebooks ligados depois do expediente.

Foi então que comecei a buscar soluções para tratar esse pequeno problema, e uma das formas que consegui fazer com facilidade e de rápida implementação, foi através de GPO em meu domínio.

Siga os passos abaixo, caso precise criar essa regra em seu controlador de domínio também.

Primeiro passo é abrir o **Gerenciador de Política de Grupo.**

![](../assets/img/uploads/2019/02/image.png)  

![](../assets/img/uploads/2019/02/image-1.png)  

Clique com o botão direito do Mouse na **OU** que deseja criar a GPO, e clique em **Criar um GPO neste domínio e fornecer um link para ele aqui…**

Eu selecionei a “**OU**” de **Computadores** dentro da “**OU**” do meu **domínio**:

![](../assets/img/uploads/2019/02/image-3.png)

Informe o nome desejado para a **GPO**:

**Nome Inserido:** Desligamento Programado de Computadores.

Após criarmos a GPO, vamos edita-la, clique com o botão direito na GPO criada. Teremos a tela abaixo:

Configurações do Computador » Preferências » Configurações do Painel de Controle » Tarefas Agendadas

Clique com o botão direito em Tarefa Agendada e selecione a opção Tarefa Agendada (no mínimo Windows 7)

![](../assets/img/uploads/2019/02/Screen-Shot-2020-05-03-at-7.50.02-PM-1024x523.png)

Preencha os campos conforme as imagens abaixo:  
Aba **Geral**:

![](../assets/img/uploads/2019/02/Screen-Shot-2019-02-01-at-14.02.43.png)

Aba **Disparadores**, clique em Nova… e faça o cadastro das informações abaixo:

![](../assets/img/uploads/2020/05/disparadores.png)

Aba **Ações**:

![](../assets/img/uploads/2020/05/acoes.png)

Aba **Condições**:

![](../assets/img/uploads/2020/05/condicoes.png)

Aba **Configurações**:

![](../assets/img/uploads/2020/05/configuracoes.png)

Aba **Comum**:

![](../assets/img/uploads/2020/05/comum.png)

Para aplicar a GPO nos computadores, vincule a GPO criada na **OU** de **Computadores** e nos **setores** correspondentes:

![](../assets/img/uploads/2019/02/image-4.png)

Após isso você pode esperar replicar as GPOs para as estações, ou executar o comando **gpupdate /force** para forçar a atualizar.

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Até a próxima!

- - - - - -

![](../assets/img/uploads/2019/02/foto-redonda.png)  

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>