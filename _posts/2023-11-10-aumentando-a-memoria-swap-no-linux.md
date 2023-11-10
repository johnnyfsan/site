---
layout: post
date: '2023-11-10'
title: 'Aumentando a Memória Swap no Linux'
subtitle:
image: ../assets/img/thumb/aumentando-a-memoria-swap-linux.png
tag:
    - 'add mb swap linux'
    - 'add swap linux'
    - 'aumentar swap centos'
    - 'aumentar swap linux'
    - 'dicas de linux'
    - 'swap centos'
    - 'swap config'
    - 'swap debian'
    - 'swap linux'
---

- - - - - -

Olá, 🖖🏼 Uma dica muito útil para quem precisa aumentar a memória Swap em ambientes Linux.

Primeiro precisamos definir a quantidade de Swap que iremos alocar.  
Vou alocar mais 500 Mb de swap.

Em seguida precisamos criar um diretório para alocar essa memória, eu vou criar no raiz mesmo:

```
mkdir /swap
```

O próximo passo é acessar o diretório e criar a swap:

```
cd /swap
```

```
dd if=/dev/zero of=/swap/swapfile bs=1024 count=500000
```

```
mkswap /swap/swapfile
```

Ativando a nova memória Swap no ambiente:

```
swapon /swap/swapfile
```

Verificando o aumento da Swap:

```
free
```

ou

```
cat /proc/meminfo
```

Por último precisamos configurar o arquivo /etc/fstab para que a configuração da nova Swap esteja disponível no boot do sistema.

```
vim /etc/fstab
```

E adicione o conteúdo abaixo no final do arquivo:

```
/swap none swap sw 0 0
```

Você pode reiniciar host para verificar se a configuração acima foi aplicada com sucesso.

Dúvidas? Postem nos comentários!  
👋🏼 Até a próxima!

- - - - - -


**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>
