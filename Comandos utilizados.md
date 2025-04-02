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
