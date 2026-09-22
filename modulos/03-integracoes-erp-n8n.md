# Módulo 3 — Integrações externas: conectando no ERP via n8n

> Este é o módulo central do treinamento. A ideia é sair com um exemplo funcional: o cliente informa o número do pedido no dialogue (Módulo 2), o Zendesk chama um webhook do n8n, o n8n consulta o ERP e devolve o status, e o bot mostra a resposta.

## 3.1 Arquitetura geral

```
Cliente digita nº do pedido
        │
        ▼
  Dialogue (Zendesk) — bloco de ação/API
        │  chama
        ▼
  Webhook (n8n)
        │  consulta
        ▼
  HTTP Request (n8n → API do ERP)
        │  formata a resposta
        ▼
  n8n responde ao Zendesk em JSON
        │
        ▼
  Dialogue exibe o resultado ao cliente
```

**Por que passar pelo n8n em vez do dialogue chamar o ERP direto?** Porque o Zendesk exige que a resposta da chamada venha em JSON num formato específico, e nem todo ERP responde assim, ou pode exigir autenticação mais complexa, combinar mais de uma chamada, ou precisar de alguma lógica de formatação. O n8n vira a "cola" entre os dois mundos: recebe do Zendesk num formato, fala com o ERP no formato que o ERP entende, e devolve pro Zendesk no formato que ele espera.

## 3.2 Autenticação: como o Zendesk se conecta em sistemas externos

Antes de criar a ação em si, você cria uma **connection** (conexão) no Action Builder, que guarda as credenciais de forma segura e é reutilizável em várias ações. Os tipos suportados:

- **API key**
- **Basic auth**
- **Bearer token**
- **OAuth 2.0**

Para o webhook do n8n, o mais simples costuma ser proteger a URL com um **Bearer token** ou uma **API key** enviada num header — configure isso tanto na connection do Zendesk quanto na validação do primeiro passo do workflow no n8n (rejeitar a chamada se o header não bater).

> Não exponha o webhook do n8n sem nenhuma autenticação. Qualquer pessoa que descobrir a URL conseguiria chamá-la e, através dela, acionar sua consulta ao ERP.

## 3.3 Configurando a ação de API no Zendesk (Action Builder / Custom Actions)

1. No Admin Center, crie a **connection** (3.2) apontando para a URL base do seu ambiente n8n, com o método de autenticação escolhido.
2. Crie a **custom action**:
   - Selecione a connection criada.
   - Defina os **inputs** que a ação recebe (ex.: `numero_pedido`) — esses inputs vêm das variáveis capturadas no dialogue (Módulo 2, a variável `numero_pedido` que você já tinha criado).
   - Monte o **corpo da requisição (body)**: use o ícone `{+}` para inserir os inputs como placeholders dentro do JSON que será enviado ao n8n.
   - **Importante:** a resposta que o n8n devolver precisa ser **JSON**, com o header `Content-Type: application/json`. Sem isso, o Zendesk não consegue interpretar o retorno.
3. Volte ao **dialogue** (Módulo 2) e, no ponto onde estava a mensagem fixa "essa função ainda está em construção", insira um bloco de **ação/API** apontando para essa custom action recém-criada.
4. Faça o **mapeamento da resposta**: os campos que vierem no JSON de retorno do n8n (ex.: `status_pedido`, `previsao_entrega`) são mapeados para novas variáveis do dialogue, que você usa na mensagem seguinte (ex.: "Seu pedido está: {{status_pedido}}, previsão de entrega: {{previsao_entrega}}").
5. **Ambientes:** o node de API no dialogue permite alternar entre ambiente de sandbox/teste e produção sem reconstruir o fluxo — vale montar o workflow do n8n primeiro num ambiente de teste antes de apontar para o ERP de produção.

## 3.4 Montando o lado n8n

### Passo 1 — nó Webhook (recebe a chamada do Zendesk)

- Crie um novo workflow no n8n, adicione um nó **Webhook** como trigger.
- Configure o método (normalmente `POST`) e copie a URL gerada — essa é a URL que vai na connection do Zendesk (3.2/3.3).
- No próprio nó (ou logo depois, com um nó de condição), valide o header de autenticação combinado com o Zendesk antes de prosseguir — se não bater, retorne um erro e encerre o workflow ali.

### Passo 2 — nó HTTP Request (consulta o ERP)

- Adicione um nó **HTTP Request** logo após o Webhook.
- Configure a URL/endpoint do ERP, método, headers e autenticação necessários para consultar o pedido (usando o `numero_pedido` recebido do Zendesk no corpo da requisição do webhook).
- Se o ERP exigir mais de uma chamada (ex.: uma para autenticar e outra para buscar o pedido), encadeie mais nós HTTP Request — o n8n foi feito exatamente para isso.

### Passo 3 — formatando a resposta

- Use um nó de transformação (**Set** / **Edit Fields**, ou um nó de **Code** se a lógica for mais complexa) para pegar o retorno bruto do ERP e montar o JSON exatamente no formato que o Zendesk espera (os nomes de campo que você vai mapear no passo 3.3.4 — ex.: `status_pedido`, `previsao_entrega`).
- O último nó do workflow deve ser a resposta ao webhook (**Respond to Webhook**), devolvendo esse JSON formatado com status 200 e `Content-Type: application/json`.

### Exemplo de JSON de resposta esperado pelo Zendesk

```json
{
  "status_pedido": "Em transporte",
  "previsao_entrega": "25/09/2026",
  "encontrado": true
}
```

O campo `encontrado` é proposital: no Módulo 4 você vai usar um bloco condicional no dialogue para checar esse campo e decidir entre mostrar o status ou dizer "não encontrei esse pedido, pode conferir o número?".

## 3.5 Exemplo prático guiado (resumo de ponta a ponta)

1. Cliente escolhe "Rastrear pedido" no dialogue (Módulo 2) e informa o número → variável `numero_pedido`.
2. Dialogue chama a custom action, enviando `numero_pedido` no body.
3. n8n recebe no Webhook, valida autenticação, chama a API do ERP com esse número.
4. n8n formata a resposta no JSON esperado e responde ao webhook.
5. Zendesk mapeia `status_pedido` e `previsao_entrega` para variáveis do dialogue.
6. Bot responde: "Seu pedido está: Em transporte, previsão de entrega: 25/09/2026."

## 3.6 O que ainda falta (fica para o Módulo 4)

Este módulo monta o **caminho feliz** (pedido encontrado, ERP no ar, resposta rápida). Ainda não tratamos:
- O que fazer quando `encontrado` vier `false`.
- O que fazer se o n8n/ERP demorar demais ou cair (timeout).
- Regras de quando isso deve escalar para um atendente humano em vez de travar o cliente num loop.

Isso é assunto do **Módulo 4 — Lógica de conversa e regras de negócio**, que usa bloco condicional exatamente sobre essa resposta da API.

## Checklist de saída do módulo

- [ ] Você criou uma connection no Zendesk com autenticação apontando para o n8n
- [ ] Você criou uma custom action com input (`numero_pedido`) e body configurado
- [ ] Você montou o workflow no n8n: Webhook → HTTP Request (ERP) → formatação → Respond to Webhook
- [ ] Você testou a chamada de ponta a ponta num ambiente de sandbox/teste antes de qualquer ideia de ir para produção
- [ ] Você mapeou a resposta JSON para variáveis do dialogue e usou elas na mensagem final ao cliente

## Próximo módulo

**Módulo 4 — Lógica de conversa e regras de negócio**: tratar o campo `encontrado: false`, timeouts e erros do ERP, e definir quando o bot deve parar de tentar sozinho e chamar um humano.
