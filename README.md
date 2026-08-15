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





</div>

## Sobre essa transição

Trabalhei anos com dados — Python, SQL, Snowflake, Power BI. Sempre que precisava de uma infraestrutura nova, tinha que esperar alguém provisionar. Foi aí que pensei: "por que eu mesma não aprendo a fazer isso?"
Então entrei no AWS re/Start. E aqui estou, no final do curso, com as mãos sujas de console AWS, quebrando a cabeça com IAM policies e finalmente entendendo por que aquele Security Group não deixava a porta 22 abrir.
Minha ideia é simples: usar tudo que aprendi em dados e juntar com infraestrutura Cloud. Construir coisas que funcionem, que sejam seguras e que eu sabia exatamente como montar do zero.

## Meus laboratórios práticos na AWS (O que eu já fiz de verdade).
Não é teoria. É console aberto, instância subindo, erro aparecendo e eu resolvendo.


## Lab: EC2 + EBS + Snapshots

O que fiz: Subi uma instância EC2 na região us-east-1, anexei um volume EBS, configurei snapshots automáticos e testei recuperação de dados após "simular" uma falha (li o volume errado de propósito — aprendi na marra).
Serviços: EC2 | EBS | IAM (role para acesso ao S3)
O que deu errado: Esqueci de adicionar a role de IAM na instância antes de subir. Tive que criar outra. Agora eu leio duas vezes antes de clicar em "Launch".

## Lab: VPC, Subnets e Security Groups
O que fiz: Criei uma VPC do zero com subnets pública e privada. Configurei um Security Group que só permite SSH da minha máquina (IP específico, não 0.0.0.0/0 — aprendi que isso é burrice logo no primeiro dia).
Serviços: VPC | Subnets | Route Tables | Internet Gateway | Security Groups
O que deu errado: A instância na subnet privada não tinha acesso à internet. Demorei 40 minutos pra perceber que a Route Table não estava associada. Foi um tapa na cara que me ensinou mais do que qualquer slide.

## Lab: Observabilidade com CloudWatch + SNS + EventBridge

O que fiz: Configurei métricas no CloudWatch para monitorar CPU de duas instâncias EC2. Criei alarmes que disparam notificações por SNS (e-mail) quando a CPU passa de 80%. Usei EventBridge para capturar eventos de parada/terminação de instâncias.
Serviços: CloudWatch | SNS | EventBridge | EC2
O que deu errado: O alarme do CloudWatch não disparava. Descobri que a métrica estava configurada para "Average" em 5 minutos, mas eu estava fazendo o teste em 30 segundos. Mudei para "High Resolution" e funcionou. Detalhe mata.

## Lab: S3 — Versionamento, Lifecycle e Políticas

O que fiz: Criei buckets com versionamento ativado, configurei políticas de lifecycle para mover objetos antigos para Glacier e bloqueei acesso público com Bucket Policies. Testei upload, download e recuperação de versões antigas.
Serviços: S3 | IAM | AWS Config
O que deu errado: Tentei acessar um objeto privado pela URL pública. Obviamente deu 403. Foi aí que entendi a diferença entre ACL, Bucket Policy e IAM Policy na prática.

## Lab: Auto Scaling + Alta Disponibilidade

O que fiz: Configurei um Auto Scaling Group com Launch Template, defini políticas de escala baseadas em CPU e distribuí as instâncias em múltiplas Availability Zones. Testei o stress da aplicação e vi as instâncias subirem sozinhas.
Serviços: EC2 Auto Scaling | CloudWatch | ALB | VPC
O que deu errado: O Load Balancer não distribuía o tráfego. As instâncias estavam em subnets privadas sem NAT Gateway. O health check falhava e o ASG ficava criando e destruindo instâncias em loop. Pânico por 20 minutos, depois paz.

## Lab: Cloud Security e Compliance (AWS Config)

O que fiz: Ativei o AWS Config para monitorar conformidade dos recursos. Criei regras para verificar se buckets S3 estão criptografados, se Security Groups não têm portas abertas demais e se EBS volumes estão criptografados.
Serviços: AWS Config | CloudTrail | IAM
O que aprendi: Compliance não é só "clicar em enable". Você precisa entender o que está sendo monitorado e por quê. Criei um dashboard no Config para visualizar recursos não conformes de um jeito só meu.

## Infrastructure as Code

Estou começando a traduzir esses labs para Terraform. A ideia é: se eu consigo criar no console, eu consigo criar em código.
O que já estou fazendo:
Módulos básicos de EC2 + VPC
Variáveis e outputs organizados
State remoto no S3 (com locking no DynamoDB — porque aprendi que perder state é perder tudo)
Repositório de Terraform em construção 🚧

## A bagagem que eu trago de Dados

Antes de tudo isso, eu já trabalhava com:
Python — automação de ETL, scripts de limpeza, análise exploratória
SQL — consultas complexas, otimização de queries, modelagem dimensional
Snowflake — data warehousing, pipelines, gestão de custos de compute
Power BI — dashboards que realmente ajudavam a área de negócio a decidir.

Isso me ajuda muito na Cloud porque eu sei o que a infraestrutura precisa entregar pra quem trabalha com dados. Eu já estive do outro lado pedindo acesso, pedindo mais CPU, pedindo storage. Agora eu sei como fazer.

## O que vem agora

[x] AWS re/Start — quase lá
[ ] Certificação AWS Cloud Practitioner (em preparação)
[x] Terraform nos labs que já fiz na mão - Concluindo Certificação - Quase lá
[x] Primeiro projeto próprio: pipeline de dados 100% na AWS (S3 → Lambda → RDS → QuickSight)
[x] Continuar documentando tudo aqui

🌐 ## Vamos conversar?

Se você é recrutador, mentor ou alguém também fazendo essa transição, vamos trocar ideias.

💼 LinkedIn: linkedin.com/in/eliana-diniz
📧 E-mail: eliana.dinizsilva@gmail.com

"O console AWS não tem Ctrl+Z. Mas tem CloudTrail. E isso já é alguma coisa."
