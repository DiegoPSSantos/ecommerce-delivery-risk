# Fase 2 — Entendimento e Aquisição dos Dados

## Dataset selecionado

Para este projeto, será utilizado o dataset público **Brazilian E-Commerce Public Dataset by Olist**, disponível no Kaggle.

Embora o projeto seja apresentado com a empresa fictícia **NexaMarket**, os dados utilizados são baseados em uma operação real de e-commerce brasileiro, disponibilizados de forma anonimizada para fins educacionais e analíticos.

Esse dataset foi escolhido porque possui características muito próximas ao problema de negócio definido na Fase 1. Ele contém informações sobre pedidos, clientes, vendedores, produtos, pagamentos, frete, prazos de entrega, avaliações e localização geográfica, permitindo construir uma solução preditiva para estimar o risco de atraso na entrega.

## Justificativa da escolha

O dataset foi considerado adequado para este projeto por quatro motivos principais.

Primeiro, ele representa um cenário realista de e-commerce, com pedidos realizados por clientes em diferentes regiões do Brasil e enviados por diferentes vendedores.

Segundo, contém informações temporais relevantes, como data de compra, data estimada de entrega e data real de entrega ao cliente. Essas variáveis permitem criar a variável-alvo do projeto, indicando se um pedido atrasou ou não.

Terceiro, apresenta dados comerciais e logísticos importantes, como valor do pedido, valor do frete, quantidade de itens, localização do cliente, localização do vendedor, categoria do produto e tipo de pagamento.

Quarto, permite a construção de um projeto completo de Ciência de Dados, passando por integração de múltiplas tabelas, limpeza de dados, análise exploratória, engenharia de atributos, modelagem, explicabilidade e implantação de uma solução simples em produção.

## Arquivos esperados

Após o download do dataset, espera-se encontrar os seguintes arquivos principais:

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

Cada arquivo representa uma dimensão diferente da operação de e-commerce.

## Descrição das tabelas

### 1. Pedidos

Arquivo:

```text
olist_orders_dataset.csv
```

Essa será a tabela principal do projeto, pois contém o registro dos pedidos e suas principais datas.

Colunas relevantes esperadas:

```text
order_id
customer_id
order_status
order_purchase_timestamp
order_approved_at
order_delivered_carrier_date
order_delivered_customer_date
order_estimated_delivery_date
```

Essa tabela será usada para identificar quando o pedido foi realizado, qual era a data estimada de entrega e quando ele foi efetivamente entregue ao cliente.

A partir dela será criada a variável-alvo `atrasou_entrega`.

### 2. Itens dos pedidos

Arquivo:

```text
olist_order_items_dataset.csv
```

Essa tabela contém os produtos associados a cada pedido.

Colunas relevantes esperadas:

```text
order_id
order_item_id
product_id
seller_id
shipping_limit_date
price
freight_value
```

Ela será usada para calcular informações como quantidade de itens, valor total dos produtos, valor total do frete e identificação do vendedor responsável.

Como um pedido pode ter mais de um item, essa tabela precisará ser agregada por `order_id` antes de ser integrada à base analítica final.

### 3. Clientes

Arquivo:

```text
olist_customers_dataset.csv
```

Essa tabela contém informações de localização dos clientes.

Colunas relevantes esperadas:

```text
customer_id
customer_unique_id
customer_zip_code_prefix
customer_city
customer_state
```

Ela será usada para identificar o estado, cidade e prefixo de CEP do cliente. Essas informações podem ajudar a explicar diferenças regionais no risco de atraso.

### 4. Vendedores

Arquivo:

```text
olist_sellers_dataset.csv
```

Essa tabela contém informações de localização dos vendedores.

Colunas relevantes esperadas:

```text
seller_id
seller_zip_code_prefix
seller_city
seller_state
```

Ela será usada para identificar a localização do vendedor e permitir análises de distância aproximada entre vendedor e cliente.

