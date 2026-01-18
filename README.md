# Relatório de um projeto uma plataforma virtual para uma farmácia fictícia, utilizando a infraestrutura da AWS.

# 💊 Plataforma Virtual de Farmácia - Relatório de Implementação AWS

**Data:** Janeiro de 2026  
**Empresa:** Abstergo Industries / [Sua Farmácia]  
**Responsável:** Nathália Costa 
**Repositório:** [[LinkGitHub](https://github.com/nathcpc)]

---

## 📋 Sumário Executivo

Este relatório documenta a implementação de uma plataforma virtual de e-commerce para farmácia fictícia utilizando serviços da Amazon Web Services (AWS). O projeto foi desenvolvido com o objetivo de aplicar conceitos práticos de computação em nuvem, escalabilidade e segurança em um cenário real e dinâmico.

---

## 🎯 Introdução

A implementação de uma plataforma de farmácia virtual na AWS representa uma oportunidade de demonstrar proficiência em arquitetura de nuvem, DevOps e desenvolvimento full-stack. Este projeto explora três serviços AWS estratégicos que proporcionam redução de custos, melhoria de performance e aumento da segurança da infraestrutura.

### Objetivos do Projeto

- ✅ Implementar infraestrutura escalável e segura na AWS
- ✅ Reduzir custos operacionais utilizando serviços gerenciados
- ✅ Garantir alta disponibilidade e performance da plataforma
- ✅ Demonstrar boas práticas de arquitetura em nuvem
- ✅ Documentar todas as etapas e decisões técnicas

---

## 📊 Descrição do Projeto

O projeto foi estruturado em **3 etapas principais**, cada uma focando em um serviço AWS específico que agrega valor à plataforma.

### 🏗️ Arquitetura Geral

```
┌─────────────────────────────────────────────────────────────┐
│                    PLATAFORMA DE FARMÁCIA                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Frontend (React/Vue)  →  API Gateway  →  Banco de Dados  │
│                                                             │
│  • Autenticação: AWS Cognito                              │
│  • Armazenamento: S3 + CloudFront                         │
│  • Backend: Lambda + RDS                                  │
│  • Mensageria: SNS/SQS                                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 Etapa 1: Amazon RDS (Relational Database Service)

### 📌 Nome da Ferramenta
**Amazon RDS PostgreSQL**

### 🎯 Foco da Ferramenta
- Gerenciamento automático de banco de dados relacional
- Backup automático e recuperação de desastres
- Escalabilidade vertical e horizontal
- Segurança integrada com VPC

### 📝 Descrição do Caso de Uso

O Amazon RDS foi escolhido para armazenar e gerenciar os dados principais da farmácia:

**Dados Armazenados:**
- Informações de medicamentos (nome, princípio ativo, dosagem, preço, estoque)
- Perfil de usuários/clientes (dados pessoais, histórico de compras)
- Pedidos e transações (data, status, endereço de entrega)
- Fornecedores (contato, horário de entrega)
- Prescrições digitais (validação de receitas)

**Benefícios:**
- **Redução de Custos:** Eliminação de servidores on-premises
- **Segurança:** Criptografia em trânsito e em repouso
- **Disponibilidade:** Multi-AZ para alta disponibilidade (99.95% SLA)
- **Manutenção:** AWS gerencia patches e atualizações

**Configuração Implementada:**
```yaml
Tipo de Instância: db.t3.medium
Motor: PostgreSQL 14.7
Armazenamento: 100 GB SSD
Backup: 7 dias de retenção automática
Multi-AZ: Habilitado
VPC: Privada (acesso via Security Groups)
Custo Estimado: ~$150-200/mês
```

---

## ☁️ Etapa 2: Amazon S3 (Simple Storage Service) + CloudFront

### 📌 Nome da Ferramenta
**Amazon S3 com CloudFront CDN**

### 🎯 Foco da Ferramenta
- Armazenamento escalável e durável de objetos
- Distribuição global de conteúdo com baixa latência
- Hospedagem de website estático (frontend)
- Versionamento e controle de acesso

### 📝 Descrição do Caso de Uso

O S3 armazena todos os arquivos estáticos e assets da plataforma, enquanto o CloudFront distribui o conteúdo globalmente:

**Arquivos Armazenados:**
- Imagens de medicamentos (fotos, bulas)
- Frontend React/Vue compilado (build estático)
- Documentos (termos de serviço, políticas de privacidade)
- Logs e relatórios em PDF
- Banners e materiais promocionais

**Arquitetura:**
```
Usuário → CloudFront (Cache Global)
                ↓
         Servidores Edge (300+ pontos)
                ↓
         S3 Bucket (Origem)
```

**Benefícios:**
- **Performance:** Redução de latência em até 90% com CloudFront
- **Durabilidade:** 99.999999999% (11 noves)
- **Segurança:** Bloqueio de acesso público, CORS configurado
- **Escalabilidade:** Automática para qualquer volume de tráfego

**Configuração Implementada:**
```yaml
S3 Buckets: 2 (produção + staging)
Versionamento: Habilitado
Acesso Público: Bloqueado
CloudFront Distributions: 1 (cache em 300+ edge locations)
TTL Cache: 86400 segundos (24 horas)
Certificado SSL/TLS: AWS Certificate Manager (gratuito)
Custo Estimado: ~$50-80/mês (incluindo transferência de dados)
```

---

## 🔐 Etapa 3: AWS Lambda + API Gateway

### 📌 Nome da Ferramenta
**AWS Lambda com API Gateway**

### 🎯 Foco da Ferramenta
- Computação serverless sem gerenciamento de servidores
- API REST escalável e segura
- Integração com outros serviços AWS
- Modelo de preço por execução (pay-as-you-go)

### 📝 Descrição do Caso de Uso

Lambda e API Gateway formam o backend da plataforma, processando toda a lógica de negócios:

**Funções Lambda Implementadas:**
1. `buscarMedicamentos` - Consulta catálogo de produtos
2. `criarPedido` - Processa novos pedidos
3. `validarPrescricao` - Valida receitas digitais
4. `atualizarEstoque` - Gerencia inventário
5. `processarPagamento` - Integração com Stripe/MercadoPago
6. `enviarNotificacao` - SNS para confirmações por email/SMS

**Fluxo de Requisição:**
```
Cliente (Frontend)
    ↓
API Gateway (HTTPS)
    ↓
Lambda Function (execução instantânea)
    ↓
RDS / DynamoDB / SNS (serviços auxiliares)
    ↓
Resposta JSON → Cliente
```

**Benefícios:**
- **Custos Reduzidos:** Paga apenas pelo tempo de execução (1M requisições gratuitas/mês)
- **Escalabilidade Automática:** Sem preocupação com provisionamento
- **Segurança:** IAM roles refinadas, sem exposição de servidores
- **Velocidade de Deploy:** Atualizar código em segundos

**Configuração Implementada:**
```yaml
Runtime: Python 3.11 / Node.js 18
Memória: 256-512 MB
Timeout: 30 segundos
Ambiente: staging + produção
API Gateway:
  - Autenticação: AWS Cognito
  - Rate Limiting: 1000 requisições/segundo
  - Cache: 300 segundos para GET
Custo Estimado: ~$20-50/mês (com volume esperado)
```


---

## 🏆 Benefícios Alcançados

### ✅ Redução de Custos
- Eliminação de CAPEX (gastos em hardware)
- OPEX variável conforme demanda
- Sem desperdício de recursos ociosos

### ✅ Escalabilidade
- Crescimento automático sem intervenção manual
- Suporta de 1 a milhões de usuários simultâneos
- Multi-região para cobertura global

### ✅ Confiabilidade
- SLA de 99.99% de disponibilidade
- Backup automático e disaster recovery
- Redundância geográfica integrada

### ✅ Segurança
- Compliance com HIPAA, PCI-DSS (importantes para farmácia)
- Criptografia em trânsito e em repouso
- Auditoria completa com CloudTrail

### ✅ Performance
- Latência reduzida com CloudFront
- Banco de dados otimizado para leitura/escrita
- Lambda com cold start < 1 segundo

---

## 🔄 Arquitetura Detalhada

```
┌──────────────────────────────────────────────────────────────────┐
│                    USUÁRIO FINAL                                 │
└────────────────────────────┬─────────────────────────────────────┘
                             │
                ┌────────────▼────────────┐
                │   CloudFront CDN       │
                │ (Cache Global - 300+)  │
                └────────────┬────────────┘
                             │
        ┌────────────────────┼────────────────────┐
        │                    │                    │
   ┌────▼─────┐      ┌──────▼──────┐    ┌───────▼────┐
   │  S3      │      │  API        │    │   Cognito  │
   │ (Assets) │      │ Gateway     │    │ (Auth)     │
   └──────────┘      └──────┬──────┘    └────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
   ┌────▼────┐      ┌──────▼──────┐    ┌──────▼─────┐
   │  Lambda │      │   RDS       │    │   SNS      │
   │(Backend)│      │(PostgreSQL) │    │(Mensagens) │
   └─────────┘      └─────────────┘    └────────────┘
```

---


## 🎓 Conclusão

A implementação desta plataforma virtual de farmácia na AWS demonstra a aplicação prática de:

✅ **Arquitetura em Nuvem** - Design escalável e resiliente  
✅ **DevOps & CI/CD** - Automation e deploy contínuo  
✅ **Segurança em Nuvem** - Compliance e boas práticas  
✅ **Serverless Computing** - Redução de custos e complexidade  
✅ **Infraestrutura como Código** - Reprodutibilidade e versionamento  

Este projeto serve como portfólio sólido para demonstrar competências em tecnologias de nuvem, sendo um diferencial importante em entrevistas técnicas e oportunidades profissionais na área de Cloud Computing.

---

## 📚 Referências e Anexos

### Documentação Oficial AWS
- [AWS RDS Documentation](https://docs.aws.amazon.com/rds/)
- [AWS S3 Documentation](https://docs.aws.amazon.com/s3/)
- [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/)
- [AWS API Gateway Documentation](https://docs.aws.amazon.com/apigateway/)
- [AWS Cognito Documentation](https://docs.aws.amazon.com/cognito/)

### Ferramentas Recomendadas
- [AWS Management Console](https://console.aws.amazon.com/)
- [AWS SAM CLI](https://aws.amazon.com/serverless/sam/)
- [Terraform AWS Provider](https://registry.terraform.io/providers/hashicorp/aws/)
- [Postman](https://www.postman.com/) - Testar API

---

## Responsável pelo Projeto

Nathália Costa

