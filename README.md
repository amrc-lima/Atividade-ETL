# 📊 Pipelines de Dados com Apache Hop

## 🧠 Sobre o Projeto

Este projeto implementa um processo de **ETL (Extração, Transformação e Carga)** utilizando o **Apache Hop**, com o objetivo de extrair e processar dados do **Currículo Lattes**.

A solução demonstra, de forma prática, como orquestrar pipelines de dados, integrando diferentes etapas de processamento até a visualização final dos dados.

---

## 🎯 Objetivo

Construir um fluxo completo de dados que:

* Extraia dados de arquivos XML (Currículo Lattes)
* Transforme e normalize as informações
* Carregue os dados em um banco PostgreSQL
* Disponibilize os dados para análise no Power BI

---

## 🏗️ Arquitetura do Projeto

O fluxo segue a seguinte estrutura:

1. **Fonte de Dados**

   * Arquivos XML do Currículo Lattes

2. **Processamento (Apache Hop)**

   * Leitura dos XMLs
   * Extração de dados dos pesquisadores
   * Extração de produções científicas (artigos)
   * Transformações e joins

3. **Armazenamento**

   * Banco de dados PostgreSQL (Docker)

4. **Visualização**

   * Power BI (gráfico de artigos indexados)

---

## 🐳 Banco de Dados (Docker + PostgreSQL)

O banco de dados foi configurado via Docker utilizando PostgreSQL.

### 🔧 Build da imagem

```bash
docker build -t docker_simcc .
```

### ▶️ Execução do container

```bash
docker run -d --name docker_simcc -p 5437:5432 docker_simcc
```

### 🗄️ Estrutura do Banco

#### Tabela: pesquisadores

* pesquisadores_id (UUID)
* lattes_id (VARCHAR)
* nome (VARCHAR)

#### Tabela: producoes

* producoes_id (UUID)
* pesquisadores_id (UUID)
* issn (VARCHAR)
* nomeArtigo (TEXT)
* anoArtigo (INTEGER)

---

## 🔄 Pipelines no Apache Hop

### 📌 Pipeline 1: Dados do Pesquisador

* Leitura do XML
* Extração do nome completo
* Extração do ID Lattes
* Inserção no banco

---

### 📌 Pipeline 2: Vários Pesquisadores

* Leitura de múltiplos arquivos XML
* Processamento em lote
* Inserção na tabela de pesquisadores

---

### 📌 Pipeline 3: Produções Científicas

* Extração de artigos publicados
* Leitura do caminho XML:

  ```
  /CURRICULO-VITAE/PRODUCAO-BIBLIOGRAFICA/ARTIGOS-PUBLICADOS
  ```
* Associação com o pesquisador (via ID Lattes)
* Inserção na tabela `producoes`

---

### 🔗 Integração de Dados

* Join entre pesquisadores e produções
* Ordenação por ID
* Garantia de integridade relacional

---

## 🔁 Workflow

Foi criado um workflow no Apache Hop para:

* Orquestrar a execução dos pipelines
* Garantir a ordem correta de processamento
* Automatizar o fluxo ETL

---

## 📊 Visualização no Power BI

* Conexão com PostgreSQL
* Modelagem de relacionamento entre tabelas
* Criação de gráfico de colunas clusterizado
* Análise de artigos indexados por pesquisador

---

## 🛠️ Tecnologias Utilizadas

* Apache Hop
* PostgreSQL
* Docker
* Power BI

---

## 🚀 Como Executar

1. Suba o banco com Docker
2. Configure a conexão no Apache Hop
3. Execute os pipelines na seguinte ordem:

   * Dados do Pesquisador
   * Vários Pesquisadores
   * Produções
4. Execute o workflow
5. Conecte o Power BI ao banco

---

## 📌 Resultado

O projeto gera uma base estruturada de pesquisadores e suas produções científicas, permitindo análises visuais sobre artigos publicados.

---

## 👨‍💻 Autor

Projeto desenvolvido como atividade acadêmica para a disciplina de Engenharia de Dados / ETL.

---

## 📎 Observações

* Os arquivos XML devem estar organizados em diretórios acessíveis ao pipeline
* O projeto pode ser expandido para incluir mais métricas e análises
* Estrutura ideal para estudos de pipelines e integração de dados

---
