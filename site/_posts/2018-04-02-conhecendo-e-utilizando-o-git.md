---
title: 'Conhecendo e Utilizando o GIT'
date: '2018-04-02T13:59:13-03:00'
type: post
image: /assets/img/thumb/conhecendo-e-utilizando-o-git.png
tag:
    - 'aprenda git'
    - 'aprender usar o git'
    - 'comandos basicos git'
    - 'git basico'
    - 'git centos'
    - 'git debian'
    - 'git linux'
    - 'git para iniciantes'
    - 'iniciando no git'
---

- - - - - -

Opa, tudo bem? 🙂 Hoje vamos estar Conhecendo e Utilizando o GIT em ambientes Linux.  
O GIT é um sistema de controle de versão distribuído, que atua também como um sistema de gerenciamento de código fonte para milhares de desenvolvedores no mundo a fora.  
O Projeto foi desenvolvido por Linus Torvalds para o desenvolvimento do poderoso Kernel do sistema Linux. Hoje é utilizado em muitos projetos.

#### Conhecendo e Utilizando o GIT foi criado para auxiliar no processo inicial de uso da ferramenta GIT

Sua licença está sob a GPL 2.

#### UTILIZANDO O GIT EM AMBIENTE LINUX

Instalando o GIT em ambientes CentOS:

```
yum install git-core
```

Instalando o GIT em ambientes Baseados em Debian:

```
apt-get install git
```

Você pode também consulta o Site Oficial do GIT para fazer o Download: <http://git-scm.com/downloads>

#### ENTENDENDO COMO O GIT FUNCIONA

O GIT mantém três árvores que são responsáveis pelo fluxo de trabalho.

**WORKING DIRECTORY**  
Responsável por armazenar e controlar os arquivos vigentes.

**INDEX**  
Atua e funciona como uma espécie de área temporária.

**HEAD**  
Essa árvore tem a função de apontar para o ultimo commit que realizamos.

#### CONFIGURANDO USUARIO E O E-MAIL DO GITHUB

```
git config --global user.name usuario
git config --global user.email usuario@email.com
```

#### CONFIGURANDO O EDITOR DE TEXTO PADRÃO

```
git config --global core.editor vim
```

Definimos também o editor que faz o DIFF dos arquivos, no meu caso estou utilizando vimdiff.

```
git config --global merge.tool vimdiff
```

#### VERIFICANDO AS CONFIGURAÇÕES DO GIT

```
git config --list
```

```
user.name=usuario
user.email=usuario@email.com
core.editor=vim
merge.tool=vimdiff
```

#### INICIANDO UM REPOSITÓRIO

Vamos agora iniciar um repositório local com o GIT.

```
cd /root/
mkdir git
cd git
git init
Initialized empty Git repository in /root/git/.git/
```

Observe que foi criado em /root/git/ um diretório oculto, dentro dele temos a seguinte estrutura.

branches config description HEAD hooks info objects refs

#### CRIE UMA CONTA NO GITHUB

<http://github.com>

Apos ter criado a conta, crie um novo repositório lá no site do GITHUB.

E vamos sincronizar nosso diretorio remoto.

```
git remote add origin https://github.com/seu-usuario/git.git
```

Criamos agora nossos arquivos no diretório do git:  
Vou criar um exemplo com nome de algumas distros de linux.

```
touch centos debian redhat fedora ubuntu
```

Agora vamos criar um arquivo chamado LEIAME.

```
vim LEIAME
```

conteúdo:

```
ESTE ARQUIVO EH UM TESTE UTILIZANDO O GIT
```

#### ADD

Com o ‘git add’ vamos adicionar os arquivos desejados para o monitoramento do GIT, ele vai ficar monitorando as alterações nos arquivos adicionados.

No exemplo abaixo, estou adicionando todos os meus arquivos do diretório que criamos acima.

```
git add *
```

#### COMMIT

Para que essas alterações sejam validadas, precisamos realizar o famoso ‘commit’.

```
git commit -m "MEU PRIMEIRO COMMIT USANDO O GIT :D"
[master (root-commit) 42b33ee] MEU PRIMEIRO COMMIT USANDO O GIT :D
1 files changed, 1 insertions(+), 0 deletions(-)
create mode 100644 LEIAME
create mode 100644 centos
create mode 100644 debian
create mode 100644 fedora
create mode 100644 redhat
create mode 100644 ubuntu
```

Após realizarmos o nosso ‘commit’, temos nossos arquivos dentro do nosso GIT Directory. Mas precisamos enviar isso para o host remoto, no nosso caso o GIT HUB.

#### PUSH

Para que possamos inserir as modificações e commit’s em nosso host remoto, utilizamos o comando:

```
git push origin master
```

```
Username for 'https://github.com': seu-usuario
Password for 'https://seu-usuario@github.com':
Counting objects: 4, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (4/4), 347 bytes, done.
Total 4 (delta 0), reused 0 (delta 0)
To https://github.com/seu-usuario/git.git
* [new branch] master -> master
```

Agora já temos os arquivos lá no site do GIT HUB. (Conhecendo e Utilizando o GIT)

#### CLONE

Para clonar um repositório ou até mesmo contribuir para algum projeto em desenvolvimento utilizamos o comando ‘git clone’.

Sintaxe do comando fica assim:

```
git clone https://github.com/seu-usuario/git.git
```

ou

```
git clone https://url-do-projeto
```

Observação: A URL você obtém na página do projeto desejado lá no GIT HUB. (Conhecendo e Utilizando o GIT)

#### STATUS

Quando for realizado alguma alteração em algum arquivo, utilizamos o comando abaixo:

Faça alguma alteração em algum arquivo.

Por exemplo, eu realizei no arquivo LEIAME.

Após realizar a alteração digite:

```
git status
```

```
# On branch master
# Changes not staged for commit:
# (use "git add <file>..." to update what will be committed)
# (use "git checkout -- <file>..." to discard changes in working directory)
#
# modified: LEIAME
#no changes added to commit (use "git add" and/or "git commit -a")
```

Vamos atualizar nossos arquivos…

```
git add *
```

Commit…

```
git commit -m "ADICIONEI ALTERACAO NO ARQUIVO LEIAME"
[master 9cbaaad] ADICIONEI ALTERACAO NO ARQUIVO LEIAME
1 file changed, 6 insertions(+)
```

Push…

```
git push origin master
Username for 'https://github.com': seu-usuario
Password for 'https://seu-usuario@github.com':
Counting objects: 5, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 365 bytes, done.
Total 3 (delta 1), reused 0 (delta 0)
To https://github.com/seu-usuario/git.git
4074cc5..9cbaaad master -> master
```

E na sequência já temos o arquivo alterado lá no GIT HUB.

Este foi apenas um pequeno exemplo, um simples exemplo de como estar utilizando o GIT em projetos, desenvolvimento, etc.

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Valeu! e até a próxima!

- - - - - -

![](./assets/img/uploads/2017/11/foto-perfil-redondo-johnny.png)  

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -
