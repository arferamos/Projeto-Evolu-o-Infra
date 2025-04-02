# Projeto Evolução Infraestrutura
https://github.com/arferamos/Projeto-Evolu-o-Infra/blob/main/LICENSE

## Cenário
Nesse projeto, está sendo realizado a migração de um workload rodando em um “Data Center Corporativo (On-Premises)” para dentro da AWS. Mais especificamente, o objetivo é migrar a aplicação e o banco de dados dessa aplicação, para dentro da AWS. Na arquitetura da AWS será necessário implementar a VPC, juntamente com suas subnets, as instâncias EC2 irão armazenar a aplicação e o DynamoDB para o banco de dados.

## Cenário As-Is:
Arquitetura sem escalabilidade em ambiente virtualizado, está em “End-of-Service” e obsoleto devido a versões e contrato com fornecedores sem renovação "End-of-Suporte"

# Arquitetura As-IS:

Fluxo do ambiente atual:

1-	Acesso "Usuários e Parceiros Empresa XPTO" passa pelo Firewall, F5 BigIP, Web Servers e chega até a Base de Dados.

2-	 Acesso   "Conferência mundial" passa pelo Firewall, F5 BigIP, Web Servers  e chega até a Base de Dados

3-	 Base de Dados MySQL "Master x Master" onde o HAProxy faz o balance entre as bases para leitura e escrita.

4-	 JobScheduler com rotina online e também com rotina D-1 para coleta na Base de Dados NetSMS

Segue abaixo o Desenho de Arquitetura As-IS:

![data center corporativo](https://github.com/user-attachments/assets/2a2481b3-a6a5-4a94-a0da-78c6431046f4)

Obs:
É possível utilizar a arquitetura abaixo como proposta para o cenário To-be onde a construção seria realizada de forma manual passo a passo para resolver o problema de escalabilidade e manter o ambiente em H.A, porém não seria o cenário ideal.

![image](https://github.com/user-attachments/assets/fc50683f-fbae-4901-b837-69431e8a9080)

# Proposta para o cenário To-be:
Na implementação desse projeto é crucial que a solução possa escalar tanto a infraestrutura quanto a aplicação e para isso será utilizado o AWS Elastic Beanstalk, que fará o deploy 100% automatizado.
Será utilizado o DynamoDB para armazenamento de arquivos e para esse projeto será utilizado como exemplo o cadastro de emails e também será utilizado o serviço de entrega de conteúdo, CloudFront para fazer o caching dos arquivos estáticos e dinâmicos em uma Edge Location mais próxima do usuário.

## Desenho de arquitetura To-be:
![image](https://github.com/user-attachments/assets/26e656c8-812a-42f1-8339-a4e00d57dfac)

## Layout do Projeto
![image](https://github.com/user-attachments/assets/cf901214-2ddc-4d1d-9349-f10214695bec)

## Link do Projeto:
https://d2ld81zfdsa2r6.cloudfront.net/

# Reunião de Arquitetura:
Pontos de melhorias e considerações a serem discutidas em reunião de arquitetura e preparação para a reunião executiva para realizar a defesa do projeto tanto para a parte financeira P.O quanto para opções tecnológicas. favor checar em Considerações:


Para o Cenário On-Premises, podemos considerar a aplicação rodando em Kubernetes sendo orquestrado pelo Rancher:

Storage H.A com GFS sendo utilizado a solução Huawei, Dell ou Hitachi na qual tenho experiência nas 3 soluções:



Obs:
Para cenário utilizando Terraform e Ansible, posso compartilhar outro projeto que estou finalizando e o link será disponibilizado em breve.


# Comandos Utilizados para deploy do Projeto:
## Parte 1: Implementando DynamoDB + Elastic Beanstalk (EC2, SG, ELB, TG, Autoscaling...)

## **DynamoDB (Table)**

- Name: **users**
- Partition key | Primary key: **email**

  ### ***Revisar recursos:** EC2, SG, ELB, TG, Autoscaling…*

### **Criar Chave (opcional):**

Network & Security | Key Pairs | Create key pair

Name: **ssh-aws-keypar**

Private key file format: **.pem**

### Validar roles ‘elastic' criadas…

IAM / Roles / pesquise por “elastic” / Nenhuma role criada.

- Uma será criada pelo serviço do Elastic Beanstalk
- Outra, criaremos manualmente!

### Elastic Beanstalk

### **Create application**

***Step 1 - Configure environment***

**Environment tier**

(*) Web server environment

**Application information**

Application Name: **tcb-conference**

**Platform**

Platform: Python

Platform version: **(Recommended)**

**Application code**

Upload your code

Version label: **tcb-conference-version-01**

(*) Public S3 URL: 
https://tcb-bootcamps.s3.amazonaws.com/bootcamp-aws/pt/module4/tcb-conf-app.zip

Você pode usar a tecla "TAB" para acessar o campo para informar a URL.

**Presets**

Configuration presets

**(*) High availability**   

Next

***Step 2 - Configure service access***

**Service access**

>> 02 roles serão necessárias “**service**” (será criada automaticamente) e “**ec2**”

(*) Create and use new service role 

‘**Service**’ role name: **aws-elasticbeanstalk-service-role**

[ View permissions  details ]

✔ aws-elasticbeanstalk-service-role

Permissions:

- **AWSElasticBeanstalkEnhancedHealth**
- **AWSElasticBeanstalkManagedUpdatesCustomerRolePolicy**

**Trust relationships**


EC2 key pair: **ssh-aws-bootcamp**

E vamos criar a ‘**EC2 instance profile**’:

**IAM | Roles | Create Role | Trusted entity type:  AWS service**

Common use cases: **EC2**

Next

**Add permissions  [ Quais permissões?! Volte e verifique View Permission Details da EC2 instance profile ]**

- **AWSElasticBeanstalkWebTier**
- **AWSElasticBeanstalkWorkerTier**
- **AWSElasticBeanstalkMulticontainerDocker**

Next

Role name: **aws-elasticbeanstalk-ec2-role**

**Select trusted entities**

**Trust relationships**

***Create role***

Refresh…

Next

***Step 3 - Set up networking, database, and tags***

**Virtual Private Cloud (VPC):** `N. Virginia - Default VPC`

**Instance settings**

Public IP address

[ ✔ ]  Activated

**Instance subnets**

**Availability Zone:** **us-east-1a**

*Selecione todas as zonas!!*

Next

***Step 4 - Configure instance traffic and scaling***

**Instances**

Root volume type: General Purpose (SSD)

Size: **10 GB**

**Capacity**

Auto scaling group

Load balanced

Min: **2**

Max: **4**

Fleet composition: **(*) On-Demand instances**

Instance types: **t3.micro**


