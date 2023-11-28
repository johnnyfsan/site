---
title: 'Como limpar cache no Linux'
date: '2023-11-28T14:39:43-03:00'
image: ../assets/img/thumb/swap-linux.png
    - 'limpar cache linux'
    - 'limpar cache ram linux'
    - 'limpar cache buffer linux'
    - 'limpar cache swap linux'
---

- - - - - -

Olá, 🖖🏼
Espero que esteja bem.

A dica de hoje é como realizar a limpeza de cache no Linux, as vezes precisammos efetuar a limpeza por diversos motivos, então neste tutorial estaremos abordando a limpeza do cache na memória ram, buffer e swap.

### Um pouquinho de conhecimento sobre: cache, buffer e swap.

### Cache da memória ram

O cache da memória RAM é um mecanismo usado pelo kernel do Linux para manter os dados acessados regularmente. Isso tende a aumentar a capacidade de resposta do sistema, um cache sobrecarregado pode levar à retenção de dados obsoletos, afetando o desempenho do servidor linux.


### Buffer

Já o buffer realiza a retenção dos dados temporariamente. Os buffers armazenam dados transferidos entre componentes como CPU e HD, ele tem como objetivo facilitar a comunicação, e um excesso de dados armazenados em buffer pode prejudicar a velocidade e desempenho do sistema Linux.


### Swap

A Swap é uma área alocada no disco rígido que atua como memória virtual. Embora evite travamentos do sistema devido à falta de memória, pode tornar o sistema lento se for usado em excesso.


#### Como limpar o cache de memória RAM no Linux?

Cada sistema Linux possui três opções para limpar o cache sem interromper nenhum processo ou serviço.


### Limpando PageCache

Para limpar apenas o PageCache , você pode usar o seguinte comando, que limpará especificamente o PageCache, ajudando a liberar recursos de memória.

```
sudo sync; echo 1 > /proc/sys/vm/drop_caches
```

### Limpando Dentries e Inodes

Para limpar apenas os dentries e inodes , você pode usar o seguinte comando, que sincronizará o sistema de arquivos e limpará os dentries e os inodes, melhorando o desempenho do sistema ao liberar o diretório em cache e as informações do inode.

```
sudo sync; echo 2 > /proc/sys/vm/drop_caches
```

### Limpando PageCache, Dentries e Inodes

Para limpar pagecache , dentries e inodes , você pode usar o seguinte comando, que sincronizará o sistema de arquivos e limpará pagecache, dentries e inodes, ajudando a liberar memória e melhorar o desempenho do sistema.


```
sudo sync; echo 3 > /proc/sys/vm/drop_caches 
```

### Explicando os comandos utilizados.

- O "sudo" é usado para executar o comando como superusuário no Linux.
- O "sync" vai liberar o buffer do sistema de arquivos.
- O “;” ponto e vírgula é usado para separar vários comandos em uma única linha.
- O comando "echo 3 > /proc/sys/vm/drop_caches" é usado para eliminar o cache da página.



**ATENÇÃO:** O arquivo de "drop_caches" gerencia qual tipo de dados em cache deve ser limpo e os valores são:

- 1 – Limpa apenas o cache da página.
- 2 – Limpa dentries e inodes.
- 3 – Limpa o cache da página, dentries e inodes.


### Como limpar SWAP no Linux?

Para limpar a memória SWAP no Linux, você deve usar o comando swapoff seguido do parâmetro "-a", que desabilitará todas as partições de SWAP.


```
sudo swapoff -a
```

Após desativar a SWAP, vamos ativa-la novamente, só que agora zerada:

```
sudo swapon -a
```

Para verificar você pode usar o comando "free" ou "htop".

```
free
```

Dúvidas? Postem nos comentários!  
👋🏼 Até a próxima!

- - - - - -


**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -