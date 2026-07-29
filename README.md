# Predição de Atraso de Entregas na NexaMarket

## Sobre o projeto

Este projeto tem como objetivo desenvolver uma solução completa de Ciência de Dados para prever o risco de atraso na entrega de pedidos em uma empresa fictícia de e-commerce chamada **NexaMarket**.

A proposta é simular um projeto real de mercado, passando pelas principais fases de um ciclo de Ciência de Dados: entendimento do problema de negócio, entendimento dos dados, preparação, análise exploratória, engenharia de atributos, modelagem preditiva, avaliação, explicabilidade e futura implantação da solução.

O projeto está sendo construído com foco em portfólio profissional, publicação no Kaggle e apresentação no LinkedIn, demonstrando não apenas habilidades técnicas, mas também capacidade de transformar um problema de negócio em uma solução orientada por dados.

---

## Contexto de negócio

A **NexaMarket** é uma empresa fictícia de e-commerce nacional que comercializa produtos de diferentes categorias por meio de vendedores parceiros e centros de distribuição.

Nos últimos períodos, a empresa identificou um aumento nas reclamações relacionadas a atrasos nas entregas. Atualmente, a atuação das equipes de logística e atendimento ocorre de forma majoritariamente reativa, ou seja, somente após o atraso ser percebido pelo cliente.

Esse cenário gera impactos como:

* aumento no volume de contatos no atendimento;
* maior custo operacional;
* piora na experiência do cliente;
* queda na reputação da empresa;
* risco de cancelamentos e reembolsos;
* dificuldade para identificar gargalos logísticos;
* atuação tardia sobre pedidos críticos.

Diante disso, a empresa precisa de uma solução capaz de antecipar quais pedidos possuem maior probabilidade de atraso, permitindo uma atuação preventiva das áreas de logística, operações e atendimento.

---

## Objetivo do projeto

O objetivo é desenvolver um modelo de classificação capaz de prever, no momento da confirmação do pedido, a probabilidade de que uma entrega ocorra após a data prometida ao cliente.

A solução deverá apoiar decisões como:

* priorização de pedidos com alto risco de atraso;
* acionamento preventivo de vendedores ou transportadoras;
* comunicação antecipada com o cliente;
* identificação de regiões, categorias ou vendedores com maior risco;
* melhoria dos prazos prometidos;
* redução de reclamações e custos operacionais.

---

## Tipo de problema

Este projeto será tratado como um problema de **classificação binária**.

A variável-alvo será chamada de:

```python
atrasou_entrega
```

Ela será definida da seguinte forma:

* `1`: pedido entregue após a data estimada;
* `0`: pedido entregue dentro do prazo ou antes da data estimada.

A regra de criação será:

```python
atrasou_entrega = order_delivered_customer_date > order_estimated_delivery_date
```

A data real de entrega será usada apenas para criar a variável-alvo, mas não será utilizada como entrada do modelo, pois essa informação só é conhecida após a conclusão da entrega.

---

## Dataset utilizado

O projeto utilizará o dataset público **Brazilian E-Commerce Public Dataset by Olist**, disponível no Kaggle.

Embora a empresa do projeto seja fictícia, os dados representam um cenário realista de e-commerce brasileiro, com informações sobre:

* pedidos;
* clientes;
* vendedores;
* produtos;
* pagamentos;
* fretes;
* avaliações;
* localização geográfica;
* prazos estimados e datas reais de entrega.

---

## Principais arquivos do dataset

Os principais arquivos utilizados serão:

```text
olist_orders_dataset.csv
olist_order_items_dataset.csv
olist_customers_dataset.csv
olist_sellers_dataset.csv
olist_products_dataset.csv
olist_order_payments_dataset.csv
olist_order_reviews_dataset.csv
olist_geolocation_dataset.csv
product_category_name_translation.csv
```

Cada tabela será analisada e integrada para formar uma base analítica final com uma linha por pedido.

---

## Estrutura inicial do projeto

```text
ecommerce-delivery-risk/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── docs/
│   ├── business_understanding.md
│   └── data_understanding.md
│
├── notebooks/
│   ├── 01_business_understanding.ipynb
│   └── 02_data_understanding.ipynb
│
├── src/
│   ├── data/
│   ├── features/
│   ├── models/
│   └── utils/
│
├── app/
│   └── streamlit_app.py
│
├── README.md
├── requirements.txt
└── .gitignore
```

---

## Fases do projeto

### 1. Entendimento do negócio

Nesta fase, foram definidos:

* problema de negócio;
* objetivo do projeto;
* usuários da solução;
* variável-alvo;
* momento da previsão;
* impacto esperado;
* possíveis ações após a previsão;
* principais riscos;
* restrições;
* critérios de sucesso.

