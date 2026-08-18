<p align="center">
<img src="https://raw.githubusercontent.com/Dinizasilva/Dinizasilva/main/IMG_9953.PNG" alt="Banner Eliana Diniz" width="550">
</p>

  
## 👋 E aí, sou a Eliana

**Analista de Dados → Cloud Engineering**
AWS re/Start (Escola da Nuvem) concluído · Certificação AWS Cloud Practitioner — prova agendada para **setembro/2026**

[![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=flat&logo=terraform&logoColor=white)](https://www.terraform.io/)
[![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?style=flat&logo=snowflake&logoColor=white)](https://www.snowflake.com/)
[![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)

---

## Sobre essa transição

Trabalhei anos com dados — Python, SQL, Snowflake, Power BI (certificação PL-300). Sempre que precisava de uma infraestrutura nova, tinha que esperar alguém provisionar. Foi aí que pensei: "por que eu mesma não aprendo a fazer isso?"

Entrei no programa **AWS re/Start**, pela **Escola da Nuvem**. E aqui estou, com as mãos sujas de console AWS, quebrando a cabeça com IAM policies e finalmente entendendo por que aquele Security Group não deixava a porta 22 abrir.

A ideia é essa: pegar tudo que já sei de dados e não depender mais de esperar alguém provisionar nada. Se eu souber montar a infraestrutura do zero, eu também sei onde ela quebra — e é aí que dado vira decisão, não só gráfico bonito no Power BI.

---

## Projeto principal — Pipeline de Dados Olist

Esse não é um lab isolado. É o resultado de meses de construção — todo o conhecimento técnico anterior aplicado em um projeto real, do início ao fim.

**Arquitetura Medallion (Bronze → Silver → Gold)** no Snowflake, transformações em SQL, visualização de KPIs em Power BI.

**O que tem lá:**
- Ingestão de dados brutos de e-commerce
- Limpeza, padronização e modelagem dimensional
- Dashboard com KPIs de negócio: volume de pedidos, ticket médio, distribuição geográfica, tempo de entrega
- Documentação técnica completa

**Por que isso importa pra Cloud:** esse pipeline hoje roda no Snowflake. Mas eu já sei como levar ele pra AWS: S3 (Bronze), Lambda/Glue (Silver), Redshift (Gold), QuickSight (dashboards). O que eu construí em dados, eu vou reconstruir em Cloud — só que dessa vez sabendo mexer em cada camada, não só olhando o resultado final.

[Ver projeto](https://github.com/Dinizasilva/projeto-analise-dados-olist)

---

## Laboratórios práticos na AWS

Não é teoria. É console aberto, instância subindo, erro aparecendo — e eu resolvendo.

### Lab: S3 Static Website Hosting
**O que fiz:** hospedei um site estático (HTML) direto no S3, sem servidor, sem EC2.
**Serviços:** S3 · Bucket Policy · Static Website Hosting · HTML/CSS

**O que deu errado:** o nome do bucket que eu queria já existia (nomes S3 são únicos globalmente). A opção de Static Website Hosting estava escondida nas propriedades do bucket. As permissões IAM pareciam corretas — Block Public Access desativado, Bucket Policy aplicada — e mesmo assim `AccessDenied`. Descobri uma SCP (Service Control Policy) no nível da conta bloqueando acesso público. Os links também quebravam por causa de caminhos relativos que não funcionam no endpoint `s3-website`. Refiz com caminhos simples e funcionou de primeira.

[Ver lab](https://github.com/Dinizasilva/AWS-S3-Static-Website-Hosting-Lab)

### Lab: Observabilidade — CloudWatch + SNS + EventBridge
**O que fiz:** configurei métricas no CloudWatch para monitorar CPU de duas instâncias EC2, alarmes disparando notificações por SNS quando a CPU passa de 80%, e EventBridge capturando eventos de parada/terminação de instâncias.
**Serviços:** CloudWatch · CloudWatch Logs · SNS · EventBridge · EC2 · AWS Config

**O que deu errado:** o CloudWatch Agent estava ativo mas não enviava logs — o diretório pai (`/var/log/httpd`) tinha permissões que impediam o agente (rodando como `cwagent`) de acessá-lo. Resolvido com `chgrp` e `chmod`. Também recebi `AccessDenied` no AWS Config por uma política Deny explícita no ambiente do lab — lição: **Deny sempre vence Allow**.

[Ver lab](https://github.com/Dinizasilva/AWS-infrastructure-observability)

### Lab: Auto Scaling + Alta Disponibilidade
**O que fiz:** configurei um Auto Scaling Group com Launch Template, políticas de escala baseadas em CPU, instâncias distribuídas em múltiplas Availability Zones, e testei stress na aplicação.
**Serviços:** EC2 Auto Scaling · ALB · CloudWatch · VPC

**O que deu errado:** o Load Balancer não distribuía tráfego porque as instâncias estavam em subnets privadas sem NAT Gateway. O health check falhava e o ASG entrava em loop de criar/destruir instâncias. Movi as instâncias para subnets públicas (e adicionei NAT Gateway), o health check passou. Depois rodei `stress --cpu 2 --timeout 300` e vi o ASG criar a 3ª e 4ª instância em menos de 5 minutos.

[Ver lab](https://github.com/Dinizasilva/AWS-Auto-Scaling)

### Lab: CloudFormation — Infrastructure as Code
**O que fiz:** escrevi uma infraestrutura inteira em YAML (VPC, Subnet, Security Group, EC2, S3), fiz UPDATE para adicionar recursos sem derrubar nada, e deletei tudo com um clique no final.
**Serviços:** CloudFormation · YAML · EC2 · VPC · S3 · IAM · Systems Manager Parameter Store

**O que deu errado:** nada quebrou tecnicamente, mas aprendi que o CloudFormation rastreia dependências e deleta na ordem certa (sem deixar recurso órfão). Também passei a usar o Parameter Store para buscar AMI ID dinamicamente em vez de hardcodar — AMI IDs mudam por região, hardcodar é pedir pra quebrar.

[Ver lab](https://github.com/Dinizasilva/AWS-CloudFormation-Infrastructure-as-Code-Cloud-Infrastructure-Automation)

---

### Infrastructure as Code

Traduzindo esses labs para Terraform — se eu consigo criar no console, eu consigo criar em código.

- Módulos básicos de EC2 + VPC
- Variáveis e outputs organizados
- State remoto no S3, com locking no DynamoDB (porque perder state é perder tudo)

---

### Próximos passos declarados

- [x] Pipeline de dados Olist — concluído
- [x] AWS re/Start (Escola da Nuvem) — concluído
- [ ] Certificação AWS Cloud Practitioner — **prova em setembro/2026**
- [ ] Terraform aplicado aos labs já feitos manualmente
- [ ] Segurança Cloud na prática — GuardDuty e Security Hub, sair da teoria
- [ ] Um projeto de IA Generativa de verdade, não só prompt engineering na teoria — isso ainda tá no papel, quero mudar isso
- [ ] Migrar o pipeline Olist para AWS (S3 → Lambda → Redshift → QuickSight)
- [ ] Continuar documentando tudo aqui

---

### A bagagem que eu trago de Dados

- **Python** — automação de ETL, scripts de limpeza, análise exploratória
- **SQL** — consultas complexas, otimização de queries, modelagem dimensional
- **Snowflake** — data warehousing, pipelines, gestão de custos de compute
- **Power BI (PL-300)** — dashboards que realmente ajudam a área de negócio a decidir

---

### Vamos conversar?

Se você é recrutador, mentor ou alguém também fazendo essa transição, vamos trocar ideias.

💼 LinkedIn: [linkedin.com/in/eliana-diniz](https://www.linkedin.com/in/eliana-diniz/)
📧 E-mail: eliana.dinizsilva@gmail.com

> "O console AWS não tem Ctrl+Z. Mas tem CloudTrail. E isso já é alguma coisa."
