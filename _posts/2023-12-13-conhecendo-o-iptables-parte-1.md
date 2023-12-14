---
title: 'Conhecendo o Iptables – O que é? Como Funciona? – Parte 1 '
date: '2023-12-13'
image: ../assets/img/thumb/iptables-parte-1.png
tag:
  - 'iptables linux'
  - 'como usar o iptables'
  - 'aprender iptables'
  - 'firewall com iptables'

---


- - - - - -

Olá, 🖖🏼
Neste post iremos abordar o que é o IPTABLES e como ele funciona.


## As características do Iptables:
   - Suporte aos Protocolos TCP, UDP, ICMP.
   - Pode especificar portas de endereço e destino.
   - Suporte aos módulos externos, como FTP e IRC.
   - Suporta um número ilimitado de regras por CHAINS ( correntes ).
   - Pode se criar regras de proteção contra diversos tipos de ataques.
   - Suporte para roteamento de pacotes e redirecionamentos de portas.
   - Suporta vários tipos de NAT, como o SNAT e DNAT e mascaramento.
   - Pode priorizar tráfego para determinados tipos de pacotes.
   - Tem suporte a IPV6, através do programa ‘ip6tables’.

## Como Funciona:

O **Iptables** funciona através de regras, podemos fazer com que os pacotes possam ser ou não recebidos em nossa rede ou algum host. Trabalha através de Tabelas, chains e Regras. Então vamos conhecer como isso funciona.

## Tabelas:  
Atualmente existem 3 tabelas possíveis de serem usadas no **Iptables**, sendo uma delas, a **mangle**, que é usada com pouca frequência, restando assim a **filter**, que é a padrão, podemos usar em tráfegos de dados comuns, quando não ocorre **NAT**. Quando não especificamos qual a tabela a ser utilizada é que será ativada. A outra geralmente utilizada é a ‘nat’, que como o nome diz, é utilizada em ocorrência de NAT.

## Chains: 
Através das **chains** podemos especificar uma situação do tratamento de pacotes, seja em qualquer tabela. Quando usamos a tabela **nat** as Chains possíveis são:


   - PREROUTING: Quando os pacotes entram para sofrerem NAT.  
   - POSTROUTING: Quando os pacotes estão saindo após sofrerem o NAT.  
   - OUTPUT: Pacotes que são gerados na própria máquina e que sofrerão NAT.  

Já na tabela filter as Chains são:

   - INPUT: Pacotes cujo o destino final é a própria maquina firewall.  
   - OUTPUT: Pacotes que saem da máquina firewall.  
   - FORWARD: Pacote que atravessa a máquina firewall, cujo o destino é outra máquina. Este tipo de pacote não sai da máquina firewall e sim de outra máquina da rede ou fonte. Em casos assim a máquina firewall está repassando o pacote.  

Veja abaixo uma simples referência:

```
               --------------------- --------------------------------
       TABELA  |      CHAIN          |            INTERFACE           |
               |                      ---------------- ---------------
               |                     |  ENTRADA (-i)  |    SAÍDA (-o) |
      --------- --------------------- ---------------- ---------------
     |         |  INPUT              |      SIM       |      NÃO      |
     | filter  |  OUTPUT             |      NÃO       |      SIM      |
     |         |  FORWARD            |      SIM       |      SIM      |
      --------- --------------------- ---------------- ---------------
     |         | PREROUTING          |      SIM       |      NÃO      |
     | nat     | OUTPUT              |      NÃO       |      SIM      |
     |         | POSTROUTING         |      NÃO       |      SIM      |
      --------- --------------------- ---------------- ---------------
     |         | PREROUTING          |      SIM       |      NÃO      |
     | mangle  |                     |                |               |
     |         | OUTPUT              |      NÃO       |      SIM      |
      --------- --------------------- ---------------- ---------------
```

## As Regras 

Geralmente as regras de firewall são composta da seguinte maneira “Tabela + opção + chain + dados + ação”. Através destes elementos podemos especificar o que fazer com os pacotes.

**Opções:**

  * -P  Define uma regra padrão.
  * -A  Adiciona uma nova regra as existentes. Este tem prioridade sobre a -P.
  * -D  Apaga uma regra.
  * -L  Lista as regras existentes.
  * -F  Limpa todas as regras.
  * -I  Insere uma nova regra.
  * -h  Exibe a ajuda.
  * -R  Substitui uma regra.
  * -C  Faz a checagem das regras existentes.
  * -Z  Zera uma regra específica.
  * -N  Cria uma nova regra com um nome.
  * -X  Exclui uma regra específica pelo seu nome.

**Dados:**

  * -s  Especifíca a origem do pacote. Este poder ser um host ou uma rede, veja o exemplo abaixo:
-s 192.168.0.0/255.255.255.0
ou
-s 192.168.0.0/24

**Observação:** No segundo caso estamos especificando a máscara de rede conforme o número de bits, por exemplo:
  * Máscara de rede 255.0.0.0  8
  * Máscara de rede 255.255.0.0  16
  * Máscara de rede 255.255.255.0  24

No exemplo acima estamos especificando toda uma rede da Classe C, no exemplo abaixo vamos especificar um host:

-s 192.168.30.51/255.255.255.255
ou
-s 192.168.30.51
poderia ficar assim:
-s 192.168.30.51/32

Observação: A máscara em número de bit 1 para host é 32.

