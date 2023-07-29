---
layout: post
title: Como Funciona o IAM na AWS
subtitle:
image: /site/assets/img/aws-iam.png
share-img: /site/assets/img/aws-iam.png
tags: [aws, iam, devops, cloud, amazon web services]
comments: true
---

- - - - - -

# Como Funciona o IAM na AWS

Olá pessoal, hoje vamos falar um pouco sobre o **IAM** na [Amazon Web Services.](https://aws.amazon.com/)
O **IAM** (Identity and Access Management) na AWS (Amazon Web Services) é um serviço que permite que você gerencie o acesso dos usuários e recursos aos serviços da AWS. O IAM ajuda a garantir que apenas as pessoas certas tenham acesso aos recursos certos na hora certa. Neste artigo, vamos explorar como o IAM funciona na AWS.

## Conceitos básicos do IAM

Antes de mergulharmos nos detalhes do **IAM**, vamos revisar alguns conceitos básicos:

- **Usuários:** uma entidade que representa uma pessoa ou um serviço que interage com a AWS.
- **Funções:** uma entidade que define permissões para acessar recursos específicos na AWS. As funções são usadas para conceder as permissões temporárias para usuários ou serviços que precisam acessar recursos da AWS.
- **Políticas:** um documento que define as permissões que são concedidas aos usuários, grupos ou funções. As políticas podem ser anexadas a usuários, grupos ou funções e definem quais ações são permitidas ou negadas em recursos específicos da AWS.

## Como funciona o IAM

O **IAM** funciona com base em três elementos principais: usuários, grupos e políticas. Primeiro, você cria usuários e grupos no IAM. Em seguida, você adiciona políticas a esses usuários e grupos para definir quais recursos eles podem acessar e que ações podem realizar nesses recursos. Por fim, você concede ou revoga acesso aos recursos da AWS, adicionando ou removendo usuários dos grupos.

## Criando Usuários e Grupos

Para começar, você precisa criar usuários e grupos no IAM. Você pode criar um usuário para cada pessoa ou serviço que precisa acessar a AWS. Os usuários têm credenciais de login (nome de usuário e senha) que podem ser usadas para acessar a AWS. Para criar um usuário, você precisa especificar um nome de usuário, um ID e uma senha para o usuário.

Os grupos são usados para agrupar usuários que têm as mesmas permissões. Por exemplo, você pode criar um grupo chamado **"Administradores"** e adicionar usuários a esse grupo que têm permissão para gerenciar todos os recursos na AWS. Para criar um grupo, você precisa especificar um nome para o grupo.

## Adicionando Políticas

Depois de criar usuários e grupos, você precisa adicionar políticas a esses usuários e grupos para definir quais recursos eles podem acessar e que ações podem realizar nesses recursos. As políticas são documentos JSON que definem as permissões que são concedidas aos usuários, grupos ou funções.

## Existem duas maneiras de criar políticas no IAM:

- Criando políticas personalizadas: você pode criar suas próprias políticas personalizadas para definir permissões específicas.
Usando políticas gerenciadas: a AWS fornece políticas gerenciadas que você pode usar para definir permissões comuns. As políticas gerenciadas são mantidas pela AWS e são atualizadas automaticamente para refletir as alterações nos serviços da AWS.
Concedendo ou Revogando Acesso

- Depois de criar usuários, grupos e políticas, você pode conceder ou revogar acesso aos recursos da AWS. Você pode fazer isso adicionando ou removendo usuários dos grupos. Por exemplo, se você quiser conceder acesso aos recursos da AWS para um novo funcionário, basta adicioná-lo ao grupo apropriado que já possui as políticas necessárias para acessar esses recursos. Se um funcionário sair da empresa, basta removê-lo do grupo para revogar seu acesso.

## Exemplos de uso do IAM

Aqui estão alguns exemplos de como o IAM pode ser usado na AWS:

- Gerenciamento de equipe: você pode criar grupos para diferentes funções, como desenvolvedores, administradores de banco de dados e equipes de segurança, e adicionar usuários a esses grupos com base em suas responsabilidades. Isso ajuda a garantir que cada pessoa tenha acesso apenas aos recursos relevantes para seu trabalho.
- Controle de acesso de terceiros: você pode criar usuários temporários ou criar funções com políticas temporárias para dar acesso a terceiros, como consultores ou parceiros de negócios, a recursos específicos da AWS. Isso ajuda a garantir que eles tenham acesso apenas aos recursos necessários para concluir seu trabalho e que o acesso seja revogado automaticamente após um determinado período de tempo.
- Gerenciamento de acesso de aplicativos: você pode criar funções com políticas específicas para conceder permissões a aplicativos ou serviços que precisam acessar recursos da AWS. Isso ajuda a garantir que os aplicativos ou serviços tenham acesso apenas aos recursos necessários para funcionar corretamente.

## Conclusão

O **IAM** é um serviço essencial para gerenciar o acesso aos recursos da AWS. Ele ajuda a garantir que apenas as pessoas certas tenham acesso aos recursos certos na hora certa. Com o IAM, você pode criar usuários e grupos, adicionar políticas para definir permissões e conceder ou revogar acesso aos recursos da AWS. Além disso, o IAM oferece uma variedade de recursos avançados, como MFA (autenticação de dois fatores), gerenciamento de chaves e políticas de condições para garantir a segurança e o controle de acesso aos recursos da AWS.
