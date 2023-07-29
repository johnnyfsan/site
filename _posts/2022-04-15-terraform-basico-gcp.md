---
layout: post
title: 'Terraform: O que é e como começar provisionando uma instância Linux no GCP'
subtitle:
author: 'Johnny Ferreira'
tags: [terraform, devops, cloud, gcp, aws, amazon web services]
thumbnail-img: /assets/img/uploads/2022/04/terraformgcp.png
share-img: /assets/img/uploads/2022/04/terraformgcp.png
comments: true
---
- - - - - -

Se você é um profissional de TI que atua com infraestrutura ou é sysadmin certamente já pensou ou já automatizou muito de suas tarefas operacionais. No passado quando eu era sysadmin já fazia automação das rotinas que administrava em servidores Linux, sempre utilizando Shell Script como meu aliado para execução de determinadas tarefas e rotinas.

Porém estamos vivendo uma nova era na tecnologia, com a ascensão das plataformas de Clouds públicas, como [Azure](https://azure.microsoft.com/), [AWS](https://aws.amazon.com/pt/) e [GCP](https://cloud.google.com/), trouxe ao mercado de TI uma nova forma de gerenciar recursos de infraestrutura, a chamada IaC (Infrastructure as Code).

Essa nova forma de provisionar recursos de configuração de infraestrutura como código é um dos grandes aliados do DevOps, o gerenciamento de infraestrutura é feito de uma forma inteligente, simplificada e totalmente escalável.

Agora é possível provisionar o deploy de grandes aplicações em clouds distintas, no conceito de Multi-Cloud utilizando ferramentas de IaC.

Alguns exemplos de ferramentas de IaC, como Puppet, Ansible, Chef, Vagrant, CloudFormation da [AWS](https://aws.amazon.com/pt/) e agora o Terraform que vem conquistando a comunidade e ajudando diversos profissionais de tecnologia a serem mais eficazes em suas funções.

Neste post estaremos dando os passos iniciais para trabalhar no Terraform.

*Antes de mais nada leia a documentação oficial dos caras!*

**Documentação Oficial:** [https://www.terraform.io/docs](https://www.terraform.io/docs)

- - - - - -

### **1. Fazendo o Download e Instalando o Terraform**

Let’s get started! 

Acesse o site oficial do Terraform clicando neste [link](https://www.terraform.io/downloads) para fazer o download da ferramenta. Selecione o seu sistema operacional e faça a instalação do Terraform.

[![](/assets/img/uploads/2022/04/terraform-main-page.png)](/assets/img/uploads/2022/04/terraform-main-page.png)

Depois de executar a instalação, bora pro Terminal para verificar se a instalação foi realizada com sucesso, no seu terminal digite:

 ```
terraform
```

O sistema irá nos retornar os sub-comandos/parâmetros do terraform:

[![](/assets/img/uploads/2022/04/terraform-help.png)](/assets/img/uploads/2022/04/terraform-help.png)

Se recebeu a mensagem conforme imagem acima, está tudo certo.

- - - - - -

### **2. Criando um repositório local para organização**

Antes de tudo, vamos criar um diretório local para servir de repositório para os arquivos de configuração do Terraform (Os arquivos de configuração do Terraform possuem a extensão .tf), dessa forma conseguimos organizar melhor nossa estrutura de trabalho e os arquivos não ficam solto em diferentes diretórios no sistema operacional.

No meu caso, estou utilizando Mac OS e estou criando um diretório chamado “terraform” dentro do meu diretório Home, basta abrir o Terminal e executar o comando abaixo

 ```
mkdir ˜/terraform
```

É neste diretório que estaremos salvando nossos arquivos de configuração, e você pode organizar por projeto, por provedor de cloud, por tipo de serviço ou aplicação, fica a seu critério.

**Por exemplo:**

- **Diretório principal:** terraform
- **Sub-diretório para arquivos da AWS:** terraform-aws
- **Sub-diretório para arquivos do GCP:** terraform-gcp
 
[![](/assets/img/uploads/2022/04/terraform-dir.png)](/assets/img/uploads/2022/04/terraform-dir.png)

- - - - - -

### **3. Edição dos arquivos de configuração .tf**

Em relação a edição dos arquivos de configuração do Terraform, podemos utilizar os editores de textos padrões, como Sublime, Visual Studio Code ou até mesmo o VIM (para galera raiz hehe) para sistemas Unix.

Neste exemplo utilizei o [Sublime](https://www.sublimetext.com/), onde fiz a instalação do pacote de sintaxe do Terraform no Sublime.

A dica de como realizar a instalação está neste [**Link**](https://packagecontrol.io/packages/Terraform)**.**

- - - - - -

### **4. Preparando o ambiente lá no GCP (Google Cloud Platform)**

Antes de iniciarmos a criação da nossa infraestrutura com código, precisamos preparar o Projeto que estaremos iniciando lá no GCP.

Acesse sua conta no Google Cloud Platform (GCP) através do link abaixo:

[https://console.cloud.google.com/](https://console.cloud.google.com/)

Clique em **Criar Novo Projeto**, defina um nome para o projeto e caso tenha alguma organização defina nesta tela.

[![](/assets/img/uploads/2022/04/gcp-home-1.png)](/assets/img/uploads/2022/04/gcp-home-1.png)

Após criarmos o projeto, esta é a tela inicial contendo todas as informações e recursos que podemos iniciarmos na Cloud do Google.

[![](/assets/img/uploads/2022/04/gcp-home-2.png)](/assets/img/uploads/2022/04/gcp-home-2.png)

Em seguida, vamos gerar nossa chave de acesso para que o Terraform possa acessar os recursos e gerenciar via código.

Selecione o projeto criado e clique no botão **“Acessar as configurações do projeto”**, localizado na tela de informações do Projeto.

[![](/assets/img/uploads/2022/04/gcp-1.png)](/assets/img/uploads/2022/04/gcp-1.png)

Localize e clique na opção de **“Contas de serviço”**:

[![](/assets/img/uploads/2022/04/gcp-2.png)](/assets/img/uploads/2022/04/gcp-2.png)

Clique em **“+ Criar conta de Serviço”**

[![](/assets/img/uploads/2022/04/gcp-3.png)](/assets/img/uploads/2022/04/gcp-3.png)

Na etapa **1 Detalhes da conta de Serviço** preencha os dados do seu projeto, em seguida clique em **“Criar e Continuar”**:

Na etapa 2 vamos Conceder os acessos a conta de serviço, selecione a rule (papel) **“Projeto &gt; Editor”**

[![](/assets/img/uploads/2022/04/gcp-4.png)](/assets/img/uploads/2022/04/gcp-4.png)

Em seguida clique em **Concluir**, e a chave estará no painel para os próximos passos:

[![](/assets/img/uploads/2022/04/gcp-5.png)](/assets/img/uploads/2022/04/gcp-5.png)

Em seguida vamos baixar nossa chave para configurar no Terraform 🙂 

Para isso clique na opção de **Ações**, em seguida clique em **Gerenciar chaves** conforme imagem abaixo:

[![](/assets/img/uploads/2022/04/gcp-6.png)](/assets/img/uploads/2022/04/gcp-6.png)Na próxima tela clique em **Adicionar Chave**:

[![](/assets/img/uploads/2022/04/gcp-key-1.png)](/assets/img/uploads/2022/04/gcp-key-1.png)

Selecione a opção do tipo **JSON**:

[![](/assets/img/uploads/2022/04/gcp-key-2.png)](/assets/img/uploads/2022/04/gcp-key-2.png)

Ao clicar em **Criar** a chave é gerada e feita o download no computador:

[![](/assets/img/uploads/2022/04/gcp-7.png)](/assets/img/uploads/2022/04/gcp-7.png)

Agora que já temos nossa conta de serviço configurada e a chave para acesso, precisamos ativar a API antes de irmos para a escrita do código no Terraform.

Na barra de **Pesquisa** do GCP, digite **Compute Engine** e clique na opção **Compute Engine**.

[![](/assets/img/uploads/2022/04/gcp-api-1.png)](/assets/img/uploads/2022/04/gcp-api-1.png)

Na tela inicial clique em **Ativar:**

[![](/assets/img/uploads/2022/04/gcp-api-2.png)](/assets/img/uploads/2022/04/gcp-api-2.png)

**Obs.:** Esse processo pode levar alguns segundos até ficar pronto.

Ao finalizar você será redirecionado a tela de Instâncias de VM do GCP, pode fechar ou minimizar essa tela, iremos voltar nessa tela posteriormente.

- - - - - -

### **5. Começando com o Terraform**

Vamos começar criando o arquivo principal do Terraform, esse arquivo é responsável por informar ao Terraform qual será nossa plataforma (Provedor) Cloud que estaremos utilizando, bem como sua chave de acesso, região e zona de hospedagem dos recursos.

No arquivo abaixo estamos informando ao Terraform para criar uma VPC, um recurso de Redes na nuvem.

Abra o editor de texto de sua preferência e se preferir copie e cole o código abaixo:

 ```
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "3.5.0"
    }
  }
}

provider "google" {
  credentials = file("./keys/terraform-gcp-1-347321-7625010cf89d.json") <= change this line

  project = "397965398524" <= change this line
  region  = "us-central1"
  zone    = "us-central1-c"
}

resource "google_compute_network" "vpc_network" {
  name = "terraform-network"
}
```

Lembre-se de mudar a linha onde contém credentials com a localização da sua chave de acesso que geramos anteriormente e altere também a linha onde contém project com o ID do seu projeto lá do GCP.

Salve o arquivo acima com o nome de “main.tf” na estrutura de diretório que criamos anteriormente:

[![](/assets/img/uploads/2022/04/files-terraform-1.png)](/assets/img/uploads/2022/04/files-terraform-1.png)

Seu arquivo deve ficar assim:

[![](/assets/img/uploads/2022/04/sublime-1.png)](/assets/img/uploads/2022/04/sublime-1.png)

Em seguida salve o arquivo e no Terminal vamos digitar o comando para inicializar o diretório de configurações:

Digite:

 ```
terraform init
```

Se tudo estiver certo, você irá receber a seguinte mensagem:

[![](/assets/img/uploads/2022/04/terraform-init.png)](/assets/img/uploads/2022/04/terraform-init.png)

O passo seguinte é verificar a consistência do arquivo gerado, onde utilizamos o comando terraform fmt para analisar a formatação e sintaxe do nosso diretório/arquivo, com isso é possível reduzir erros operacionais no ambiente.

No terminal digite: **terraform fmt** em seguida digite **terraform validate**

[![](/assets/img/uploads/2022/04/terraform-fmt-validate.png)](/assets/img/uploads/2022/04/terraform-fmt-validate.png)

Se o resultado for positivo estamos no caminho certo.

O próximo passo agora é: **Criar a estrutura de código para subirmos uma instância Linux CentOS 7 no GCP.**

Para fazer isso, copie e cole o código abaixo no final do seu arquivo “main.tf”:

 ```
resource "google_compute_instance" "vm_instance" {
  name         = "terraform-instance"
  machine_type = "f1-micro"

  boot_disk {
    initialize_params {
      image = "centos-cloud/centos-7"
    }
  }

  network_interface {
    network = google_compute_network.vpc_network.name
    access_config {
    }
  }
}
```

Seu arquivo irá ficar assim:

[![](/assets/img/uploads/2022/04/terraform-main-tf.png)](/assets/img/uploads/2022/04/terraform-main-tf.png)

Volte ao terminal e execute os passos de validação novamente:

[![](/assets/img/uploads/2022/04/terraform-fmt-validate.png)](/assets/img/uploads/2022/04/terraform-fmt-validate.png)

O próximo passo é: Aplicar as configurações para o Terraform replicar o provisionamento no GCP!

No terminal digite:

 ```
terraform apply
```

O Terraform irá listar todas as configurações a serem realizadas no provedor, e irá solicitar uma confirmação que precisa ser digitada na tela:

[![](/assets/img/uploads/2022/04/terraform-apply-1.png)](/assets/img/uploads/2022/04/terraform-apply-1.png)

Após verificar a solicitação, digite **yes** e clique enter.

[![](/assets/img/uploads/2022/04/terraform-apply-2.png)](/assets/img/uploads/2022/04/terraform-apply-2.png)

Após confirmarmos a criação dos recursos, o Terraform irá apresentar a evolução do provisionamento do ambiente.

[![](/assets/img/uploads/2022/04/terraform-apply-3.png)](/assets/img/uploads/2022/04/terraform-apply-3.png)

Acesse o console do GCP e verifique se sua instância está sendo provisionada.

[![](/assets/img/uploads/2022/04/gcp-on-1.png)](/assets/img/uploads/2022/04/gcp-on-1.png)

Instância lançada com sucesso!

Agora você pode levantar qualquer tipo de aplicação nela ou utilizar o Terraform para fazer isso 🙂

Agora, se você quiser fazer alguma modificação no seu arquivo **main.tf**, basta incluir a modificação e digitar **terraform apply** novamente, o Terraform irá processar sua solicitação, ele irá comparar o que foi provisionado e o que tem de diferente no arquivo tf.

- - - - - -

### **6. Desfazendo o ambiente com o Terraform**

Se você precisar destruir seu ambiente e começar um outro do zero, basta digitar:

 ```
terraform destroy
```

Todo o provisionamento e mudanças feitas foram desfeitas pelo Terraform.

Será apresentado a você as informações que serão excluídas do provedor, basta digitar **yes** para confirmar sua solicitação.

[![](/assets/img/uploads/2022/04/terraform-destroy.png)](/assets/img/uploads/2022/04/terraform-destroy.png)

Saída do comando acima:

[![](/assets/img/uploads/2022/04/terraform-destroy-2.png)](/assets/img/uploads/2022/04/terraform-destroy-2.png)

Verifique no Google Cloud Platform se a instância Linux que provisionamos com o Terraform foi destruída com sucesso.

[![](/assets/img/uploads/2022/04/gcp-on-2.png)](/assets/img/uploads/2022/04/gcp-on-2.png)

Nenhuma instância presente no GCP. **=)**

**Considerações:**

Neste tutorial vimos a simplicidade de levantar recursos em provedores de nuvem pública, é possível termos arquivos para levantar ambientes de infraestrutura de servidores, VPCs, firewalls, banco de dados, buckets entre outros serviços que podem ser feitos em minutos.

- - - - - -

**Johnny Ferreira**  
<johnny.ferreira.santos@gmail.com>  
<https://www.tidahora.com.br>