### 5. Produtos

Arquivo:

```text
olist_products_dataset.csv
```

Essa tabela contém informações sobre os produtos vendidos.

Colunas relevantes esperadas:

```text
product_id
product_category_name
product_name_lenght
product_description_lenght
product_photos_qty
product_weight_g
product_length_cm
product_height_cm
product_width_cm
```

Ela será usada para identificar a categoria do produto e características físicas que podem influenciar o prazo e o risco logístico, como peso e dimensões.

### 6. Pagamentos

Arquivo:

```text
olist_order_payments_dataset.csv
```

Essa tabela contém informações sobre os pagamentos realizados nos pedidos.

Colunas relevantes esperadas:

```text
order_id
payment_sequential
payment_type
payment_installments
payment_value
```

Ela será usada para avaliar se o tipo de pagamento, o número de parcelas ou o valor pago possuem alguma relação com atrasos.

Como um pedido pode ter mais de um pagamento, essa tabela também precisará ser agregada por `order_id`.

### 7. Avaliações

Arquivo:

```text
olist_order_reviews_dataset.csv
```

Essa tabela contém avaliações feitas pelos clientes após a compra.

Colunas relevantes esperadas:

```text
review_id
order_id
review_score
review_comment_title
review_comment_message
review_creation_date
review_answer_timestamp
```

Essa tabela poderá ser usada na análise exploratória para avaliar o impacto dos atrasos na satisfação do cliente.

No entanto, suas variáveis não devem ser usadas como entrada do modelo preditivo principal, pois a avaliação normalmente ocorre após a entrega ou após a experiência de compra. Portanto, usar esses dados no momento da previsão poderia gerar vazamento de dados.

### 8. Geolocalização

Arquivo:

```text
olist_geolocation_dataset.csv
```

Essa tabela contém informações aproximadas de latitude e longitude associadas aos prefixos de CEP.

Colunas relevantes esperadas:

```text
geolocation_zip_code_prefix
geolocation_lat
geolocation_lng
geolocation_city
geolocation_state
```

Ela poderá ser usada para estimar a distância aproximada entre cliente e vendedor, criando uma variável importante para o modelo.

### 9. Tradução de categorias

Arquivo:

```text
product_category_name_translation.csv
```

Essa tabela contém a tradução dos nomes das categorias dos produtos.

Colunas relevantes esperadas:

```text
product_category_name
product_category_name_english
```

Ela será usada para facilitar a leitura das categorias durante a análise exploratória e apresentação dos resultados.

## Tabela principal da base analítica

A base analítica final deverá ter uma linha por pedido.

A tabela central será a de pedidos, e as demais tabelas serão integradas a ela por meio das chaves disponíveis.

A chave principal da base será:

```text
order_id
```

A estrutura lógica será:

```text
orders
  ├── customers
  ├── order_items
  │     ├── sellers
  │     └── products
  │           └── category_translation
  ├── payments
  └── geolocation
```

O objetivo é transformar várias tabelas relacionais em uma única tabela de modelagem, onde cada linha representa um pedido e cada coluna representa uma característica disponível para previsão.

## Criação da variável-alvo

A variável-alvo será chamada de:

```text
atrasou_entrega
```

Ela será criada a partir da comparação entre a data real de entrega e a data estimada de entrega.

Regra:

```text
atrasou_entrega = 1
```

Quando:

```text
order_delivered_customer_date > order_estimated_delivery_date
```

E:

```text
atrasou_entrega = 0
```

Quando:

```text
order_delivered_customer_date <= order_estimated_delivery_date
```

Pedidos sem data real de entrega precisarão ser analisados com cuidado. Em um primeiro momento, a recomendação é considerar apenas pedidos com status de entrega concluída, pois o objetivo inicial é treinar o modelo com casos em que já se conhece o resultado final.

## Variáveis permitidas para modelagem

