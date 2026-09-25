[README.md](https://github.com/user-attachments/files/32658381/README.md)
# Automação com n8n e Inteligência Artificial Generativa

## Sobre o projeto

Este projeto foi desenvolvido como parte de um desafio da DIO sobre Automação com n8n e Inteligência Artificial Generativa. O objetivo foi usar a IA como ferramenta de aprendizagem ativa: reunir fontes confiáveis, formular prompts no NotebookLM, avaliar as respostas e registrar tanto os aprendizados quanto as limitações encontradas.

O tema escolhido foi a automação de processos com o n8n, com foco em workflows, nodes, Webhooks, APIs REST, JSON, expressões e lógica condicional.

---

## Objetivos de estudo

- Compreender os conceitos fundamentais do n8n;
- Entender a estrutura de workflows, nodes e triggers;
- Estudar o uso de APIs REST e Webhooks;
- Trabalhar com dados estruturados em JSON;
- Aprender a usar expressões para acessar dados entre nodes;
- Aplicar condições com o node **IF**;
- Conhecer possibilidades de integração com IA generativa;
- Desenvolver uma rotina básica de troubleshooting;
- Usar respostas de IA de forma crítica, validando-as no ambiente real.

---

## Curadoria de fontes

As fontes abaixo foram reunidas no NotebookLM para apoiar os estudos.

1. **Documentação oficial do n8n**  
   https://docs.n8n.io/  
   Fonte principal para estudar workflows, nodes, integrações e o funcionamento geral da plataforma.

2. **Webhook node**  
   https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.webhook/  
   Fonte usada para compreender como um sistema externo inicia um workflow por meio de uma requisição HTTP.

3. **HTTP Request node**  
   https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.httprequest/  
   Fonte usada para estudar o consumo de APIs REST e a comunicação com serviços externos.

4. **Expressões no n8n**  
   https://docs.n8n.io/code/expressions/  
   Fonte usada para estudar o acesso e a manipulação de dados recebidos de nodes anteriores.

5. **Inteligência Artificial no n8n**  
   https://docs.n8n.io/advanced-ai/  
   Fonte usada para conhecer recursos e integrações de IA na plataforma.

---

## Engenharia de prompts

### Prompt 1 — Conceitos básicos do n8n

**Prompt testado no NotebookLM:**

> Explique os principais conceitos do n8n para alguém que está começando em automação. Organize a resposta em: 1. O que é n8n; 2. O que é workflow; 3. O que são nodes; 4. O que são triggers; 5. Como APIs e Webhooks entram nesse processo; 6. Um exemplo prático simples.

### Resultado obtido

A resposta definiu o n8n como uma ferramenta de automação de workflows e explicou que um workflow é formado por uma sequência de etapas visuais. Também descreveu nodes como blocos que executam tarefas, triggers como eventos que iniciam a automação e o uso de APIs e Webhooks para integrar sistemas.

O exemplo apresentado foi o cadastro de um cliente: um formulário envia dados para um Webhook, o n8n trata as informações e um HTTP Request envia um cadastro para a API de um CRM.

**Fontes relacionadas no NotebookLM:**
- Documentação oficial do n8n
- Webhook node
- HTTP Request node

### Pontos positivos

- Organização clara para quem está começando;
- Conceitos apresentados em sequência lógica;
- Boa relação entre Webhook, API e HTTP Request;
- Exemplo simples próximo de um cenário real.

### Limitações e melhoria aplicada

Apesar de útil para a teoria, a resposta não mostrou o JSON de entrada, expressões, configuração dos nodes, testes ou tratamento de erros. Essa limitação orientou a criação de um segundo prompt, mais específico e prático.

---

### Prompt 2 — Workflow prático com Webhook, IF e HTTP Request

**Prompt testado no NotebookLM:**

> Crie um exemplo completo de workflow no n8n utilizando Webhook, IF e HTTP Request. Mostre também um JSON de entrada, as expressões utilizadas para acessar os dados e explique o fluxo passo a passo.

### Resultado obtido

O NotebookLM propôs um cenário de e-commerce para processar pedidos prioritários. O fluxo é:

```text
Webhook → IF → HTTP Request
```

1. O **Webhook** recebe os dados de um novo pedido.
2. O **IF** verifica se o cliente é VIP ou se o pedido possui valor superior a 100.
3. Quando pelo menos uma condição é verdadeira, o **HTTP Request** envia uma notificação para uma API externa.

**Fontes relacionadas no NotebookLM:**
- Webhook node
- HTTP Request node
- Expressões no n8n
- Documentação oficial do n8n

### JSON de entrada do exemplo

```json
{
  "cliente": {
    "nome": "Ana Silva",
    "email": "ana.silva@exemplo.com",
    "categoria": "VIP"
  },
  "pedido": {
    "id": 1042,
    "valor_total": 250.00,
    "status": "pago"
  }
}
```

### Configuração do workflow

#### 1. Webhook

Configuração sugerida:

```text
HTTP Method: POST
Path: novo-pedido
Respond: Immediately
```

Esse node é o trigger do fluxo. Ao receber uma requisição `POST`, ele disponibiliza os dados do pedido para os nodes seguintes. No exemplo fornecido, os dados ficam dentro de `$json.body`.

#### 2. Expressões utilizadas

Para acessar a categoria do cliente:

```text
{{ $json.body.cliente.categoria }}
```

Para acessar o valor total do pedido:

```text
{{ $json.body.pedido.valor_total }}
```

Para acessar o ID do pedido:

```text
{{ $json.body.pedido.id }}
```

Para acessar o e-mail do cliente:

```text
{{ $json.body.cliente.email }}
```

#### 3. IF — condições de decisão

O node IF deve usar o combinador **OR** (OU), ou seja, basta uma condição ser verdadeira para o fluxo seguir pela saída **True**.

| Condição | Tipo | Valor 1 | Operação | Valor 2 |
| --- | --- | --- | --- | --- |
| Cliente prioritário | String | `{{ $json.body.cliente.categoria }}` | Equals | `VIP` |
| Pedido de alto valor | Number | `{{ $json.body.pedido.valor_total }}` | Greater Than | `100` |

Com o JSON de exemplo, as duas condições são verdadeiras: Ana é cliente VIP e o pedido tem valor total de 250.

#### 4. HTTP Request — envio para a API

O node HTTP Request fica conectado à saída **True** do IF. Configuração sugerida:

```text
Method: POST
URL: https://api.meusistema.com/v1/notificacoes
Send Body: ON
Body Content Type: JSON
Specify Body: Using JSON
```

Exemplo de body a ser configurado no node:

```json
{
  "mensagem": "Notificação prioritária para o pedido #{{ $json.body.pedido.id }}",
  "email_destinatario": "{{ $json.body.cliente.email }}",
  "valor": "{{ $json.body.pedido.valor_total }}"
}
```

Nesse body, as expressões são avaliadas pelo n8n durante a execução. A URL é apenas ilustrativa: para testar de verdade, ela precisa ser substituída por um endpoint real que aceite requisições `POST`.

### O que aprendi com o Prompt 2

O segundo prompt produziu uma resposta muito mais aplicável. Ele conectou o JSON recebido pelo Webhook às condições do IF e ao body do HTTP Request, mostrando como os dados percorrem um workflow.

Também reforçou que prompts mais específicos tendem a gerar exemplos mais úteis para prática.

---

## Cicatrizes e troubleshooting

As "cicatrizes" representam os problemas, dúvidas e validações necessários durante o aprendizado. Neste projeto, a principal lição foi: uma resposta de IA é um ponto de partida, não uma configuração pronta para copiar sem conferir.

### 1. O caminho do JSON pode mudar

As expressões do exemplo usam `$json.body`, pois foi assumido que o Webhook entregaria o payload dentro de `body`:

```text
{{ $json.body.cliente.categoria }}
```

Dependendo do node anterior e da versão/configuração do workflow, o dado pode estar diretamente no item, por exemplo:

```text
{{ $json.cliente.categoria }}
```

**Como resolver:** execute o node anterior, abra o painel de output e copie o caminho que corresponde à estrutura real dos dados.

### 2. Uma expressão pode estar no tipo errado

Uma comparação numérica deve usar um valor numérico. Se `valor_total` vier como texto, a condição `Greater Than 100` pode falhar ou gerar resultado inesperado.

**Como resolver:** confira o tipo exibido no output; se necessário, converta o valor antes de compará-lo.

### 3. A URL de exemplo não é uma API real

`https://api.meusistema.com/v1/notificacoes` é uma URL didática. Ela não deve ser usada como destino de produção.

**Como resolver:** use a documentação da API que será integrada e confirme URL, método HTTP, autenticação, headers e formato do body.

### 4. O fluxo pode ir para a saída False

Se o cliente não for VIP e o pedido não superar 100, o IF seguirá pela saída **False**. Sem um node conectado, esse ramo apenas termina.

**Como resolver:** conecte a saída False a um node de registro, resposta alternativa ou notificação não prioritária, caso esse comportamento seja necessário.

### Roteiro de troubleshooting

1. Execute o workflow ou o node isoladamente.
2. Confira os dados de entrada e o output de cada node.
3. Verifique se os caminhos usados nas expressões existem.
4. Confirme o tipo de cada dado, especialmente valores numéricos e textos.
5. No HTTP Request, confira URL, método, body, headers e credenciais.
6. Leia a mensagem de erro e teste novamente após uma alteração por vez.

---

## Miniguia de estudo

### n8n

Ferramenta visual de automação que permite conectar serviços, APIs e regras de negócio em workflows.

### Workflow

Sequência de etapas automatizadas. Cada etapa recebe, transforma ou envia informações.

### Node

Bloco de construção do workflow. Um node pode iniciar o fluxo, validar uma condição, transformar dados ou chamar um serviço externo.

### Trigger

Node que inicia um workflow a partir de um evento. O Webhook e o Schedule Trigger são exemplos.

### Webhook

URL disponibilizada pelo n8n para receber dados e eventos enviados por outro sistema.

### API REST

Forma de comunicação entre sistemas via HTTP, normalmente utilizando métodos como `GET`, `POST`, `PUT`, `PATCH` e `DELETE`.

### JSON

Formato textual estruturado usado para transportar dados entre aplicações.

### Expressão

Código dinâmico colocado em um campo do n8n para obter dados de um item, como `{{ $json.body.cliente.email }}`.

### IF

Node que avalia regras e divide o fluxo entre as saídas **True** e **False**.

### HTTP Request

Node usado para chamar uma API externa, enviar dados ou buscar informações.

---

## Glossário

| Termo | Definição |
| --- | --- |
| API | Interface que permite a comunicação entre sistemas. |
| Webhook | Mecanismo para receber eventos e dados enviados por outro sistema. |
| JSON | Formato de dados estruturados baseado em pares de chave e valor. |
| Workflow | Fluxo de automação composto por etapas conectadas. |
| Node | Bloco que realiza uma tarefa dentro do workflow. |
| Trigger | Evento ou node que inicia uma automação. |
| Prompt | Instrução enviada a uma IA para obter uma resposta. |
| Expressão | Forma de usar dados dinamicamente em um campo do n8n. |
| Troubleshooting | Processo de identificar, investigar e corrigir problemas. |

---

## Prompts reutilizáveis

Os prompts abaixo podem ser reaproveitados em estudos futuros no NotebookLM ou em outra IA generativa.

> Explique **[conceito]** de forma simples para iniciantes e depois apresente um exemplo prático no n8n.

> Crie um exercício de n8n usando os nodes **[nodes]**. Mostre o JSON de entrada, as expressões necessárias, a configuração de cada node e o resultado esperado.

> Analise este JSON e mostre quais expressões devo usar no n8n para acessar os campos **[campos desejados]**. Explique de onde vem cada caminho.

> Analise este erro do n8n: **[mensagem de erro]**. Apresente a causa provável, como verificar o problema e os passos para corrigir.

> Revise este workflow do n8n: **[descrição ou exportação]**. Identifique riscos em expressões, tipos de dados, autenticação e tratamento de falhas.

---

## Conclusão

Este projeto demonstrou como a IA generativa pode apoiar uma aprendizagem mais ativa. Com o NotebookLM, foi possível reunir fontes, testar prompts, avaliar a qualidade das respostas e transformar uma explicação teórica em um exemplo de automação mais prático.

O principal resultado não foi apenas conhecer conceitos do n8n, mas aprender a validar as informações antes de aplicá-las. A análise do JSON real, a conferência das expressões, o teste de cada node e a investigação de erros são etapas essenciais para criar automações confiáveis.

Ao final, foram consolidados conhecimentos sobre n8n, workflows, nodes, Webhooks, APIs REST, JSON, expressões, lógica condicional, HTTP Request e troubleshooting.
