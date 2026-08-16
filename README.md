<p align="center">
<img src="https://raw.githubusercontent.com/Dinizasilva/Dinizasilva/main/IMG_9953.PNG" alt="Banner Eliana Diniz" width="550">
</p>

<div align="center">
  
👋 E aí, sou a Eliana
  
Analista de Dados → Cloud Engineering
Finalizando AWS re/Start e me preparando pra certificação

https://aws.amazon.com/
https://www.python.org/
https://www.terraform.io/
https://dev.mysql.com/doc/
https://www.snowflake.com/
https://powerbi.microsoft.com/

</div>



## Sobre essa transição

Trabalhei anos com dados — Python, SQL, Snowflake, Power BI. Sempre que precisava de uma infraestrutura nova, tinha que esperar alguém provisionar. Foi aí que pensei: "por que eu mesma não aprendo a fazer isso?"
Então entrei no AWS re/Start. E aqui estou, no final do curso, com as mãos sujas de console AWS, quebrando a cabeça com IAM policies e finalmente entendendo por que aquele Security Group não deixava a porta 22 abrir.
Minha ideia é simples: usar tudo que aprendi em dados e juntar com infraestrutura Cloud. Construir coisas que funcionem, que sejam seguras e que eu saiba exatamente como montar do zero.


### Meu projeto principal — Olist

Esse não é um lab. Esse é o resultado de meses de construção, de tudo que eu estudei, de tudo que conheci em tecnologia. Foram muitas noites, tantos erros para reparar, e aqui cheguei.

Pipeline de Dados — E-commerce Olist

Arquitetura Medallion (Bronze → Silver → Gold) no Snowflake, transformações em SQL, visualização de KPIs no Power BI. Meu primeiro projeto de dados. A base de tudo.

### O que tem lá:

* Ingestão de dados brutos de e-commerce
* Limpeza, padronização e modelagem dimensional
* Dashboard com KPIs de negócio: volume de pedidos, ticket médio, distribuição geográfica, tempo de entrega
* Documentação técnica completa
* Por que isso importa pra Cloud:
* Esse pipeline hoje roda no Snowflake. Mas eu já sei como levar ele pra AWS: S3 (Bronze), Lambda/Glue (Silver), Redshift (Gold), QuickSight (dashboards). O que eu construí em dados, eu vou reconstruir em Cloud.

Ver projeto: https://github.com/Dinizasilva/projeto-analise-dados-olist


## Meus laboratórios práticos na AWS (O que eu já fiz de verdade).
Não é teoria. É console aberto, instância subindo, erro aparecendo e eu resolvendo.

### Lab: S3 Static Website Hosting

O que fiz: Hospedei um site estático (HTML) direto no S3, sem servidor, sem EC2.

Serviços: S3 | Bucket Policy | Static Website Hosting | HTML/CSS

O que deu errado: Tudo. O nome do bucket que eu queria já existia (nomes S3 são únicos globalmente). A opção de Static Website Hosting estava escondida nas propriedades do bucket — não é um tab, é uma seção lá embaixo. 

As permissões IAM pareciam não se encontrar: Block Public Access desativado, Bucket Policy aplicada, e ainda assim AccessDenied. 
Descobri que o ambiente do lab tinha uma SCP (Service Control Policy) no nível da conta bloqueando acesso público. 
Os links no HTML quebravam porque usei caminhos relativos (./about.html) que funcionam no PC mas não no endpoint s3-website. 
Refiz o HTML com caminhos simples (about.html). Deletei tudo e refiz do zero. 
Na segunda vez, funcionou de primeira.

Ver lab: github.com/Dinizasilva/AWS-S3-Static-Website-Hosting-Lab

### Lab: Observabilidade com CloudWatch + SNS + EventBridge

O que fiz: Configurei métricas no CloudWatch para monitorar CPU de duas instâncias EC2. Criei alarmes que disparam notificações por SNS (e-mail) quando a CPU passa de 80%. Usei EventBridge para capturar eventos de parada/terminação de instâncias.

Serviços: CloudWatch | CloudWatch Logs | SNS | EventBridge | EC2 | AWS Config

