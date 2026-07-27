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

## Possíveis ações após a previsão

Após a geração da previsão de risco de atraso, a NexaMarket poderá utilizar o resultado do modelo para apoiar decisões operacionais e estratégicas.

A previsão deverá classificar cada pedido em níveis de risco, por exemplo:

* baixo risco de atraso;
* médio risco de atraso;
* alto risco de atraso.

Para pedidos classificados como **baixo risco**, o fluxo logístico poderá seguir normalmente, sem necessidade de intervenção adicional. Esses pedidos seriam apenas monitorados pelos processos padrões da empresa.

Para pedidos classificados como **médio risco**, a equipe de operações poderá realizar um acompanhamento preventivo, verificando se existem sinais de lentidão na separação, expedição ou transporte. Esses pedidos podem entrar em uma fila de observação intermediária.

Para pedidos classificados como **alto risco**, a empresa poderá adotar ações prioritárias, como:

* monitorar o pedido com maior frequência;
* acionar o vendedor ou centro de distribuição responsável;
* verificar possíveis restrições logísticas na região de destino;
* priorizar a separação e expedição do pedido;
* consultar a transportadora sobre riscos no trajeto;
* comunicar preventivamente o cliente em caso de risco elevado;
* oferecer alternativas de atendimento, quando aplicável;
* revisar a promessa de prazo para casos semelhantes no futuro.

Além da atuação individual sobre pedidos, a previsão também poderá gerar análises agregadas para apoiar decisões gerenciais. A empresa poderá acompanhar, por exemplo, quais estados, cidades, categorias, vendedores ou faixas de frete apresentam maior concentração de pedidos com alto risco de atraso.

Essas informações podem ser utilizadas para melhorar contratos com transportadoras, revisar políticas de frete, ajustar prazos estimados de entrega, identificar vendedores com baixo desempenho logístico e reduzir gargalos operacionais.

## Principais riscos do projeto

Embora o projeto tenha grande potencial de gerar valor para a NexaMarket, existem alguns riscos que precisam ser considerados durante seu desenvolvimento.

O primeiro risco é o **vazamento de dados**. Como o objetivo é prever o atraso no momento da confirmação do pedido, o modelo não poderá utilizar informações que só ficam disponíveis depois da entrega. Variáveis como data real de entrega, avaliação do cliente, status final do pedido ou reclamações posteriores não devem ser usadas como atributos de entrada.

Outro risco importante é a **qualidade dos dados**. Bases históricas de pedidos podem conter valores ausentes, inconsistências em datas, registros duplicados, categorias pouco padronizadas e informações incompletas sobre clientes, vendedores ou fretes. Esses problemas podem afetar diretamente o desempenho e a confiabilidade do modelo.

Também existe o risco de **desbalanceamento da variável-alvo**. Caso a quantidade de pedidos atrasados seja muito menor do que a quantidade de pedidos entregues no prazo, o modelo poderá aprender a favorecer a classe majoritária, apresentando boa acurácia geral, mas baixa capacidade de identificar atrasos reais.

Outro ponto relevante é a **mudança de comportamento ao longo do tempo**. Atrasos logísticos podem variar conforme sazonalidade, campanhas promocionais, períodos como Black Friday, mudanças em transportadoras, aumento de demanda, alterações operacionais ou eventos externos. Isso pode fazer com que o desempenho do modelo diminua com o passar do tempo.

Há também o risco de **interpretação incorreta da previsão**. O modelo não deve ser tratado como uma verdade absoluta, mas como uma ferramenta de apoio à decisão. Um pedido classificado como alto risco não necessariamente irá atrasar, e um pedido de baixo risco ainda poderá apresentar problema.

Por fim, existe o risco de **baixa adoção pela área de negócio**. Caso a solução não seja simples, interpretável e conectada a ações práticas, as equipes de logística e atendimento podem não incorporar o modelo em sua rotina.

## Restrições do projeto

Este projeto utilizará dados históricos disponíveis em bases públicas de e-commerce. Por esse motivo, algumas informações que poderiam existir em uma empresa real talvez não estejam disponíveis, como nome da transportadora, modalidade detalhada de frete, status intermediários do transporte ou informações em tempo real sobre rastreamento.

A previsão será construída com base nas informações disponíveis até o momento da confirmação do pedido. Portanto, qualquer variável registrada após esse momento será descartada da etapa de modelagem, mesmo que apresente forte relação com o atraso.

O projeto também deverá respeitar boas práticas de Ciência de Dados, como separação adequada entre treino e teste, prevenção de vazamento de dados, avaliação por métricas compatíveis com o problema e documentação clara das decisões tomadas.

Como se trata de um projeto de portfólio, a implantação será demonstrada por meio de uma aplicação simples, possivelmente em Streamlit, sem integração real com sistemas corporativos de pedidos, transportadoras ou atendimento ao cliente.

## Critérios de sucesso

O sucesso do projeto será avaliado tanto por critérios técnicos quanto por critérios de negócio.

Do ponto de vista técnico, espera-se que o modelo consiga identificar pedidos com maior risco de atraso com desempenho superior a uma abordagem aleatória ou baseada apenas em regras simples. Métricas como recall, precision, F1-score, ROC-AUC e matriz de confusão serão utilizadas para avaliar os resultados.

Como o objetivo principal é antecipar atrasos, o recall da classe de atraso será uma métrica especialmente importante. Isso porque a empresa deseja identificar a maior quantidade possível de pedidos que realmente poderão atrasar.

No entanto, a precision também deverá ser observada, pois um número muito alto de falsos positivos pode gerar esforço operacional desnecessário. O modelo ideal deverá equilibrar a capacidade de identificar atrasos com a geração de alertas úteis para o negócio.

Do ponto de vista de negócio, o projeto será considerado bem-sucedido se permitir:

* identificar pedidos com maior probabilidade de atraso;
* apoiar priorização operacional;
* gerar insights sobre fatores associados a atrasos;
* melhorar a tomada de decisão da área logística;
* demonstrar potencial de redução de reclamações e custos de atendimento;
* apresentar uma solução compreensível para públicos técnicos e não técnicos.

Do ponto de vista de portfólio, o projeto será considerado bem-sucedido se apresentar uma narrativa clara, código organizado, análise exploratória bem documentada, modelo comparado com alternativas, explicabilidade dos resultados e uma demonstração simples da solução em funcionamento.

## Considerações finais da Fase 1

A Fase 1 definiu o problema de negócio da NexaMarket, o objetivo da solução, os usuários envolvidos, a variável-alvo, o momento correto da previsão, os impactos esperados, as possíveis ações operacionais, os riscos, as restrições e os critérios de sucesso do projeto.

Com essa etapa concluída, o projeto passa a ter uma base de negócio clara para orientar as próximas fases. A partir daqui, as decisões técnicas de coleta, preparação, análise, modelagem e implantação deverão estar sempre conectadas ao problema central: antecipar pedidos com maior risco de atraso e apoiar uma atuação mais proativa da empresa.
