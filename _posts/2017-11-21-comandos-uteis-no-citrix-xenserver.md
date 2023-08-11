---
title: 'Comandos úteis no Citrix XenServer'
date: '2017-11-21T14:39:43-02:00'
image: ../assets/uploads/2017/11/comandos-citrix-xenserver.png
category:
    - 'Citrix XenServer'
    - 'Virtualização'
tag:
    - 'citrix'
    - 'comandos citrix'
    - 'comandos xen'
    - 'comandos xenserver'
    - 'comandos xenserver terminal'
    - 'command line citrix'
    - 'command line xenserver'
    - 'listar vm xenserver'
    - 'shutdown vm xenserver'
    - 'start vm xenserver'
    - 'xen'
    - 'xenserver'
---
- - - - - -

Olá, 🖖🏼

Comandos, console…. prático, rápido, funcional…. mas se você não usa todo dia acaba esquecendo, apesar de saber que eles existem. Vou listar alguns comandos do XenServer, comandos úteis no dia-a-dia de um SysAdmin, mas nem tanto utilizados. ⌨️

#### Listar Maquinas Virtuais (VMs):

```bash
xe vm-list
```

#### Listar discos das Maquinas Virtuais:

```bash
xe vm-disk-list --multiple
```

#### Configurar Pool para iniciar automaticamente:

```bash
xe pool-param-set uuid=uuid_pool other-config:auto_poweron=true
```

#### Configurar VM para iniciar automaticamente (auto start) após o boot do XenServer:

```bash
xe vm-param-set other-config:auto_poweron=true uuid=uuid_da_vm
```

#### Remover configuração de “auto start” das Maquinas Virtuais:

```bash
xe vm-param-remove param-key=auto_poweron param-name=other-config uuid=uuid_da_vm
```

#### Listar Templates de Sistemas Operacionais:

```bash
xe template-list
```

#### Excluir um Template:

```bash
xe template-param-set other-config:default_template=false uuid=uuid_do_template
xe template-param-set is-a-template=false uuid=uuid_do_template
xe vm-destroy uuid=uuid_do_template
```

#### Desligar uma Maquina Virtual:

```bash
xe vm-shutdown vm=nome_da_vm
```

#### Iniciar uma Maquina Virtual:

```bash
xe vm-start vm=nome_da_vm
```

#### Resetar o Estado da VM:

```bash
xe vm-reset-powerstate uuid=uuid_da_vm
```

#### Listar Tarefas do Citrix XenServer:

```bash
xe task-list
```

#### Cancelar uma tarefa:

```bash
xe task-cancel uuid=<tarefa>
```

#### Limpar todas as tarefas pendentes:

```bash
xe-toolstack restart
```

Dúvidas? Postem nos comentários!  
👋🏼 Até a próxima!

- - - - - -


**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>

- - - - - -