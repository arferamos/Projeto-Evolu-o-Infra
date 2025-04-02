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

## Link do Projeto:
https://d2ld81zfdsa2r6.cloudfront.net/

## Layout do Projeto
![image](https://github.com/user-attachments/assets/cf901214-2ddc-4d1d-9349-f10214695bec)

# Reunião de Arquitetura:
Pontos de melhorias e considerações a serem discutidos em reunião de arquitetura e preparação para a reunião executiva para realizar a defesa do projeto tanto para a parte financeira P.O e RFP quanto para opções tecnológicas.


## Como opção, utilizando nossos Datacenters em ambiente on-premises, podemos considerar como evolução a aplicação rodando em Kubernetes em cluster segregado sendo orquestrado pelo Rancher:
![Cluster Segregado](https://github.com/user-attachments/assets/c3d88cec-49f1-4d34-9d71-9420582d124c)


## Storage H.A com GFS sendo utilizado a solução Huawei, Dell ou Hitachi Vantara na qual tenho experiência nas 3 soluções:
![image](https://github.com/user-attachments/assets/f5404b7f-7882-40ec-ad6f-d0350ec23efb)

Obs: Nas soluções para storage desses players, é possível tambem utilizar NAS para file server e Object Storage com SDS

## Disaster Recovery
Disaster Recovery plan com link dedicado utilizando mpls, fiber channel com raio de até 300km entre Datacenters On-premises.
![image](https://github.com/user-attachments/assets/958fd626-5e9b-4949-910b-4aa3cffbd430)


# Datacenter On-premises é importante checar o raio entre os DCs, Backbone, Tier 1,2,3,ou 4 também qual o tipo de conexão se é utilizando VPN, Fast Connect, Link Dedicado entre on-premises e cloud etc...



Obs:
Para cenário utilizando Terraform e Ansible, posso compartilhar outro projeto que estou finalizando e o link será disponibilizado em breve.


## Comandos Utilizados para deploy do Projeto:
Favor checar em Comando utilizados.md

## Evidências da Solução
Favor checar em Evidências.md

## Plano Orçamentário FinOps
Favor checar em Plano FinOps.md

# Autor
Arlindo Ferreira da Silva Ramos

https://www.linkedin.com/in/arlindo-ramos/