Podemos especificar também:

-s www.google.com.br

Veja um exemplo na prática:

```
iptables -t filter -A INPUT -p tcp -s 192.168.0.0/24 -j ACCEPT
```

Podemos especificar qualquer origem também:


-s 0.0.0.0/0.0.0.0
ou
-s 0/0

  * -d  Especifica o destino do pacote. A sintaxe é a mesma do ‘-s’.

Veja o exemplo abaixo:

```
iptables -t filter -A INPUT -p tcp -d 192.168.0.0/24 -j ACCEPT
```

  * -i  Interface de entrada, ou seja, placa de rede, modem ou interface de conexão que estará recebendo o pacote a ser tratado.  
  * -i eth0. 
  * -i eth1. 
  * -i eth2. 
  * -i ppp0. 
  * -o  Interface de saída. As sintaxes são as mesmas que ‘-i’, sendo que neste caso estará enviando o pacote a ser tratado.
  * -o eth0
  * -o eth1
  * -o eth2
  * -o ppp0
  * !  Exclui um determinado argumento.
  * -i!eth0 – Refere-se a qualquer interface de entrada exceto a eth0
  * -s!192.168.0.45 – Refere-se a qualquer endereço de entrada exceto o 192.168.0.45

**--sport**
 Refere-se a porta de origem. Este deve ser acompanhado das funções ‘-p tcp’ e ‘-p udp’. 
 -p tcp --sport 80 – Refere-se a porta de origem 80 sob o protocolo TCP.

**--dport** Refere-se a porta destino. Assim como a função –sport, ela trabalha somente com a ‘-p tcp’ e ‘-p udp’. A sintaxe é similar a –sport.
Através das funções –sport e –dport podemos especificar não só uma porta especifica como também um range de portas:

**--sport** 33435:33525

 As Ações. 

Sempre vem após o parâmetro '-j' e geralmente são:


  * **ACCEPT**  Aceita e permite a passagem de pacote.
  * **DROP**  Não permite a passagem do pacote e abandona-o não dando sinais de recebimento.
  * **REJECT**  Assim como o DROP, não permite a passagem do pacote, mais envia um aviso ( icmp unreachable ).
  * **LOG**  Cria um arquivo de Log.

Com estes fatores podemos criar as nossas Regras com a seguinte composição:

```
iptables -A FORWARD -s 192.168.0.45 -p icmp -j DROP
```

Sendo: **-A** (opção)/ **FORWARD**(Chain)/**-s 192.168.0.45 -p icmp**(Dados) /**-j DROP**(Ação)

No exemplo acima a regra determina que todos os pacotes icmp de origem no endereço 192.168.0.45 devem ser barrados.


 Salvando as Regras. 

Depois de executar as regras, precisamos salvá-las, para que quando se reinicie o servidor não precise ficar digitando as regras.

```
itpables-save > iptables-servidor-1
```

No caso iptables-servidor-1 é o nome que escolhi para salvar as regras, você pode colocar qualquer nome de sua preferência.

Para Recuperá-las:

```
iptables-restore > <nome do arquivo>
```

Outra forma seria fazer um script com as regras:
Que na minha opinião é muito mais fácil e simples de manter o Firewall organizado.
Veja abaixo um modelo de script:

```
#!/bin/bash

#limpando tabelas
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X

#liberando acesso interno da rede
iptables -A INPUT -p tcp –syn -s 192.168.1.0/255.255.255.0 -j ACCEPT
iptables -A OUTPUT -p tcp –syn -s 192.168.1.0/255.255.255.0 -j ACCEPT
iptables -A FORWARD -p tcp –syn -s 192.168.1.0/255.255.255.0 -j ACCEPT

#compartilhando a web na rede interna
iptables -t nat -A POSTROUTING -s 192.168.1.0/255.255.255.0 -o eth1 -j MASQUERADE
echo 1 > /proc/sys/net/ipv4/ip_forward

# Protecao contra port scanners ocultos
iptables -A INPUT -p tcp –tcp-flags SYN,ACK,FIN,RST RST -m limit –limit 1/s -j ACCEPT

# Bloqueando traceroute
iptables -A INPUT -p udp -s 0/0 -i eth1 –dport 33435:33525 -j DROP

#Protecoes contra ataques
iptables -A INPUT -m state –state INVALID -j DROP

#termina
echo “Iptables Pronto”
```

Salve-o com um nome sugestivo, como por exemplo "start-firewall.sh"
Dê permissão de execução:

```
chmod +x start-firewall.sh
```

Pronto. Toda vez que quiser que ele seja habilitado é só executá-lo ( sempre como root ):

```
./start-firewall.sh
```

Para não ter que ficar chamando o script toda vez que inicializar o sistema, podemos fazer com que ele seja ativado na inicialização. Para isso é preciso editar o arquivo **'/etc/rc.d/rc.local'** e incluir o comando no final do arquivo.

Veja agora a Parte 2 sobre IPTables: [[http:tidahora.com.br/index.html/doku.php?idconhecendo_o_iptables_compartilhamento_de_conexao_mascaramento_e_redirecionamento_de_pacotes_parte_2|Compartilhamento de Conexão/Mascaramento e Redirecionamento de Pacotes]]


Dúvidas? Postem nos comentários!  
👋🏼 Até a próxima!

- - - - - -


**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -
