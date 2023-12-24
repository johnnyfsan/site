---
title: 'Desligar / Reiniciar Máquina Windows Remotamente'
date: '2018-09-11'
image: ../assets/img/uploads/2018/09/desligar-reiniciar-maquina-windows-remotamente.png
category:
    - 'Windows 10'
    - 'Windows 7'
    - 'Windows 8'
    - 'Windows Server 2008'
tag:
    - 'comando desligar remoto computador windows'
    - 'comando para desligar windows'
    - 'desligar computador remoto'
    - 'desligar remotamente windows'
    - 'desligar windows cmd'
    - 'desligar windows prompt'
    - 'desligar windows remoto'
    - 'reiniciar computador remoto'
    - 'reiniciar pc remoto'
    - 'reiniciar windows remoto'
    - 'shutdown client windows remot'
    - 'shutdown cmd windows'
    - 'shutdown windows'
    - 'Windows Server 2012'

---

- - - - - -

Olá, tudo bem? 😄  
É comum um administrador de redes se deparar com situações onde ele precisa forçar o desligamento ou reiniciar um micro remotamente. Independente se tem alguém utilizando essa máquina ou não. Os motivos podem ser diversos, desde a uma atualização que necessita de um reboot, ou um desligamento de um colaborador, onde o mesmo após sua demissão não pode mais ter acesso ao computador em rede local.

Neste tutorial irei descrever de uma forma simples como desligar ou reiniciar um computador remotamente com sistema operacional Windows.

**Observação: Lembrando que o procedimento deve ser realizado por um usuário com previlégio de Administrador nos computadores. (Exemplo: Administradores, Administradores de Domínio)**

Abaixo vou explicar os parâmetros que podemos utilizar para desligar um computador remotamente.

- - - - - -

Parâmetros para o comando “shutdown”:

**DESLIGAR O COMPUTADOR:** -s  
**REINICIAR O COMPUTADOR:** -r  
**TEMPO DE ESPERA:** -t xx ( xx = se refere ao tempo em segundos)  
**FORÇAR A FINALIZAÇÃO DE PROGRAMAS:** -f  
**COMPUTADOR REMOTO:** -m \\\\endereco\_ip ou \\\\nome\_computador

Agora que já sabemos os parâmetros que podemos utilizar para essa tarefa, vamos testar na prática.

Em um computador com um usuário administrador ou com permissões de super usuário abra o **Prompt de Comandos**:

Utilizando as teclas **W****inkey + R** digite **CMD** e clique em **OK**:

![](../assets/img/uploads/2018/09/desligar-reiniciar-maquina-windows-remotamente-1.png)

Com o Prompt de Comandos aberto, digite o seguinte:

```
shutdown -s -t 01 -f -m \\ip_do_cliente
```

Vou desligar o computador com Endereço IP 10.1.0.254 por exemplo:  
Digite o comando acima, e pressione **Enter** para confirmar o desligamento.

![](../assets/img/uploads/2018/09/desligar-reiniciar-maquina-windows-remotamente-2.png)

Na tela do computador cliente, vai aparecer uma mensagem de aviso que o computador será desligado.

![](../assets/img/uploads/2018/09/desligar-reiniciar-maquina-windows-remotamente-3.png)

Em seguida é realizado a ação de shutdown remoto.

![](../assets/img/uploads/2018/09/desligar-reiniciar-maquina-windows-remotamente-4.png)



Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Até a próxima!

- - - - - -

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -