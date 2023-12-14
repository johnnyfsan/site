---
title: 'Conhecendo o IPTables - Compartilhamento de Conexão, Mascaramento e Redirecionamento de Pacotes - Parte 2'
date: '2023-12-13'
image: ../assets/img/thumb/iptables-parte-2.jpeg
tag:
    - 'iptables linux'
    - 'aprendendo iptables'
    - 'configurando firewall linux com iptables'
    - 'ip_forward iptables'
---

- - - - - -

Olá, 🖖🏼

Na Parte 1 do artigo sobre iptables analisamos como o firewall funciona.
Vamos agora saber como ele trabalha com compartilhamento de conexão, mascaramento e redirecionamento de pacotes.

## Compartilhamento de Conexão. 

Vamos ter a rede **192.168.1.0/255.255.255.0** como exemplo. Vejam o comando abaixo:

```
iptables -t nat -A POSTROUTING -s 192.168.1.0/255.255.255.0 -o eth1 -j MASQUERADE
echo 1 > /proc/sys/net/ipv4/ip_forward
```

Os hosts da minha rede podem utilizar a internet através do roteamento dinâmico.
A linha que habilita o redirecionamento é essa:

```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

Mas vale observar que se inserirmos somente esta linha no script, a cada vez que reiniciar o sistema será necessário a ativação do roteamento. Para que não percamos o roteamento é necessário editar o arquivo **/etc/sysctl.conf** e inserimos ou modificamos a linha do modo que fique assim:

```
net.ipv4.ip_forward=1
```

Voltando ao exemplo, temos que verificar uma ação nova ( MASQUERADE ), servindo para que as máquinas da rede interna possam acessar a internet usando o IP externo do Gateway. Assim as máquinas da rede interna ficarão invisíveis para a rede externa. Para que uma máquina da rede interna possa executar serviços onde cuja necessita que o IP da máquina seja visível para redes externas, será necessário fazer um redirecionamento de IP’s ou Portas.

Vamos ter a maquina de IP 192.168.0.50 como exemplo, vamos supor que ela precise executar um serviço de FTP, na qual tem que ser feito a visibilidade da máquina. Podemos fazer com que o Firewall, cujo IP externo seja 200.200.100.100 receba estes pacotes e retransmita:

```
iptables -t nat -A PREROUTING -s 200.200.100.100 -i eth0 -j DNAT --to 192.168.0.50
iptables -t nat -A POSTROUTING -s 200.200.100.100 -o eth0 -p tcp --dport 21 -j ACCEPT
iptables -t nat -A POSTROUTING -s 192.168.0.50 -o eth0 -j SNAT --to 200.200.100.100
iptables -t nat -A POSTROUTING -s 192.168.0.50 -o eth0 -p tcp --dport 21 -j ACCEPT
```

Note que nas regras acimas, temos mais duas ações a SNAT e DNAT:

  * **SNAT:** É utilizada quando queremos alterar o endereço de origem do pacote. Aqui nós aplicamos para fazer o mascaramento. (Obs.: Somente a Chain POSTROUTING pode ser usada na ação SNAT).
  * **DNAT:** É utilizada quando desejamos alterar o endereço de destino do pacote. Está ação é aplicada para fazer redirecionamento de portas, redirecionamento de servidor, load balance e proxy transparente. As Chains que podem ser utilizadas para esta ação são PREROUTING e OUTPUT.

Outra Ação **--redirect**

A REDIRECT pode ser utilizada para fazer redirecionamento de portas, quando fazemos um redirecionamento de portas usamos o dado --to-port após a ação REDIRECT:

```
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 3128
```

A regra acima faz com que todos os pacotes direcionados a porta 80 sejam redirecionados para a porta 3128.

Quando usamos a tabela nat, é preciso inserir o parâmetro -t para especificar a tabela:

```
-t nat
```

Note que quando omitimos este parâmetro usamos a tabela filter por padrão.
Além destas ações, veja que existe também um novo dado.

**--to** = Este dado server para definir.

```
iptables -t nat -A PREROUTING -p tcp -d 200.200.100.100 --dport 80 -j DNAT --to 192.168.0.50:3128
```

Aqui, como na regra anterior os pacotes são redirecionados para a máquina 192.168.0.50, mas neste caso especificamos a porta, ou seja, porta 3128.

Opções e observações.

Vale observar a respeito da ordem das regras e manejo. Uma delas é que a primeira regra tem prioridade sobre a segunda caso ambas estejam em conflito, veja:

```
iptables -A FORWARD -p tcp –syn -s 192.168.1.0/24 -j ACCEPT
iptables -A FORWARD -p tcp –syn -s 192.168.1.0/24 -j DROP
```

A regra que terá validade será a primeira.

Podemos ver as regras em execução com o comando:

```
iptables -L
```

**--line-number** = Está opção mostra o número das regras

```
iptables -L --line-number
```

Para listar as Regras de Nat com o número de linhas:

```
iptables -L -t nat –line-numer
```

Para limpar todas as regras e tabelas usamos os comandos abaixo:

```
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X
```

Para que tudo possa funcionar corretamente é necessário primeiramente que todos os módulos necessários estejam carregados.

```
modprobe ip_conntrack
modprobe ip_conntrack_ftp
modprobe ip_nat_ftp
modprobe ip_queue
modprobe ip_tables
modprobe ipt_LOG
modprobe ipt_MARK
modprobe ipt_MASQUERADE
modprobe ipt_MIRROR
modprobe ipt_REDIRECT
modprobe ipt_REJECT
modprobe ipt_TCPMSS
modprobe ipt_TOS
modprobe ipt_limit
modprobe ipt_mac
modprobe ipt_mark
modprobe ipt_multiport
modprobe ipt_owner
modprobe ipt_state
modprobe ipt_tcpmss
modprobe ipt_tos
modprobe ipt_unclean
modprobe iptable_filter
modprobe iptable_mangle
modprobe iptable_nat
```

É importante lembrar que as regras de iptables devem seguir uma ordem definida, ou seja, a regra posterior deve estar de acordo com a regra anterior, para que tudo corra sem problemas:

```
iptables -P FORWARD -j DROP
iptables -A FORWARD -s 192.168.0.0/24 -d 10.0.0.0/8 -j ACCEPT
```

## Considerações 
O Iptables tem inúmeras possibilidades de regras, É praticamente impossível citar todos os parâmetros e regras que podemos utilizar. Fica como orientação ao administrador estudar os mínimos detalhes para poder aplicar as regras mais convenientes para a sua rede.
E sua rede tem uma ferramenta fortíssima que irá proteger seus dados e sua rede de possíveis ataques externos.


Dúvidas? Postem nos comentários!  
👋🏼 Até a próxima!

- - - - - -


**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -