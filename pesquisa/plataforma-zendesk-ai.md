# Pesquisa: plataforma de chatbots/IA do Zendesk (setembro/2026)

> Pesquisa feita via busca na web em 22/09/2026. Zendesk muda nomenclatura e empacotamento de planos com frequência — antes de seguir qualquer módulo do treinamento, revalide os pontos marcados como "confirmar na sua conta".

## ⚠️ Alerta crítico: Bot Builder / Flow Builder legado está sendo descontinuado

- Desde **02/02/2025**, o Bot Builder clássico (o que cria "bot flows" com Flow Builder, Answers e Intents) é considerado **legado**.
- Em **31/08/2026** o Zendesk parou desenvolvimento técnico nele (só bugs críticos).
- Em **10/12/2026** ele deixa de funcionar completamente.
- Entre 11/05/2026 e 12/06/2026, contas foram migradas automaticamente do pacote "AI Agents – Essential + legacy bot builder/answers/intents" para a **nova experiência de AI Agents**.

**Implicação prática para este treinamento:** não vale a pena estudar/documentar o Bot Builder + Flow Builder antigo, exceto se sua conta especificamente ainda não migrou (confirme no admin center). O treinamento abaixo é montado em cima da **ferramenta nova**: Agent Builder, Dialogue Builder e Action Builder.

