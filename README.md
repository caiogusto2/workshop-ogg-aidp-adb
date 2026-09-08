# 🚀 OCI AI Data Platform & Autonomous Database Workshop

Workshop prático para construção de uma solução de dados **end-to-end na Oracle Cloud Infrastructure (OCI)** utilizando **OCI AI Data Platform** e **Oracle Autonomous Database 26ai**.

Ao longo dos laboratórios, você irá construir um pipeline completo de dados, passando pela ingestão e processamento com **Apache Spark**, implementação da **Arquitetura Medalhão (Bronze, Silver e Gold)**, integração com **Autonomous Database**, automação de processos e exploração dos dados utilizando recursos como **Select AI, APEX, Data Redaction e ORDS**.

---

## 🎯 Objetivo

O objetivo deste workshop é demonstrar, de forma prática, como diferentes serviços da Oracle Cloud podem trabalhar em conjunto na construção de uma plataforma moderna de dados.

A jornada começa com a ingestão e transformação dos dados no **OCI AI Data Platform** e continua no **Oracle Autonomous Database 26ai**, onde os datasets processados são utilizados para automação, consultas em linguagem natural, segurança de dados e disponibilização de APIs REST.

### Arquitetura do workshop

```text
Arquivos CSV
    │
    ▼
OCI AI Data Platform
    │
    ├── Bronze
    │     Dados brutos
    │
    ├── Silver
    │     Dados tratados e combinados
    │
    └── Gold
          Dados agregados
              │
              ▼
Oracle Autonomous Database 26ai
              │
              ├── Procedures
              ├── Oracle Scheduler
              ├── Select AI
              ├── Oracle APEX
              ├── Data Redaction
              └── ORDS / REST API
```

---

## 🧪 Laboratórios

O workshop está dividido em duas partes principais.

### Parte 1 — OCI AI Data Platform

Nesta etapa você irá construir um pipeline de dados utilizando notebooks e Apache Spark.

Você aprenderá a:

- Criar uma instância do **OCI AI Data Platform**;
- Configurar catálogo, volume, workspace e cluster Spark;
- Carregar datasets CSV;
- Realizar análise exploratória e verificações de qualidade;
- Criar a camada **Bronze**;
- Criar a camada **Silver**;
- Criar a camada **Gold**;
- Trabalhar com **PySpark**;
- Integrar o AI Data Platform com o Autonomous Database;
- Replicar datasets processados para o Autonomous Database;
- Criar um **Workflow** para orquestração dos notebooks.

### Pipeline

```text
orders.csv ─────┐
                ├──► Bronze ──► Silver ──► Gold
customers.csv ──┘                           │
                                           ▼
                              Autonomous Database
```

A camada **Bronze** mantém os dados importados.

A camada **Silver** combina e prepara os dados de clientes e pedidos.

A camada **Gold** produz indicadores agregados, como:

- quantidade de pedidos;
- total de vendas;
- ticket médio;
- indicadores por classe de cliente.

---

### Parte 2 — Oracle Autonomous Database 26ai

Depois da construção do pipeline, o workshop continua no **Oracle Autonomous Database 26ai**.

Nesta etapa você aprenderá a:

- Criar schemas e usuários;
- Configurar privilégios;
- Criar procedures para transformação e replicação de dados;
- Automatizar execuções com **Oracle Scheduler**;
- Configurar credenciais OCI;
- Criar um perfil do **Select AI**;
- Consultar dados utilizando linguagem natural;
- Utilizar **Oracle APEX**;
- Configurar **Data Redaction**;
- Criar serviços REST utilizando **ORDS**.

---

## 🤖 Select AI

Uma das etapas do workshop demonstra como utilizar o **Select AI** para realizar consultas sobre os dados utilizando linguagem natural.

Exemplos:

```text
Qual a quantidade de ordens por delivery_type?
```

```text
Qual a quantidade de ordens por customer_class?
```

```text
Quais as ordens efetuadas pelo email alfred.foley@yahoo.com?
```

```text
Qual a quantidade de ordens por warehouse_id?
```

O Select AI interpreta a pergunta e utiliza o contexto das tabelas para auxiliar na geração das consultas correspondentes.

---

## 🔐 Data Redaction

O workshop também apresenta um exemplo de proteção de informações utilizando **Oracle Data Redaction**.

Uma política de redaction é aplicada à coluna de e-mail dos clientes para demonstrar como dados sensíveis podem ser protegidos dependendo do contexto de acesso.

---