### 2. Entendimento dos dados

Nesta fase, o objetivo é conhecer a estrutura do dataset, avaliando:

* arquivos disponíveis;
* quantidade de linhas e colunas;
* tipos de dados;
* valores ausentes;
* chaves de relacionamento;
* período histórico dos pedidos;
* colunas permitidas e proibidas para modelagem;
* riscos de vazamento de dados.

### 3. Preparação dos dados

Nesta etapa serão realizados tratamentos como:

* conversão de datas;
* tratamento de valores ausentes;
* remoção de inconsistências;
* agregação de tabelas;
* criação da variável-alvo;
* construção da base analítica de modelagem.

### 4. Análise exploratória dos dados

A análise exploratória buscará responder perguntas como:

* qual é a taxa geral de atraso?
* quais estados possuem maior risco?
* determinadas categorias atrasam mais?
* o valor do frete influencia o atraso?
* pedidos mais distantes possuem maior risco?
* existe sazonalidade nos atrasos?

### 5. Engenharia de atributos

Serão criadas variáveis como:

* prazo prometido em dias;
* dia da semana da compra;
* mês da compra;
* quantidade de itens;
* valor total do pedido;
* valor total do frete;
* distância aproximada entre cliente e vendedor;
* características do produto;
* histórico de atraso por vendedor ou categoria, respeitando a ordem temporal.

### 6. Modelagem preditiva

Serão avaliados diferentes modelos de classificação, como:

* Regressão Logística;
* Árvore de Decisão;
* Random Forest;
* XGBoost ou LightGBM.

As métricas previstas incluem:

* Accuracy;
* Precision;
* Recall;
* F1-score;
* ROC-AUC;
* Matriz de confusão;
* PR-AUC, caso a base seja desbalanceada.

### 7. Explicabilidade

O projeto buscará explicar quais fatores mais influenciam o risco de atraso, utilizando técnicas como:

* importância das variáveis;
* análise de erros;
* SHAP, quando aplicável.

### 8. Implantação

A solução final será demonstrada por meio de uma aplicação simples, possivelmente desenvolvida com Streamlit, permitindo simular o risco de atraso para novos pedidos.

### 9. Monitoramento

Também será documentado como a solução poderia ser monitorada em produção, considerando:

* performance do modelo ao longo do tempo;
* drift de dados;
* taxa real de atraso;
* falsos positivos;
* falsos negativos;
* volume de pedidos classificados como alto risco.

---

## Status atual

O projeto encontra-se na **Fase 2 — Entendimento dos Dados**.

Até o momento, já foram concluídas:

* definição do problema de negócio;
* escolha da empresa fictícia NexaMarket;
* documentação da Fase 1;
* seleção do dataset;
* documentação inicial da Fase 2;
* organização dos primeiros documentos no GitHub.

---

## Tecnologias previstas

As principais tecnologias e bibliotecas previstas para o projeto são:

* Python;
* Pandas;
* NumPy;
* Matplotlib;
* Scikit-learn;
* XGBoost ou LightGBM;
* SHAP;
* Streamlit;
* Jupyter Notebook;
* Git e GitHub;
* Kaggle.

---

## Boas práticas adotadas

Este projeto será desenvolvido seguindo boas práticas de Ciência de Dados, como:

* documentação clara das decisões;
* separação entre dados brutos e dados processados;
* prevenção de vazamento de dados;
* análise exploratória antes da modelagem;
* avaliação de múltiplos modelos;
* escolha de métricas alinhadas ao problema de negócio;
* explicabilidade dos resultados;
* organização do repositório para leitura por recrutadores e avaliadores técnicos.

---

## Próximos passos

Os próximos passos do projeto são:

1. criar e executar o notebook de entendimento dos dados;
2. carregar todos os arquivos CSV;
3. analisar dimensões, tipos de dados e valores ausentes;
4. validar as chaves de relacionamento;
5. criar a primeira versão da variável-alvo;
6. calcular a taxa inicial de atraso;
7. iniciar a preparação da base analítica de modelagem.

---

## Objetivo de portfólio

Este projeto faz parte de uma jornada de desenvolvimento profissional em Ciência de Dados, com foco em construir um portfólio sólido, prático e alinhado a problemas reais de negócio.

A intenção é demonstrar competências em:

* análise de dados;
* pensamento de negócio;
* modelagem preditiva;
* engenharia de atributos;
* comunicação de resultados;
* organização de projetos;
* documentação técnica;
* implantação de soluções de dados.

---

## Autor

Projeto desenvolvido por Diego Santos como parte de sua jornada de aprendizado e construção de portfólio em Ciência de Dados.