Fontes:
- [About AI agents – Zendesk help](https://support.zendesk.com/hc/en-us/articles/6970583409690-About-AI-agents)
- [Migrating to the new AI agents experience – Zendesk help](https://support.zendesk.com/hc/en-us/articles/10543162665242-Migrating-to-the-new-AI-agents-experience)
- [Announcing expanded access to AI agent capabilities for all Zendesk customers – Zendesk help](https://support.zendesk.com/hc/en-us/articles/10487730059034-Announcing-expanded-access-to-AI-agent-capabilities-for-all-Zendesk-customers)
- [About bot builder (Legacy) – Zendesk help](https://support.zendesk.com/hc/en-us/articles/4408838909210-About-bot-builder-Legacy)

## Visão geral da ferramenta atual (2026)

- **AI Agents** é o nome atual da linha de produtos de chatbot/IA do Zendesk (substituiu "Answer Bot" e o "bot builder" antigo).
- Existem dois níveis de pacote: **AI Agents – Essential** (mais básico) e **AI Agents – Advanced** (libera dialogue builder, use cases, generative procedures, automações mais ricas e ações/integrações customizadas).
- **Agent Builder**: ferramenta no-code para construir, testar e publicar agentes de IA customizados em cima das políticas e fluxos de trabalho da empresa.
- **Dialogue Builder**: editor visual (árvore de decisão) para desenhar conversas guiadas — é o sucessor conceitual do Flow Builder.
- **Generative procedures**: permitem que o agente gere respostas usando IA generativa em cima da base de conhecimento/procedimentos, em vez de só seguir um fluxo fixo.
- Requisito de plano: **Zendesk Suite Professional ou Enterprise** + add-on de AI Agents (Essential vem incluso em alguns planos; recursos avançados de integração exigem Advanced).

Fontes:
- [About AI agents – Zendesk help](https://support.zendesk.com/hc/en-us/articles/6970583409690-About-AI-agents)
- [AI agents general info – Zendesk help](https://support.zendesk.com/hc/en-us/sections/4405298908570-AI-agents-general-info)
- [AI Agents | Zendesk Developer Docs](https://developer.zendesk.com/documentation/ai-agents/)
- [Zendesk AI Agents Explained: Features, Pricing & 2026 Updates](https://help-desk-migration.com/zendesk-ai-agents/)

## Integrações externas (o que interessa para consultar o ERP)

Este é o ponto mais relevante para o seu caso de uso (consultar ERP durante a conversa):

- **Action Builder / Custom Actions**: abordagem **atual e recomendada** pelo Zendesk para integrar o bot com qualquer API externa. Permite configurar um passo de "chamada de API" dentro do fluxo, passando parâmetros da conversa e recebendo dados de volta (ex.: status de pedido, dados cadastrais) para usar na resposta ao cliente.
- **Integration Builder**: ferramenta mais antiga com o mesmo objetivo (conectar a qualquer API/fonte de dados sem código). Ainda existe, mas o Zendesk já sinaliza Custom Actions como o caminho recomendado para novas integrações — não vale a pena investir tempo profundo nela.
- Ações pré-construídas existem para alguns sistemas conhecidos (Google Sheets, Jira, Salesforce, Slack) — para o **ERP interno**, será sempre integração customizada (via Action Builder apontando para uma API própria ou para um webhook do n8n).
- Autenticação: as ações customizadas suportam API key/OAuth para autenticar contra o sistema externo.

Fontes:
- [About the integration builder for AI agents – Zendesk help](https://support.zendesk.com/hc/en-us/articles/8357756844442-About-the-integration-builder-for-AI-agents)
- [About actions in AI agents – Zendesk help](https://support.zendesk.com/hc/en-us/articles/10783053154202-About-actions-in-AI-agents)
- [Creating a custom CRM integration for an advanced AI agent – Zendesk help](https://support.zendesk.com/hc/en-us/articles/8357758272154-Creating-a-custom-CRM-integration-for-an-advanced-AI-agent)
- [Announcing the integration of the action builder with AI agents – Zendesk help](https://support.zendesk.com/hc/en-us/articles/10736071224858-Announcing-the-integration-of-the-action-builder-with-AI-agents)

## Onde entra o n8n

O Zendesk não tem conector nativo para ERPs específicos nem para n8n — a integração é sempre via **API/webhook genérico**, então o n8n funciona bem como camada intermediária:

- **n8n como receptor**: você cria um workflow no n8n com um nó **Webhook**, que gera uma URL única. Essa URL é configurada como o endpoint da "chamada de API" no Action Builder do Zendesk.
- **n8n consultando o ERP**: dentro do workflow, o nó **HTTP Request** faz a chamada real para a API do ERP (ou para qualquer sistema que não tenha nó nativo no n8n), trata/formata a resposta, e devolve um JSON simples para o Zendesk responder ao cliente.
- Essa arquitetura (Zendesk → webhook n8n → ERP → resposta formatada → Zendesk) é vantajosa porque:
  - Isola a complexidade/autenticação do ERP fora do Zendesk.
  - Permite tratar erros, formatar dados e até combinar múltiplas fontes antes de responder, sem depender só do que o Action Builder consegue fazer nativamente.

Fontes:
- [A practical guide to Zendesk integrations with n8n | eesel AI](https://www.eesel.ai/blog/zendesk-integrations-with-n8n)
- [How to build Zendesk n8n integration workflows: A complete guide | eesel AI](https://www.eesel.ai/blog/zendesk-n8n-integration-workflow)
- [Webhook and Zendesk: Automate Workflows with n8n](https://n8n.io/integrations/webhook/and/zendesk/)

## Termos que ainda vão aparecer em buscas (mas são legados — não confundir)

| Termo legado | Status | Equivalente atual |
|---|---|---|
| Bot Builder | Descontinuado em 10/12/2026 | Agent Builder |
| Flow Builder (bot flows) | Descontinuado junto com o Bot Builder | Dialogue Builder |
| Answer Bot / Answers | Renomeado/absorvido | AI Agents |
| Intents (modelo antigo) | Absorvido na nova experiência | Use cases / intents (novo modelo, dentro do Agent Builder) |
| Integration Builder | Ainda existe, mas não é mais o caminho recomendado | Action Builder / Custom Actions |

## Perguntas em aberto para confirmar na sua conta antes de aprofundar o treinamento

1. Sua conta já migrou para a nova experiência de AI Agents, ou ainda está no legado? (Admin Center → AI agents)
2. Qual o plano contratado hoje (Suite Professional/Enterprise) e existe o add-on **AI Agents Advanced**? Sem ele, os módulos de integração com API não se aplicam.
3. Quantas marcas/instâncias de SAC existem, e cada uma tem um Zendesk separado ou é multi-brand numa conta só? Isso muda se o bot é construído uma vez e reaproveitado ou replicado por marca.
4. O ERP já expõe uma API própria, ou o acesso a dados hoje é só via banco de dados/relatórios? Isso define se o n8n conversa direto com uma API ou precisa de um passo a mais.
