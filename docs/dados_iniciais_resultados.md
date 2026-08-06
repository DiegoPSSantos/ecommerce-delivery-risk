# Relatório de Achados Iniciais dos Dados

## Projeto

**Predição de Atraso de Entregas em E-commerce — NexaMarket**

## Objetivo deste relatório

Este relatório apresenta os principais achados iniciais da etapa de entendimento dos dados do projeto de predição de atraso de entregas da empresa fictícia **NexaMarket**.

O objetivo desta fase é conhecer a estrutura dos dados disponíveis, avaliar a qualidade inicial das tabelas, identificar chaves de relacionamento, verificar valores ausentes e antecipar possíveis riscos para as próximas etapas do projeto.

Nesta fase ainda não foi realizada modelagem preditiva. O foco está na compreensão dos dados e na avaliação de sua adequação ao problema de negócio.

---

## Dataset analisado

O projeto utiliza o dataset público **Brazilian E-Commerce Public Dataset by Olist**, composto por múltiplas tabelas relacionadas a uma operação de e-commerce no Brasil.

As tabelas analisadas inicialmente foram:

- pedidos;
- itens dos pedidos;
- clientes;
- vendedores;
- produtos;
- pagamentos;
- avaliações;
- geolocalização;
- tradução das categorias de produtos.

Essas tabelas permitem representar diferentes dimensões da operação, como dados comerciais, logísticos, geográficos, temporais e de satisfação do cliente.

---

## Arquivos carregados

Foram carregados os seguintes arquivos CSV na pasta `data/raw/`:

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

No total, foram analisados **9 arquivos**.

---

## Visão geral das tabelas

A análise inicial identificou que o dataset está estruturado em formato relacional, com diferentes tabelas conectadas principalmente pelas chaves `order_id`, `customer_id`, `product_id` e `seller_id`.

Resumo inicial das tabelas:

| Tabela | Descrição | Linhas | Colunas |
|---|---|---:|---:|
| pedidos | Informações principais dos pedidos e datas logísticas | 99441 | 8 |
| itens_pedido | Produtos, vendedores, preços e fretes por item do pedido | 112650 | 5 |
| clientes | Dados de localização dos clientes | 99441 | 5 |
| vendedores | Dados de localização dos vendedores | 3095 | 4 |
| produtos | Características e categorias dos produtos | 32951 | 9 |
| pagamentos | Formas de pagamento, parcelas e valores pagos | 103886 | 5 |
| avaliacoes | Avaliações realizadas pelos clientes | 99224 | 7 |
| coordenadas | Latitude e longitude aproximadas por prefixo de CEP | 1000163 | 5 |
| produto_categoria | Tradução das categorias de produtos | 71 | 2 |

---

## Chaves de relacionamento identificadas

As principais chaves identificadas foram:

| Chave | Uso principal |
|---|---|
| `order_id` | Relaciona pedidos com itens, pagamentos e avaliações |
| `customer_id` | Relaciona pedidos com clientes |
| `product_id` | Relaciona itens dos pedidos com produtos |
| `seller_id` | Relaciona itens dos pedidos com vendedores |
| `product_category_name` | Relaciona produtos com a tabela de tradução de categorias |
| `zip_code_prefix` | Permite aproximação geográfica entre cliente e vendedor |

A tabela `pedidos` será a tabela central da base analítica, pois representa o pedido e contém as datas necessárias para criação da variável-alvo.

---

## Período histórico dos pedidos

A análise das datas dos pedidos indicou que o dataset cobre o período entre:

Data inicial dos pedidos: **2016-09-04 21:15:19**
Data final dos pedidos: **2018-10-17 17:30:18**

Esse período histórico será importante para análises temporais, criação de variáveis de calendário e separação entre treino e teste de forma cronológica.

---

## Status dos pedidos

A coluna `order_status` apresenta diferentes situações possíveis para os pedidos, como pedidos entregues, cancelados, enviados ou em processamento.

Para a criação inicial da variável-alvo, a recomendação é considerar apenas pedidos com status **delivered**, pois nesses casos existe informação final sobre a data real de entrega.

Pedidos não entregues, cancelados ou sem data real de entrega devem ser analisados separadamente, pois não permitem confirmar se houve ou não atraso.

---

## Criação inicial da variável-alvo

A variável-alvo do projeto será chamada de `atrasou_entrega`.

Ela será criada com a seguinte regra:

```text
atrasou_entrega = 1, quando order_delivered_customer_date > order_estimated_delivery_date
atrasou_entrega = 0, quando order_delivered_customer_date <= order_estimated_delivery_date
```

Essa variável representa se o pedido foi entregue após a data prometida ao cliente.

Na primeira análise, foram considerados apenas pedidos entregues e com datas válidas de entrega real e entrega estimada.

Quantidade de pedidos entregues analisados:

```text
96470
```

Taxa inicial de atraso identificada:

```text
8.11%
```

Essa taxa será fundamental para avaliar se a variável-alvo é balanceada ou desbalanceada e para definir as métricas mais adequadas na etapa de modelagem.

---

## Valores ausentes

A análise inicial identificou valores ausentes em algumas tabelas, especialmente em colunas relacionadas a datas logísticas, informações de produtos e avaliações.

Colunas com valores ausentes relevantes deverão ser avaliadas com cuidado nas próximas fases, pois podem representar:

- pedidos ainda não concluídos;
- informações não registradas no sistema;
- dados opcionais;
- inconsistências de preenchimento;
- campos que só existem após determinada etapa do processo.

Na próxima fase, cada coluna com valores ausentes será classificada conforme sua importância para o projeto e sua disponibilidade no momento da previsão.

---

## Variáveis com risco de vazamento de dados

Foram identificadas algumas variáveis que não devem ser utilizadas como entrada do modelo, pois representam informações conhecidas apenas após o avanço ou conclusão do processo logístico.

Exemplos de variáveis com risco de vazamento:

```text
order_delivered_customer_date
order_delivered_carrier_date
review_score
review_comment_title
review_comment_message
review_creation_date
review_answer_timestamp
order_status
```

A coluna `order_delivered_customer_date` será utilizada apenas para criar a variável-alvo, mas não poderá ser usada como feature do modelo.

As informações de avaliação do cliente também não deverão ser utilizadas na modelagem principal, pois normalmente são geradas após a experiência de compra.

---

## Variáveis candidatas para modelagem

As variáveis candidatas para modelagem serão aquelas disponíveis até o momento da confirmação do pedido.

Exemplos iniciais:

```text
customer_state
customer_city
seller_state
seller_city
product_category_name
price
freight_value
payment_type
payment_installments
order_purchase_timestamp
order_approved_at
order_estimated_delivery_date
quantidade_itens
valor_total_produtos
valor_total_frete
prazo_prometido_dias
dia_semana_compra
mes_compra
distancia_cliente_vendedor
```

Essas variáveis ainda precisarão passar por tratamento, agregação e validação antes de serem utilizadas na modelagem.

---

## Principais achados iniciais

A etapa inicial de entendimento dos dados trouxe os seguintes achados:

1. O dataset possui estrutura relacional e exige integração de múltiplas tabelas para construção da base analítica.

2. A tabela `pedidos` será a base central do projeto, pois contém as principais datas necessárias para criação da variável-alvo.

3. A variável `atrasou_entrega` pode ser criada a partir da comparação entre a data real de entrega e a data estimada de entrega.

4. Nem todos os pedidos devem ser usados na criação inicial do alvo, pois pedidos não entregues ou sem data real de entrega não permitem confirmar o resultado.

5. Algumas tabelas possuem múltiplas linhas por pedido, como itens e pagamentos, exigindo agregações antes da integração.

6. Existem variáveis com alto risco de vazamento de dados, especialmente aquelas relacionadas à entrega final, avaliações e status posterior do pedido.

7. A localização de clientes e vendedores pode permitir a criação de variáveis geográficas relevantes, como distância aproximada entre origem e destino.

8. A análise inicial da taxa de atraso será importante para orientar a escolha das métricas de avaliação do modelo.

---

## Decisões tomadas nesta fase

Com base na análise inicial, foram tomadas as seguintes decisões:

- utilizar `pedidos` como tabela central da base analítica;
- considerar inicialmente apenas pedidos entregues para criação da variável-alvo;
- criar a variável `atrasou_entrega` com base na comparação entre entrega real e entrega estimada;
- excluir variáveis pós-entrega da etapa de modelagem;
- agregar tabelas com múltiplas linhas por pedido antes da união final;
- manter os dados brutos preservados na pasta `data/raw/`;
- salvar bases tratadas futuramente na pasta `data/processed/`.

---

## Próximos passos

A próxima etapa do projeto será a preparação dos dados.

As principais atividades previstas são:

1. converter colunas de datas para o tipo adequado;
2. filtrar pedidos válidos para modelagem;
3. criar a variável-alvo definitiva;
4. agregar itens, pagamentos e produtos por pedido;
5. integrar clientes, vendedores e geolocalização;
6. tratar valores ausentes;
7. criar variáveis derivadas;
8. salvar a primeira versão da base analítica em `data/processed/`.

---

## Conclusão

A Fase 2 confirmou que o dataset selecionado é adequado para o problema de negócio proposto.

Os dados disponíveis permitem representar diferentes aspectos da operação de e-commerce, incluindo pedidos, produtos, clientes, vendedores, pagamentos, fretes, prazos e localização geográfica.

Apesar disso, o projeto exige cuidados importantes com integração de tabelas, tratamento de valores ausentes, prevenção de vazamento de dados e definição correta das variáveis disponíveis no momento da previsão.

Com esses cuidados, será possível avançar para a construção de uma base analítica consistente para modelagem preditiva de atraso de entregas.