Como a previsão deve ocorrer no momento da confirmação do pedido, só poderão ser utilizadas variáveis conhecidas até esse momento.

Exemplos de variáveis permitidas:

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

Essas variáveis são compatíveis com o momento da previsão, pois representam informações comerciais, geográficas, temporais e logísticas conhecidas antes da entrega acontecer.

## Variáveis proibidas para modelagem

Algumas variáveis não poderão ser utilizadas como entrada do modelo, pois só são conhecidas após o avanço do processo logístico ou após a conclusão da entrega.

Exemplos de variáveis proibidas:

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

A variável `order_delivered_customer_date` será usada apenas para criar a variável-alvo, mas não poderá ser usada como atributo explicativo.

A variável `order_status` também deve ser tratada com cuidado. Como ela pode refletir o estado final do pedido, seu uso pode causar vazamento de dados. Para a primeira versão do modelo, ela será excluída das features.

## Estratégia de integração dos dados

A integração dos dados será feita em etapas.

Primeiro, a tabela de pedidos será filtrada para manter apenas pedidos entregues e com datas válidas.

Depois, a tabela de itens será agregada por pedido, gerando variáveis como:

```text
quantidade_itens
valor_total_produtos
valor_total_frete
quantidade_vendedores
```

Em seguida, os dados de clientes serão adicionados pela chave `customer_id`.

Depois, os dados dos vendedores serão adicionados a partir dos itens do pedido. Quando um pedido tiver mais de um vendedor, será necessário definir uma regra de agregação, como considerar o primeiro vendedor, o vendedor principal ou criar uma variável indicando múltiplos vendedores.

Os dados de produtos serão integrados para trazer a categoria e características físicas dos produtos. Quando um pedido tiver mais de um produto, será necessário criar agregações, como peso total, volume médio ou categoria principal.

Os dados de pagamento serão agregados por pedido, permitindo identificar valor total pago, tipo principal de pagamento e número máximo de parcelas.

Por fim, os dados de geolocalização poderão ser usados para estimar a distância aproximada entre cliente e vendedor.

## Estrutura de armazenamento dos dados

Os arquivos originais deverão ser armazenados na pasta:

```text
data/raw/
```

Os arquivos tratados e consolidados deverão ser armazenados na pasta:

```text
data/processed/
```

A base analítica final poderá ser salva como:

```text
data/processed/abm_delivery_risk.csv
```

A sigla `abm` representa `analytical base model`, ou base analítica de modelagem.

## Cuidados importantes

Durante esta fase, será necessário tomar alguns cuidados.

O primeiro cuidado é preservar os dados brutos sem alterações. Qualquer tratamento deve ser feito em novos arquivos dentro da pasta `data/processed/`.

O segundo cuidado é documentar todas as decisões de integração, especialmente quando uma tabela tiver múltiplas linhas para o mesmo pedido.

O terceiro cuidado é evitar vazamento de dados. Variáveis que representam eventos posteriores à confirmação do pedido não devem ser usadas na modelagem.

O quarto cuidado é verificar a qualidade das datas, pois a criação da variável-alvo depende diretamente da comparação entre data estimada e data real de entrega.

O quinto cuidado é registrar o dicionário de dados do projeto, explicando o significado de cada variável criada.

## Entregáveis da Fase 2

Ao final da Fase 2, o projeto deverá conter:

```text
data/raw/
data/processed/
docs/data_understanding.md
notebooks/02_data_understanding.ipynb
```

Além disso, espera-se que estejam definidos:

```text
dataset utilizado
arquivos disponíveis
descrição das tabelas
chaves de relacionamento
variável-alvo
variáveis permitidas
variáveis proibidas
estratégia de integração
cuidados contra vazamento de dados
```

Com essa etapa concluída, o projeto estará pronto para avançar para a preparação dos dados, limpeza, tratamento de inconsistências e construção da primeira versão da base analítica.
