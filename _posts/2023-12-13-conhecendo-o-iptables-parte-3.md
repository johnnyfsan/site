---
title: 'Conhecendo o IPTables - Alguns Exemplos de Uso - Parte 3'
date: '2023-12-13'
image: ../assets/img/thumb/iptables-parte-3.png
category:
    - 'Citrix XenServer'
    - 'Virtualização'
tag:
    - 'configurando firewall linux iptables'
    - 'linux iptables examples'
    - 'iptables linux exemplos'
    - 'iptables linux'
---


- - - - - -


Bom, agora que vimos como o iptables funciona e quais parâmetros e opções temos disponíveis para a implementação dele vamos fazer alguns testes pra ver na prática.

Antes de tudo, vamos limpar qualquer regra de firewall no servidor:

```
iptables -F
iptables -t nat -F
iptables -X
iptables -t nat -X
```

Verifique se está tudo limpo com os comandos abaixo:

```
iptables -L -n
iptables -t nat -L -n
```

Agora vamos carregar os módulos:

```
modprobe ip_tables
modprobe iptable_filter
modprobe iptable_mangle
modprobe iptable_nat
modprobe ip_nat_ftp
modprobe ip_gre
modprobe ip_conntrack
modprobe ip_conntrack_ftp
```

Vamos começar com um exemplo simples, bloquear o acesso ao serviço de SSH que por padrão usa a porta 22.

```
iptables -t filter -A INPUT -p tcp --dport 22 -j DROP
```

Resporta do comando: 
```
ssh: connect to host server port 22: Connection timed out
```

Note que o comando acima é bem simples, onde o iptables usa a tabela padrão (-t filter), faz a busca da opção a ser usada (-A INPUT), verifica os dados (-p tcp –dport 22), e por fim aplica a ação deseja, no caso bloqueando (-j DROP).

Quando realizamos um bloqueio a um determinado serviço ou porta, o cliente que tenta realizar o acesso simplesmente não sabe se trata de um bloqueio ou se o serviço está indisponível.
Para isso temos uma opção chamado REJECT, que avisa o cliente com uma mensagem de erro (Connection refused), mostra a mensagem sem ficar tentando conectar no serviço.

Veja o exemplo com o uso do REJECT.

```
iptables -t filter -A INPUT -p tcp --dport 22 -j REJECT
```

Resposta do comando: 

```
ssh: connect to host server port 22: Connection refused
```

Para que fique mais fácil para praticar cada exemplo a ser seguido, limpe as regras de firewall.

Vamos a outro exemplo: 
Nesse exemplo vamos bloquear as requisições de ping.

```
iptables -t filter -A INPUT -p icmp -j DROP
```

Em uma maquina cliente da sua rede, faça o teste, tente pingar o servidor. 
Vai bloquear na Hora. 
Veja que seguimos a mesma estrutura do exemplo do SSH.
Agora nosso teste vai partir para bloquear o acesso a internet aos clientes da rede.
Nosso ambiente de teste é o seguinte:

**Windows 10:**
**IP:** 10.106.0.50

**Servidor** CentOS 6.3 Firewall
**IP:** 10.106.0.100

Primeiro precisamos habilitar o ip_forward para compartilhar a internet com os clientes.

```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

Agora faça um teste em seu cliente para ver se ele acessa a internet.
Se sim, está tudo certo, caso não verifique as configurações de rede.

Vamos fazer um teste bloqueando o acesso total a Internet aos clientes do Firewall.
Analise o comando abaixo:

```
iptables -t filter -A FORWARD -s 10.0.0.0/8 -p tcp --dport 80 -j DROP
```

Bloqueando também a páginas HTTPS, muitos sites usam como Facebook, Hotmail, Gmail, etc.

```
iptables -t filter -A FORWARD -s 10.0.0.0/8 -p tcp --dport 443 -j DROP
```

Agora vamos ter como exemplo que toda nossa a rede é liberada pra acessar a qualquer site da Internet.
Só não queremos liberar o Hotmail.

```
iptables -t filter -A FORWARD -s 10.0.0.0/8 -d www.hotmail.com -p tcp --dport 80 -j DROP
iptables -t filter -A FORWARD -s 10.0.0.0/8 -d www.hotmail.com -p tcp --dport 443 -j DROP
```

Agora vamos trabalhar com redirecionamento.

Muito simples, muito usado em firewall com Squid para direcionar a saída de acessos da porta 80 para 3128.
Veja o exemplo:

```
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT –to-port 3128
```

  * '- t nat' = Tabela
  * '- A PREROUTING' = Chain
  * -i eth0 = Entrada / Interface
  * -p tcp = Tipo de Protocolo
  * --dport 80 = Porta Origem
  * -j REDIRECT = Ação 
  * --to-port 3128 = Porta Destino

Simples!
Para o redirecionamento da porta 80 para 3128, o serviço squid deve estar configurado no servidor.

Até aqui vimos regras simples e básicas do iptables, com ele podemos montar diversas regras.

Vamos ver como podemos fazer um acesso a alguma maquina da rede local pelo IP Válido do Servidor.
Primeiramente precisamos saber qual o serviço que essa maquina vai oferecer, por exemplo, Área de trabalho remota do Windows.

**IP da Estação:** 10.106.0.50
**Porta da Conexão remota:** 3389
**IP Válido do Servidor:** 200.200.100.100
**Porta da Conexão remota do servidor:** 8888

**Passo 1:**
Vamos criar uma regra para tratar quando o pacote chega ao firewall.

```
Iptables -t nat -A PREROUTING -d 200.200.100.100 -p tcp --dport 8888 -j DNAT --to-dest 10.106.0.50:3389
```

**O que essa regra tá fazendo ?**
Quando o pacote chega no IP 200.200.100.100, o firewall faz um NAT no pacote para que ele tenha um novo destino dentro da rede, que é o IP 10.106.0.50, mais isso só vai acontecer quando a porta for 8888.

**Passo 2:**
O pacote já foi encaminhado, mais e depois disso, ele preciso de um retorno certo ?
Veja abaixo:

```
iptables -t nat -A POSTROUTING -d 10.106.0.50 -p tcp --dport 3389 -j SNAT --to-dest 200.200.100.100:8888
```

Essa regra vai alterar a origem do pacote apontando para o firewall, para que seja feito o retorno da requisição.

**Observação:** Só não esqueça de que o encaminhamento de pacotes deve ser ativo.

```
echo 1 > /proc/sys/net/ipv4/ip_forward
```


Dúvidas? Postem nos comentários!  
👋🏼 Até a próxima!

- - - - - -


**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -