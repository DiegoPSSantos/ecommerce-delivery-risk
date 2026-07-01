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