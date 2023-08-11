---
title: 'Os comandos mais utilizados no Linux'
date: '2023-03-12T16:22:05-03:00'
type: post
image: ../assets/img/thumb/comandos-mais-usados-linux.jpg
tag:
    - 'Administração de sistemas'
    - 'Comandos Linux'
    - 'Comandos mais usados no Linux'
    - 'dicas linux'
    - 'Sistemas Operacionais'

---

- - - - - -

O Linux é um sistema operacional popular que é amplamente utilizado em servidores, desktops e dispositivos móveis. O Linux possui uma grande variedade de comandos que podem ser utilizados para realizar tarefas específicas. Neste artigo, apresentaremos os comandos mais utilizados no Linux e suas principais funcionalidades.

### Comandos básicos

#### 1. ls

O comando `ls` é utilizado para listar os arquivos e diretórios presentes no diretório atual. Alguns exemplos de utilização do comando `ls` são:

```
ls
```

Este comando irá listar os arquivos e diretórios presentes no diretório atual.

```
ls -a
```

Este comando irá listar todos os arquivos e diretórios presentes no diretório atual, incluindo os arquivos ocultos.

#### 2. cd

O comando `cd` é utilizado para navegar entre os diretórios do sistema. Alguns exemplos de utilização do comando `cd` são:

```
cd /home/user
```

Este comando irá navegar para o diretório `/home/user`.

```
cd ..
```

Este comando irá navegar para o diretório pai do diretório atual.

#### 3. mkdir

O comando `mkdir` é utilizado para criar um novo diretório. Alguns exemplos de utilização do comando `mkdir` são:

```
mkdir /home/user/novo_diretorio
```

Este comando irá criar um novo diretório chamado `novo_diretorio` dentro do diretório `/home/user`.

#### 4. rm

O comando `rm` é utilizado para remover arquivos ou diretórios do sistema. Alguns exemplos de utilização do comando `rm` são:

```
rm arquivo.txt
```

Este comando irá remover o arquivo `arquivo.txt` do sistema.

```
rm -r diretorio
```

Este comando irá remover o diretório `diretorio` e todos os arquivos e subdiretórios contidos nele.

### Comandos avançados

#### 1. grep

O comando `grep` é utilizado para buscar por um padrão em um arquivo ou na saída de outro comando. Alguns exemplos de utilização do comando `grep` são:

```
grep "padrão" arquivo.txt
```

Este comando irá buscar pelo padrão `padrão` no arquivo `arquivo.txt`.

```
comando | grep "padrão"
```

Este comando irá executar o comando `comando` e buscar pelo padrão `padrão` na sua saída.

#### 2. top

O comando `top` é utilizado para visualizar informações sobre os processos em execução no sistema. Alguns exemplos de utilização do comando `top` são:

```
top
```

Este comando irá exibir uma lista dos processos em execução no sistema, ordenados por uso de CPU.

```
top -u usuario
```

Este comando irá exibir uma lista dos processos em execução no sistema que estão sendo executados pelo usuário `usuario`.

#### 3. ps

O comando `ps` é utilizado para visualizar informações sobre os processos em execução no sistema. Alguns exemplos de utilização do comando `ps` são:

```
ps
```

Este comando irá exibir uma lista dos processos em execução no sistema.

```
ps -ef
```

Este comando irá exibir uma lista detalhada dos processos em execução

#### 4. wget

O comando `wget` é utilizado para fazer download de arquivos da internet. Alguns exemplos de utilização do comando `wget` são:


```
wget https://exemplo.com/arquivo.txt
```

Este comando irá fazer o download do arquivo `arquivo.txt` do site `https://exemplo.com`.

```
wget -r -np https://exemplo.com/
```

Este comando irá fazer o download de todos os arquivos do site `https://exemplo.com`.

#### 5. scp

O comando `scp` é utilizado para transferir arquivos entre máquinas remotas. Alguns exemplos de utilização do comando `scp` são:

```
scp arquivo.txt usuario@exemplo.com:/home/usuario
```

Este comando irá transferir o arquivo `arquivo.txt` para a máquina `exemplo.com`, no diretório `/home/usuario`.

```
scp usuario@exemplo.com:/home/usuario/arquivo.txt .
```

Este comando irá transferir o arquivo `arquivo.txt` da máquina `exemplo.com`, no diretório `/home/usuario`, para o diretório atual.

#### 6. tar

O comando `tar` é utilizado para criar, extrair e manipular arquivos de arquivos compactados. Alguns exemplos de utilização do comando `tar` são:

```
tar -cvzf arquivo.tar.gz pasta/
```

Este comando irá compactar o conteúdo da pasta `pasta/` em um arquivo chamado `arquivo.tar.gz`.

```
tar -xvzf arquivo.tar.gz
```

Este comando irá extrair o conteúdo do arquivo `arquivo.tar.gz`.

#### 7. find

O comando `find` é utilizado para buscar por arquivos e diretórios no sistema. Alguns exemplos de utilização do comando `find` são:

```
find /home/user -name "arquivo.txt"
```

Este comando irá buscar pelo diretório `diretorio` dentro do diretório `/home`.

### Conclusão

O Linux possui uma grande variedade de comandos que podem ser utilizados para realizar tarefas específicas. Neste artigo, apresentamos os comandos mais utilizados no Linux e suas principais funcionalidades. Com estes comandos, você poderá realizar diversas tarefas no sistema operacional Linux de forma mais eficiente e produtiva.

Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Valeu! e até a próxima!

- - - - - -

![](../assets/img/uploads/2017/11/foto-perfil-redondo-johnny.png)

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -