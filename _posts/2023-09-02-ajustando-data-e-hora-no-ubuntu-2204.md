---
layout: post
date: '2023-09-02'
title: 'Ajustando Data e Hora no Ubuntu 22.04 Jammy'
subtitle:
image: ../assets/img/thumb/timezone-ubuntu.png
tag:
    - 'ajustar hora ubuntu 22.04'
    - 'timedatectl ubuntu 22.04'
    - 'ajustar horario utc-3 ubuntu 22.04'
    - 'como definir data e hora brasil ubuntu 22.04'
comments: true
---

- - - - - -

Opa, tudo certo?

Passando aqui para registrar uma dica rápida de como ajustar a data e hora para o timezone Brasil no Ubuntu 22.04 LTS.

Para verificar a hora do seu sistema Linux Ubuntu 22.04, digite:

```bash
date
```

A saída do comando acima será algo como:

```bash
Sat Sep  2 16:53:47 UTC 2023
```

Você pode digitar o comando "timedatectl" para obter mais detalhes na saída do comando:

```bash
timedatectl
```

A saída do comando acima será algo como:

```bash
               Local time: Sat 2023-09-02 16:54:24 UTC
           Universal time: Sat 2023-09-02 16:54:24 UTC
                 RTC time: Sat 2023-09-02 16:54:24
                Time zone: Etc/UTC (UTC, +0000)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```

Você pode listar todas as configurações disponíveis para configurar no seu ambiente com o comando abaixo:

```bash
timedatectl list-timezones
```

Você vera uma lista com todas as opções de timezone disponíveis.
Em nosso caso vamos filtrar por São Paulo, utilizando o comando abaixo:

```bash
timedatectl list-timezones |grep Sao_Paulo
```

O Resultado irá aparecer assim:

```bash
America/Sao_Paulo
```

Este é o parametro que iremos utilizar para configurar no nosso comando para ajustar o timezone correto para o nosso ambiente.

Digite o comando abaixo para definir o horário UTC-3:

```bash
sudo timedatectl set-time America/Sao_Paulo
```

O Resultado irá aparecer assim:

```bash
               Local time: Sat 2023-09-02 14:00:13 -03
           Universal time: Sat 2023-09-02 17:00:13 UTC
                 RTC time: Sat 2023-09-02 17:00:13
                Time zone: America/Sao_Paulo (-03, -0300)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```

Prontinho, data e hora ajustado para o timezone de São Paulo.


Dúvidas, comentário e sugestões postem nos comentários…  
👋🏼 Até a próxima!

- - - - - -

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<http://www.tidahora.com.br>  