#  _Projeto Final PB - Compass Uol | AWS e DevSecOps_

   ![logo-compass](https://github.com/user-attachments/assets/37b0ded0-e990-4228-8295-b063c8197782) 

   ---
   
## Contexto

Nós somos da empresa "Fast Engineering S/A" e gostaríamos de uma solução dos 
senhores(as), que fazem parte da empresa terceira "TI SOLUÇÕES INCRÍVEIS". 
Nosso eCommerce está crescendo e a solução atual não está atendendo mais a alta 
demanda de acessos e compras que estamos tendo. 

</br>

Atualmente usamos: 

• 01 servidor para Banco de Dados Mysql (500GB de dados, 10Gb de RAM, 3 Core 
CPU); 

• 01 servidor para a aplicação utilizando REACT – frontend (5GB de dados, 2Gb de 
RAM, 1 Core CPU); 

• 01 servidor de backend com 3 APIs, com o Nginx servindo de balanceador de 
carga e que armazena estáticos como fotos e links. (5GB de dados, 4Gb de RAM, 
2 Core CPU);

</br>

## Proposta

</br>

**Fase 1**: Migração “Lift and Shift” (o mais rápido possível, mantendo a aplicação como está, com o mínimo de modernização).  
</br>
**Fase 2**: Modernização com contêineres (EKS), banco gerenciado (RDS), armazenamento de objetos (S3), reforço de segurança etc.

---

## Diagrama da arquitetura Aual:

![Screenshot 2025-01-21 163258](https://github.com/user-attachments/assets/34982be8-c280-4aa5-ac75-119750750bb3)

---

# ETAPA 1 : Migração (Lift and Shift)

A meta é migrar rapidamente os servidores on-premises para a AWS, sem modificar a arquitetura.

<h3>Quais atividades são necessárias para a migração?</h3>

Planejamento e Análise:
Avaliação da infraestrutura atual (servidores, banco de dados, aplicações).
Identificação de requisitos de desempenho, segurança e escalabilidade.
Definição da estratégia de migração (re-host, re-platform, re-architect).

Configuração do Ambiente na AWS:
Criar instâncias EC2 para backend e frontend.
Provisionar a infraestrutura de rede (VPC, Subnets, Security Groups).
Configurar servidores de replicação para staging area.
Provisionar RDS para o banco de dados.

Instalação do AWS Replication Agent:
Instalar e configurar o AWS MGN Replication Agent nos servidores de origem para capturar dados e transferi-los para a AWS.

Replicação dos Dados:
Transferência de dados em tempo real para os servidores de replicação na AWS usando TCP 1500.
Validação da integridade dos dados replicados.

Testes (Test EC2 Instances):
Implementação de testes nas instâncias provisionadas para validar desempenho e funcionalidades.

Cutover (Migração final):
Atualização dos DNS para apontar para as novas instâncias AWS.
Monitoramento pós-migração para garantir estabilidade.

Descomissionamento do Ambiente Antigo:
Após testes e validações, desativação dos servidores antigos.

<h3>Quais as ferramentas vão ser utilizadas?</h3>

AWS MGN (Application Migration Service):

Para replicação contínua dos servidores de origem para a AWS.
Amazon EC2:

Hospedagem das instâncias frontend e backend após a migração.
Amazon S3:

Armazenamento temporário de dados durante a migração.
AWS DMS (Database Migration Service):

Para a migração do banco de dados MySQL para o Amazon RDS.
Amazon RDS:

Para armazenamento gerenciado do banco de dados MySQL após a migração.
AWS CloudWatch:

Para monitoramento de logs e desempenho das instâncias EC2 e banco de dados.
AWS VPC (Virtual Private Cloud):

Para configurar a infraestrutura de rede segura.
AWS Security Groups:

Para controle do tráfego de entrada e saída das instâncias.
AWS Route 53:

Para gerenciamento de DNS e redirecionamento de tráfego após o cutover.
Protocolo TCP (443, 1500, 3306):

Para controle, transferência de dados e conexão com o banco de dados.

<h3>Como serão garantidos os requisitos de Segurança?</h3>

Controle de Acesso:

Configuração de Security Groups para restringir acessos.
Uso de IAM Roles para limitar permissões específicas aos serviços.
Criptografia:

Tráfego seguro via TCP 443 (TLS/SSL) para controle de replicação.
Armazenamento de dados criptografados no S3 e RDS (AES-256).
Isolamento de Rede:

Divisão entre subnet pública (frontend) e privada (backend/banco de dados).
Uso de NAT Gateway para comunicação segura sem exposição direta à internet.
Monitoramento e Auditoria:

Uso do AWS CloudTrail para auditoria de eventos de acesso.
Configuração de CloudWatch Alarms para detecção de anomalias.
Firewall de Aplicação:

Implementação do AWS WAF (Web Application Firewall) para proteção contra ataques de camada de aplicação (DDoS, SQL Injection).

<h3>Como será realizado o processo de Backup?</h3>

Como será realizado o processo de Backup?
Durante a migração e após a conclusão, o processo de backup será feito das seguintes maneiras:

Durante a migração:

O AWS MGN mantém versões contínuas dos dados replicados, permitindo recuperação em caso de falha.
Os dados também podem ser armazenados temporariamente no Amazon S3, caso seja necessário reverter a migração.
Após a migração (Ambiente AWS):

Snapshots do Amazon EBS: Snapshots periódicos dos volumes de armazenamento EC2 para recuperação em caso de falha.
Backups automáticos do RDS: Configuração de retenção de backups automatizados no Amazon RDS.
Replication Cross-Region: Configurar replicação do banco de dados para outra região para garantir alta disponibilidade.
Armazenamento em longo prazo no S3 Glacier: Para arquivos e logs de auditoria de longo prazo.

<h3>Diagrama da infraestrutura na AWS</h3>

![Diagrama sem título drawio](https://github.com/user-attachments/assets/9b7b1d42-3acd-4ec1-9a62-7c17c09e59b4)


*o AWS Replication Agent envia os dados on-premises para uma VPC Temporária (Staging), enquanto o AWS MGN converte as VMs em instâncias EC2. Paralelamente, o AWS DMS cuida da migração do banco de dados para Amazon RDS, garantindo integridade de dados.*


# 💰 Valores 

## _Estimativa de custo Lift-and-Shift ETAPA 1_

* Custo mensal: 380,99 USD USD
  
* Custo total de 12 months : 4.571,88 USD USD
<br>

![image](https://github.com/user-attachments/assets/bf5f1721-3976-461b-9b10-6a044952790e)


  
