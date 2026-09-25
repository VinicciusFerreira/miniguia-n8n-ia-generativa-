# Automação com n8n e Inteligência Artificial Generativa

## Sobre o projeto

Este projeto foi desenvolvido como parte de um desafio da DIO com o objetivo de utilizar Inteligência Artificial como ferramenta de aprendizagem ativa.

O tema escolhido foi Automação de Processos utilizando n8n e Inteligência Artificial Generativa.

## Objetivos de estudo

- Aprender os principais conceitos do n8n;
- Entender APIs REST;
- Estudar Webhooks;
- Trabalhar com JSON;
- Aprender integrações com Inteligência Artificial;
- Desenvolver técnicas de troubleshooting.

## Curadoria de fontes

## Engenharia de prompts

## Cicatrizes e troubleshooting

## Miniguia de estudo

## Curadoria de fontes

### 1. Documentação oficial do n8n
https://docs.n8n.io/

Fonte principal para estudar workflows, nodes, integrações e funcionamento da plataforma.

### 2. Webhook node
https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.webhook/

Utilizada para entender como sistemas externos podem iniciar workflows no n8n.

### 3. HTTP Request node
https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.httprequest/

Utilizada para estudar consumo de APIs REST e comunicação com serviços externos.

### 4. Expressões no n8n
https://docs.n8n.io/code/expressions/

Fonte utilizada para estudar acesso e manipulação de dados entre nodes.

### 5. Inteligência Artificial no n8n
https://docs.n8n.io/advanced-ai/

Utilizada para estudar integrações com modelos de IA e criação de fluxos inteligentes.

## Glossário

## Prompts reutilizáveis

## Conclusão

## Curadoria de fontes

## Engenharia de prompts

### Prompt 1 — Conceitos básicos do n8n

**Prompt utilizado:**

> Explique os principais conceitos do n8n para alguém que está começando em automação. Organize a resposta em:
> 1. O que é n8n
> 2. O que é workflow
> 3. O que são nodes
> 4. O que são triggers
> 5. Como APIs e Webhooks entram nesse processo
> 6. Um exemplo prático simples

### Resultado obtido

A resposta conseguiu explicar de forma organizada os principais conceitos do n8n, incluindo workflows, nodes, triggers, APIs e Webhooks.

O exemplo de cadastro de cliente ajudou a visualizar como esses elementos podem funcionar juntos em uma automação real.

### Pontos positivos

- Resposta bem estruturada;
- Conceitos apresentados em ordem lógica;
- Uso de exemplos práticos;
- Relação clara entre Webhook, HTTP Request e API;
- Linguagem adequada para iniciantes.

### Limitações encontradas

Apesar de explicar bem os conceitos, a resposta ficou mais teórica do que prática em alguns pontos.

Por exemplo, não mostrou:
- como os dados chegam em formato JSON;
- como acessar esses dados dentro do n8n;
- como utilizar expressões;
- como testar o workflow;
- como identificar erros durante a execução.

### Próxima melhoria do prompt

Para obter uma resposta mais prática, o próximo prompt será:

> Crie um exemplo completo de workflow no n8n utilizando Webhook, IF e HTTP Request. Mostre também um JSON de entrada, as expressões utilizadas para acessar os dados e explique o fluxo passo a passo.
