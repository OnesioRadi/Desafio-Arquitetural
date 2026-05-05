# ADR 001: Escolha da Tecnologia de Mensageria

## Status
Proposto

## Contexto
O sistema precisa garantir que o serviço de Lançamentos continue operando mesmo que o serviço Consolidado esteja indisponível. Além disso, precisamos suportar uma carga de 50 req/s.

## Decisão
Decidimos utilizar o **RabbitMQ** como Message Broker para o desacoplamento dos serviços.

## Justificativa
1. **Garantia de Entrega:** O RabbitMQ permite persistência de mensagens em disco, garantindo que nenhum lançamento financeiro seja perdido se o serviço Consolidado cair.
2. **Complexidade vs Necessidade:** Embora o Kafka seja excelente, o RabbitMQ é mais simples de configurar e gerenciar para o volume de 50 req/s, reduzindo o custo operacional (Overhead).
3. **Protocolo AMQP:** Oferece suporte nativo a confirmações (Acknowledgements), essencial para integridade financeira.

## Consequências
- Precisaremos gerenciar um cluster RabbitMQ (ou usar o Amazon MQ).
- Implementação de lógica de Retry (tentativas) no consumidor para casos de erro temporário.