## 🌐 REST API com ORDS

Na etapa final, os dados são disponibilizados através de um endpoint REST utilizando **Oracle REST Data Services (ORDS)**.

Exemplo conceitual:

```text
Client
   │
   ▼
ORDS REST API
   │
   ▼
Autonomous Database
   │
   └── Data Redaction
```

Isso demonstra como datasets processados pela plataforma podem ser disponibilizados para aplicações e outros consumidores através de APIs.

---

## 📁 Estrutura do repositório

```text
workshop-dataplatform/
│
├── aidataplatform/
│   └── Materiais do laboratório de OCI AI Data Platform
│
├── autonomousdb/
│   └── Materiais do laboratório de Autonomous Database
│
├── workshops/
│   └── Materiais relacionados aos workshops
│
├── 0-pt-config-lab/
│   └── Recursos de preparação/configuração do laboratório
│
├── index.html
├── manifest.json
└── README.md
```

---

## 🛠️ Tecnologias utilizadas

| Tecnologia | Utilização |
|---|---|
| Oracle Cloud Infrastructure | Plataforma cloud |
| OCI AI Data Platform | Plataforma de processamento de dados |
| Apache Spark | Processamento distribuído |
| PySpark | Transformação e análise |
| Delta Tables | Persistência das camadas de dados |
| Autonomous Database 26ai | Banco de dados |
| PL/SQL | Procedures e automações |
| Oracle Scheduler | Orquestração no banco |
| Select AI | Consultas em linguagem natural |
| Oracle APEX | Aplicação demonstrativa |
| Data Redaction | Proteção de dados |
| ORDS | Disponibilização de APIs REST |

---

## 📋 Pré-requisitos

Antes de iniciar, recomenda-se possuir:

- Uma conta Oracle Cloud com acesso aos serviços utilizados;
- Permissões para criação dos recursos necessários;
- Conhecimentos básicos de SQL;
- Conhecimentos básicos de Python são úteis, mas não obrigatórios;
- Familiaridade básica com conceitos de engenharia e processamento de dados.

---

## ▶️ Como executar o workshop

### 1. Acessa o conteudo

Acesse esse github como página web: https://caiogusto2.github.io/workshop-dataplatform

### 2. Prepare o ambiente

Siga inicialmente as instruções de preparação do laboratório e crie os recursos OCI necessários.

### 3. Execute a Parte 1

Comece pelo laboratório de **OCI AI Data Platform**.

Construa as camadas:

```text
Bronze → Silver → Gold
```

e replique os resultados para o Autonomous Database.

### 4. Execute a Parte 2

Depois de concluir a primeira parte, continue com o laboratório de **Oracle Autonomous Database 26ai**.

Nesta etapa você utilizará os datasets criados anteriormente para explorar automação, Select AI, APEX, segurança e APIs REST.

---

## ⚠️ Importante

Este workshop cria recursos na Oracle Cloud que podem gerar consumo de créditos ou custos dependendo do tipo de conta e das configurações utilizadas.

Ao finalizar o laboratório, revise os recursos provisionados e remova aqueles que não serão mais necessários.

Nunca armazene no repositório:

- senhas;
- private keys;
- API keys;
- wallets;
- tokens;
- OCIDs ou outras credenciais sensíveis que não devam ser públicas.

---

## 👥 Autores

**Autor**

- Caio Oliveira

**Autora contribuinte**

- Isabelle Anjos

---

## 🛡️ Safe Harbor

O conteúdo deste workshop tem finalidade exclusivamente educacional e informativa.

As funcionalidades, recursos, serviços, disponibilidade e demais características dos produtos Oracle apresentados neste material podem ser alterados a qualquer momento.

O conteúdo não representa compromisso de entrega de qualquer material, código ou funcionalidade e não deve ser utilizado como base para decisões de compra.

---

## 📚 Recursos

- [Oracle Cloud Infrastructure Documentation](https://docs.oracle.com/en-us/iaas/)
- [Oracle Autonomous Database Documentation](https://docs.oracle.com/en/cloud/paas/autonomous-database/)
- [Oracle APEX](https://apex.oracle.com/)
- [Oracle REST Data Services](https://www.oracle.com/database/technologies/appdev/rest.html)

---

## ⭐ Sobre este projeto

Este repositório foi criado para apoiar workshops e demonstrações práticas de uma arquitetura moderna de dados utilizando tecnologias Oracle.

Se este conteúdo foi útil, considere adicionar uma ⭐ ao repositório.