O que deu errado: O CloudWatch Agent tava "ativo" mas não enviava logs. Verifiquei permissões do arquivo (access_log) — tava certo. 
Mas o diretório pai (/var/log/httpd) tava com drwx------ e grupo root. O agente roda como cwagent e nem conseguia entrar no diretório. 
sudo chgrp cwagent /var/log/httpd e sudo chmod 750 resolveram. 
Também tentei ver os Configuration Recorders no AWS Config e tomei AccessDenied por causa de uma política Deny explícita no lab. 
Me forçou a lembrar: Deny sempre vence Allow.

Ver lab: github.com/Dinizasilva/aws-infrastructure-observability

### Lab: Auto Scaling + Alta Disponibilidade

O que fiz: Configurei um Auto Scaling Group com Launch Template, defini políticas de escala baseadas em CPU e distribuí as instâncias em múltiplas Availability Zones. Testei o stress da aplicação e vi as instâncias subirem sozinhas.

Serviços: EC2 Auto Scaling | ALB | CloudWatch | VPC

O que deu errado: O Load Balancer não distribuía o tráfego. As instâncias estavam em subnets privadas sem NAT Gateway. 
O health check falhava e o ASG ficava criando e destruindo instâncias em loop. Pânico por 20 minutos. 
O gráfico do CloudWatch parecia batimento cardíaco descontrolado. 
Mudei as instâncias pra subnets públicas (ou adicionei NAT Gateway), o health check passou, o loop parou. 
Depois rodei stress --cpu 2 --timeout 300 e vi o ASG criar a 3ª e 4ª instância em menos de 5 minutos.

Ver lab: github.com/Dinizasilva/AWS-Auto-Scaling-lab

### Lab: CloudFormation — Infrastructure as Code

O que fiz: Escrevi uma infraestrutura inteira em YAML: VPC, Subnet, Security Group, EC2, S3. Cliquei em "Create Stack" e vi o AWS criar tudo sozinho. 
Depois fiz UPDATE pra adicionar um S3 sem derrubar nada. No final, deletei tudo com um clique.

Serviços: CloudFormation | YAML | EC2 | VPC | S3 | IAM | Systems Manager Parameter Store

O que deu errado: Na verdade, nada quebrou nesse lab — o que me assustou foi o DELETE. Fiquei com medo de deixar recurso órfão. 
Mas o CloudFormation rastreia dependências e deleta na ordem certa. 
Também aprendi a usar o Parameter Store pra buscar AMI ID dinamicamente em vez de hardcodar. 
AMI IDs mudam de região pra região. Hardcodar é pedir pra quebrar.

Ver lab: github.com/Dinizasilva/AWS-CloudFormation-Infrastructure-as-Code-Cloud-Infrastructure-Automation


### Infrastructure as Code

Estou traduzindo esses labs para Terraform. 

A ideia é: se eu consigo criar no console, eu consigo criar em código.

O que já estou fazendo:

* Módulos básicos de EC2 + VPC
* Variáveis e outputs organizados
* State remoto no S3 (com locking no DynamoDB — porque aprendi que perder state é perder tudo)
* Repositório de Terraform em construção 🚧


### O que vem agora

[x] Pipeline de dados Olist — concluído
[x] AWS re/Start — concluído
[ ] Certificação AWS Cloud Practitioner (em preparação)
[ ] Terraform nos labs que já fiz na mão
[ ] Migrar o pipeline Olist para AWS (S3 → Lambda → Redshift → QuickSight)
[ ] Continuar documentando tudo aqui


## A bagagem que eu trago de Dados

Antes de tudo isso, eu já trabalhava com:
Python — automação de ETL, scripts de limpeza, análise exploratória
SQL — consultas complexas, otimização de queries, modelagem dimensional
Snowflake — data warehousing, pipelines, gestão de custos de compute
Power BI — dashboards que realmente ajudavam a área de negócio a decidir.


🌐 ## Vamos conversar?

Se você é recrutador, mentor ou alguém também fazendo essa transição, vamos trocar ideias.

💼 LinkedIn: linkedin.com/in/eliana-diniz
📧 E-mail: eliana.dinizsilva@gmail.com

"O console AWS não tem Ctrl+Z. Mas tem CloudTrail. E isso já é alguma coisa."
