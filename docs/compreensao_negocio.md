# Predição de Atraso de Entregas em E-commerce

## Problema de negócio

A **NexaMarket** é uma empresa de e-commerce nacional que comercializa produtos de diferentes categorias por meio de parceiros vendedores e centros de distribuição próprios. Seus pedidos são enviados para clientes localizados em diversas regiões do Brasil, utilizando diferentes transportadoras, modalidades de frete e prazos estimados de entrega.

Nos últimos 2 meses, a empresa identificou um aumento nas reclamações relacionadas a atrasos nas entregas. Os cliente relatam insatisfação principalmente quando o pedido ultrapassa a data prometida na data da compra, o que impacta negativamente a experiência do usuário, a confiança na marca e a probabilidade de recompra.

Atualmente, o acompanhamento de pedidos ocorre de forma majoritariamente reativa. Em geral, as equipes de logística e atendimento passam a atuar somente após a confirmação do atraso, quando o cliente entra em contato para solicitar informações, registrar uma reclamação ou pedir cancelamento e reeembolso.

Esse processo gera consequências relevantes para o negócio, tais como:

- aumento do volume de contatos via e-mail, chat e canais de mensagens;
- crescimento de custos operacionais relacionados à equipe de suporte;
- piora nas avaliações dos pedidos e da reputação da empresa;
- maior risco de cancelamento, reembolso ou chargeback;
- perda de confiança do consumidor e redução da taxa de recompra;
- dificuldade de identificar vendedores, categorias, regiões ou perfis logísticos com maior incidência de atraso;
- atuação tardia da área de operações, sem tempo suficiente para priorizar pedidos críticos.

A **NexaMarket** possui dados históricos sobre pedidos, clientes, vendedores, produtos, pagamentos, fretes e prazos de entrega. Entretanto, esses dados ainda não são utilizados de forma integrada para antecipar riscos logísticos no momento em que um novo pedido é confirmado.

**Diante desse cenário, a empresa precisa de uma solução orientada por dados capaz de identificar antecipadamente quais pedidos possuem maior probabilidade de serem com atraso, depois da data prometida ao cliente.**

A previsão deverá ser realizada no momento da confirmação do pedido, utilizando apenas informações disponíveis até esse momento. Com base no risco estimado, a área de operação poderá priorizar o acompanhamento dos pedidos mais críticos e adotar ações preventivas antes que o atraso ocorra.

Entre as possíveis ações para pedidos classificados como alto risco estão :

- monitoramento prioritário do status logístico;
- contato preventivo com o cliente para alinhar expectativas;
- acionamento antecipado da transportadora ou do vendedor responsável;
- priorização de separação e expedição no centro de distribuição;
- avaliação de alternativas de transporte, quando aplicável;
- análise posterior de causas recorrentes de atraso por região, vendedor, categoria ou modalidade de frete.

Portanto, o problema central da **NexaMarket** é a ausência de um mecanismo preditivo que permita antecipar atrasos de entrega e apoiar uma atuação proativa das equipes de logística, operações e atendimento ao cliente.

## Objetivo do projeto

O objetivo deste projeto é desenvolver uma solução de Ciência de Dados capaz de prever, no momento da confirmação de um pedido, a probabilidade de que a entrega ocorra após a data prometida ao cliente.

A solução deverá analisar informações históricas de pedidos, clientes, vendedores, produtos, pagamentos, fretes e prazos logísticos para identificar padrões associados a atrasos de entrega. Com isso, a NexaMarket poderá classificar novos pedidos conforme o risco de atraso e apoiar decisões operacionais de forma mais preventiva.

Do ponto de vista técnico, o projeto será tratado como um problema de **classificação binária**, no qual o modelo deverá indicar se um pedido possui maior ou menor probabilidade de atraso.

O resultado esperado não é apenas criar um modelo preditivo, mas também construir uma solução interpretável e aplicável ao contexto de negócio. A previsão deverá gerar insumos úteis para as equipes de logística, operações e atendimento, permitindo priorizar pedidos críticos antes que o problema seja percebido pelo cliente.

A solução final deverá entregar:

* uma base analítica consolidada para modelagem;
* uma análise exploratória dos fatores associados a atrasos;
* um modelo preditivo capaz de estimar o risco de atraso;
* uma explicação dos principais fatores que influenciam a previsão;
* uma interface simples para simular a classificação de novos pedidos;
* uma documentação clara para apoiar a tomada de decisão do negócio.

## Usuários da solução

A solução será utilizada principalmente pelas áreas de operações, logística, atendimento ao cliente e gestão da NexaMarket.

A equipe de **operações logísticas** utilizará a previsão para acompanhar pedidos classificados como alto risco, priorizando aqueles que exigem maior atenção durante as etapas de separação, expedição e transporte.

A equipe de **atendimento ao cliente** poderá utilizar o score de risco para atuar de forma preventiva, antecipando possíveis problemas e melhorando a comunicação com o consumidor antes que uma reclamação seja aberta.

A equipe de **gestão e planejamento** poderá acompanhar indicadores agregados de risco por região, categoria de produto, vendedor, transportadora ou período, identificando gargalos recorrentes no processo logístico.

Além disso, a solução poderá apoiar análises estratégicas sobre desempenho operacional, qualidade dos vendedores parceiros, eficiência das modalidades de frete e cumprimento dos prazos prometidos ao cliente.

## Variável-alvo

A variável-alvo do projeto será chamada de `atrasou_entrega`.

Ela indicará se um pedido foi entregue depois da data estimada informada ao cliente no momento da compra.

A regra de criação da variável será:

```python
atrasou_entrega = 1
```

Quando a data real de entrega ao cliente for maior que a data estimada de entrega.

```python
data_entrega_cliente > data_estimada_entrega
```

Caso contrário, a variável receberá o valor:

```python
atrasou_entrega = 0
```

Ou seja:

* `1` representa pedido entregue com atraso;
* `0` representa pedido entregue dentro do prazo ou antes da data estimada.

É importante destacar que a data real de entrega será utilizada apenas para construir a variável-alvo durante a etapa de preparação dos dados. Essa informação não poderá ser utilizada como variável explicativa do modelo, pois ela só é conhecida após a conclusão da entrega.

Usar a data real de entrega como entrada do modelo causaria vazamento de dados, tornando o desempenho artificialmente alto e inviabilizando o uso da solução em um cenário real de produção.

## Momento da previsão

A previsão deverá ser realizada no momento da confirmação do pedido, quando a compra já foi registrada no sistema e as principais informações comerciais e logísticas estão disponíveis.

Nesse momento, espera-se que a empresa já conheça informações como:

* localização do cliente;
* localização do vendedor ou centro de distribuição;
* categoria do produto;
* valor do pedido;
* valor do frete;
* tipo de pagamento;
* número de parcelas;
* quantidade de itens;
* data de compra;
* prazo estimado de entrega;
* modalidade de frete, quando disponível.

A previsão não poderá utilizar informações que só surgem depois da confirmação do pedido, como data real de entrega, avaliação do cliente, reclamações posteriores, status final da entrega ou qualquer outro dado registrado após o encerramento do processo logístico.

Essa definição é essencial para garantir que o modelo possa ser aplicado em um ambiente real, onde a empresa precisa tomar decisões antes que o atraso aconteça.

## Impacto esperado

A implantação de uma solução preditiva para risco de atraso pode gerar impacto positivo em diferentes áreas da NexaMarket.

Do ponto de vista operacional, a empresa poderá priorizar pedidos com maior risco e atuar antes que o atraso seja confirmado. Isso tende a melhorar a eficiência da equipe logística, reduzindo o esforço gasto com acompanhamento manual e permitindo melhor direcionamento dos recursos disponíveis.

Do ponto de vista do atendimento ao cliente, a solução pode reduzir o volume de contatos reativos, reclamações e solicitações de suporte relacionadas a atrasos. Ao identificar pedidos críticos antecipadamente, a empresa poderá melhorar a comunicação com o consumidor e reduzir a percepção negativa causada pela falta de informação.

Do ponto de vista estratégico, o modelo poderá revelar padrões importantes sobre atrasos por região, vendedor, categoria de produto, prazo prometido, valor de frete e distância logística. Esses insights podem apoiar decisões sobre melhoria de processos, renegociação com parceiros, revisão de políticas de frete e ajustes nos prazos prometidos ao cliente.

Entre os principais impactos esperados estão:

* redução da taxa de entregas atrasadas;
* aumento da satisfação do cliente;
* redução de reclamações no atendimento;
* diminuição de cancelamentos e reembolsos relacionados a atrasos;
* melhoria da reputação da empresa;
* maior eficiência na priorização operacional;
* identificação de gargalos logísticos recorrentes;
* apoio à tomada de decisão baseada em dados.
