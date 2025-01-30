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

# Atividades Necessárias para a Migração

## 1. Planejamento e Análise
- Avaliação da infraestrutura atual (servidores, banco de dados, aplicações).
- Identificação de requisitos de desempenho, segurança e escalabilidade.
- Definição da estratégia de migração (re-host, re-platform, re-architect).

## 2. Configuração do Ambiente na AWS
- Criar instâncias EC2 para backend e frontend.
- Provisionar a infraestrutura de rede (VPC, Subnets, Security Groups).
- Configurar servidores de replicação para staging area.
- Provisionar RDS para o banco de dados.

## 3. Instalação do AWS Replication Agent
- Instalar e configurar o AWS MGN Replication Agent nos servidores de origem para capturar dados e transferi-los para a AWS.

## 4. Replicação dos Dados
- Transferência de dados em tempo real para os servidores de replicação na AWS usando TCP 1500.
- Validação da integridade dos dados replicados.

## 5. Testes (Test EC2 Instances)
- Implementação de testes nas instâncias provisionadas para validar desempenho e funcionalidades.

## 6. Cutover (Migração Final)
- Atualização dos DNS para apontar para as novas instâncias AWS.
- Monitoramento pós-migração para garantir estabilidade.

## 7. Descomissionamento do Ambiente Antigo
- Após testes e validações, desativação dos servidores antigos.

---

# Ferramentas Utilizadas

## 1. **AWS MGN (Application Migration Service)**
- Para replicação contínua dos servidores de origem para a AWS.

## 2. **Amazon EC2**
- Hospedagem das instâncias frontend e backend após a migração.

## 3. **Amazon S3**
- Armazenamento temporário de dados durante a migração.

## 4. **AWS DMS (Database Migration Service)**
- Para a migração do banco de dados MySQL para o Amazon RDS.

## 5. **Amazon RDS**
- Para armazenamento gerenciado do banco de dados MySQL após a migração.

## 6. **AWS CloudWatch**
- Para monitoramento de logs e desempenho das instâncias EC2 e banco de dados.

## 7. **AWS VPC (Virtual Private Cloud)**
- Para configurar a infraestrutura de rede segura.

## 8. **AWS Security Groups**
- Para controle do tráfego de entrada e saída das instâncias.

## 9. **AWS Route 53**
- Para gerenciamento de DNS e redirecionamento de tráfego após o cutover.

## 10. **Protocolos TCP (443, 1500, 3306)**
- Para controle, transferência de dados e conexão com o banco de dados.

<br>

# Garantia de Segurança e Backup no Processo de Migração para AWS

## 🔐 Como serão garantidos os requisitos de Segurança?

### **Controle de Acesso:**
- **Security Groups**: Configuração de regras para restringir acessos aos serviços e instâncias.
- **IAM Roles**: Uso de IAM Roles para limitar permissões específicas aos serviços, garantindo que apenas usuários e processos autorizados possam acessar determinados recursos.

### **Criptografia:**
- **Tráfego seguro**: Uso de protocolo TCP 443 (TLS/SSL) para controle de replicação entre ambientes, garantindo tráfego seguro.
- **Armazenamento de dados criptografados**: Os dados são armazenados de forma criptografada no S3 e no RDS utilizando AES-256, para proteger a integridade e confidencialidade das informações.

### **Isolamento de Rede:**
- **Subnet Pública e Privada**: A infraestrutura será dividida entre uma subnet pública (para o frontend) e uma subnet privada (para o backend e banco de dados), garantindo um melhor controle de tráfego.
- **NAT Gateway**: Implementação de um NAT Gateway para permitir comunicação segura entre as instâncias privadas e a internet, sem expô-las diretamente.

### **Monitoramento e Auditoria:**
- **AWS CloudTrail**: Uso do CloudTrail para registrar e auditar todos os eventos de acesso aos serviços da AWS.
- **AWS CloudWatch Alarms**: Configuração de alarmes para detectar anomalias de desempenho ou segurança nos serviços em tempo real.

### **Firewall de Aplicação:**
- **AWS WAF**: Implementação do Web Application Firewall (WAF) para proteger as aplicações contra ataques comuns, como DDoS e SQL Injection.

---

## 🔄 Como será realizado o processo de Backup?

### **Durante a Migração:**
- **AWS MGN**: O AWS Migration Hub (MGN) realiza a replicação contínua dos dados, mantendo versões das informações. Isso garante a possibilidade de recuperação em caso de falha durante o processo de migração.
- **Armazenamento Temporário no Amazon S3**: Caso seja necessário reverter a migração, os dados podem ser armazenados temporariamente no Amazon S3 para backup.

### **Após a Migração (Ambiente AWS):**
- **Snapshots do Amazon EBS**: Realização de snapshots periódicos dos volumes EC2 para garantir recuperação em caso de falha no armazenamento.
- **Backups Automáticos do Amazon RDS**: Configuração de backups automáticos no Amazon RDS, com retenção configurável, para garantir a integridade dos dados.
- **Replication Cross-Region**: Implementação de replicação de banco de dados para outra região, garantindo alta disponibilidade e tolerância a falhas.
- **Armazenamento de Longo Prazo no S3 Glacier**: Arquivos e logs de auditoria serão armazenados no S3 Glacier, permitindo armazenamento a longo prazo com baixo custo.

---

## 🖥️ Diagrama da Infraestrutura na AWS

![Diagrama da infraestrutura na AWS](https://github.com/user-attachments/assets/9b7b1d42-3acd-4ec1-9a62-7c17c09e59b4)

**Descrição do Diagrama:**
- O **AWS Replication Agent** envia dados on-premises para uma **VPC Temporária (Staging)**.
- O **AWS MGN** converte as VMs em **instâncias EC2**.
- O **AWS DMS** é responsável pela migração do banco de dados para o **Amazon RDS**, garantindo a integridade dos dados durante o processo.



# 💰 Valores 

## _Estimativa de custo Lift-and-Shift ETAPA 1_

* Custo mensal: 380,99 USD
  
<br>

![image](https://github.com/user-attachments/assets/bf5f1721-3976-461b-9b10-6a044952790e)

<br>
<br>


# Etapa 2: Modernização/Kubernetes

## Atividades da Migração
- **Levantamento da infraestrutura**: Mapeamento dos recursos existentes para planejar a migração.
- **Definição da arquitetura AWS**: Estruturação do ambiente na nuvem considerando escalabilidade e segurança.
- **Configuração do Kubernetes (EKS)**: Criação e ajuste do cluster para orquestração de containers.
- **Implementação de CI/CD**: Automação do deploy para garantir entregas contínuas.
- **Monitoramento e ajustes**: Acompanhamento pós-migração para otimizações e correções.

## Ferramentas Utilizadas
- **Terraform**: Provisionamento da infraestrutura como código.
- **Kubernetes**: Gerenciamento e escalabilidade de containers.
- **Helm**: Automação do deploy de aplicações no Kubernetes.
- **Prometheus & Grafana**: Monitoramento e visualização de métricas.
- **AWS CodePipeline**: Automação do fluxo de CI/CD.
- **Fluentd**: Coleta e análise de logs.

## Infraestrutura na AWS
- **Cluster EKS**: Orquestração e execução dos containers.
- **Amazon RDS (MySQL)**: Armazenamento persistente dos dados.
- **Elastic Load Balancing**: Distribuição de tráfego entre instâncias.
- **Amazon Route 53**: Gerenciamento de DNS e roteamento de tráfego.
- **AWS WAF**: Proteção contra ameaças e ataques.
- **AWS CloudFront**: Distribuição otimizada de conteúdo.

## Segurança
- **AWS IAM**: Controle de permissões e autenticação.
- **AWS Secrets Manager**: Armazenamento seguro de credenciais.
- **AWS CloudWatch**: Monitoramento e rastreamento de aplicações.
- **AWS WAF**: Proteção contra acessos indevidos.
- **TLS/SSL**: Comunicação segura entre serviços.

## Backup e Recuperação
- **Backup automatizado do RDS**: Garantia de integridade dos dados.
- **Snapshot dos volumes EBS**: Recuperação rápida de armazenamento.
- **Logs e métricas no S3**: Registro histórico para auditoria.
- **Disaster Recovery com replicação**: Estratégia para continuidade do serviço em falhas críticas.

---

## 🖥️ Diagrama da Infraestrutura na AWS

![Kubernetes drawio](https://github.com/user-attachments/assets/c1a5269a-1158-42de-8673-afd43b52ef49)

---

# 💰 Valores 

## _Estimativa de custo Modernização ETAPA 2_

* Custo mensal: 423,62 USD
* Custo Anual: 5.083,44 USD

<br>

![imagem (1)](https://github.com/user-attachments/assets/784d1132-8fea-44f4-852e-bd019c96f891)
