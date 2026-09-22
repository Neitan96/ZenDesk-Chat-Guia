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
- **Correção importante (atualiza o que essa pesquisa dizia antes):** até maio/2026 existiam dois níveis — **AI Agents – Essential** (básico, incluso) e **AI Agents – Advanced** (pago, liberava dialogue builder, generative procedures, integrações via API). **Em 11/05/2026 a Zendesk removeu essa divisão**: os recursos que antes exigiam o add-on Advanced (raciocínio agentic, procedures multi-etapas, integrações externas via API) passaram a vir **inclusos em todos os planos Suite e Support**, sem add-on separado. Ou seja: o requisito de "add-on AI Agents Advanced" citado na primeira versão desta pesquisa está desatualizado — o que ainda vale confirmar é apenas se a conta está num plano Suite/Support e se já recebeu esse novo empacotamento.
- **Agent Builder**: ferramenta no-code para construir, testar e publicar agentes de IA customizados em cima das políticas e fluxos de trabalho da empresa. **Atenção:** a documentação oficial trata "custom agents" no Agent Builder como **EAP (Early Access Program)** — ou seja, pode não estar disponível por padrão em todas as contas ainda; pode ser necessário solicitar acesso antecipado.
- **Dialogue Builder**: editor visual (árvore de decisão) para desenhar conversas guiadas — é o sucessor conceitual do Flow Builder.
- **Generative procedures**: permitem que o agente gere respostas usando IA generativa em cima da base de conhecimento/procedimentos, em vez de só seguir um fluxo fixo.
- Requisito de plano (atualizado): **Zendesk Suite ou Support**, qualquer tier — não é mais necessário add-on separado para as funções avançadas, segundo o novo empacotamento pós-maio/2026. Vale confirmar no Admin Center se a conta já foi migrada para esse modelo.

Fontes:
- [About AI agents – Zendesk help](https://support.zendesk.com/hc/en-us/articles/6970583409690-About-AI-agents)
- [AI agents general info – Zendesk help](https://support.zendesk.com/hc/en-us/sections/4405298908570-AI-agents-general-info)
- [AI Agents | Zendesk Developer Docs](https://developer.zendesk.com/documentation/ai-agents/)
- [Zendesk AI Agents Explained: Features, Pricing & 2026 Updates](https://help-desk-migration.com/zendesk-ai-agents/)
- [Zendesk AI agent advanced vs essential: a 2026 comparison | eesel AI](https://www.eesel.ai/blog/zendesk-ai-agent-advanced-vs-essential)
- [Announcing expanded access to AI agent capabilities for all Zendesk customers – Zendesk help](https://support.zendesk.com/hc/en-us/articles/10487730059034-Announcing-expanded-access-to-AI-agent-capabilities-for-all-Zendesk-customers)
- [Creating and using custom agents in Zendesk (EAP) – Zendesk help](https://support.zendesk.com/hc/en-us/articles/10724438136858-Creating-and-using-custom-agents-in-Zendesk-EAP)
- [Understanding custom agents and turning on the agent builder (EAP) – Zendesk help](https://support.zendesk.com/hc/en-us/articles/10724212443802-Understanding-custom-agents-and-turning-on-the-agent-builder-EAP)

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

## Canais suportados

O AI Agent roda dentro da mensageria do Zendesk (Web Widget / Messaging) e se estende, sem reconstruir o fluxo, para:

- Web Widget (site)
- WhatsApp
- Facebook Messenger
- Instagram Direct
- X (Twitter) DM
- WeChat, LINE
- SMS (via Twilio)
- SDKs mobile (iOS, Android, Unity)

**Atenção:** o comportamento do agente pode variar por canal (nem todo recurso funciona igual em todo canal social — ex.: botões/carrosséis podem não existir no SMS). Vale revisar o artigo oficial "Differences in AI agent functionality on social messaging channels" antes de publicar num canal novo.

Fontes:
- [Zendesk messaging: Complete guide to setup, features, and pricing in 2026 | eesel AI](https://www.eesel.ai/blog/zendesk-messaging)
- [Differences in AI agent functionality on social messaging channels – Zendesk help](https://support.zendesk.com/hc/en-us/articles/4408822333722-Differences-in-AI-agent-functionality-on-social-messaging-channels)
- [How can I add the AI agent to WhatsApp or another social messaging channel? – Zendesk help](https://support.zendesk.com/hc/en-us/articles/9755141536410-How-can-I-add-the-AI-agent-to-WhatsApp-or-another-social-messaging-channel)

## Onde fica no Admin Center

- **AI > AI agents > AI agents**: configurações dos agentes de IA "padrão" (o modelo baseado em dialogues/generative procedures rodando sobre um fluxo existente).
- **AI > Agent builder > Custom agents**: onde se cria um agente customizado do zero (recurso em EAP — pode exigir ativação/solicitação de acesso antecipado).
- Depois de selecionar um agente, a aba **Settings** dele concentra as opções específicas daquele agente.

Fonte:
- [Migrating to the new AI agents experience – Zendesk help](https://support.zendesk.com/hc/en-us/articles/10543162665242-Migrating-to-the-new-AI-agents-experience)
- [Understanding custom agents and turning on the agent builder (EAP) – Zendesk help](https://support.zendesk.com/hc/en-us/articles/10724212443802-Understanding-custom-agents-and-turning-on-the-agent-builder-EAP)

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

1. Sua conta já migrou para a nova experiência de AI Agents, ou ainda está no legado? (Admin Center → AI → AI agents)
2. A conta já está no novo empacotamento (pós-11/05/2026, sem Essential/Advanced separados)? Se ainda aparecer a divisão antiga, isso muda o que está disponível para o módulo de integrações.
3. O **Agent Builder / Custom agents** (EAP) já está habilitado na conta, ou seria preciso solicitar acesso antecipado à Zendesk?
4. Quantas marcas/instâncias de SAC existem, e cada uma tem um Zendesk separado ou é multi-brand numa conta só? Isso muda se o bot é construído uma vez e reaproveitado ou replicado por marca.
5. O ERP já expõe uma API própria, ou o acesso a dados hoje é só via banco de dados/relatórios? Isso define se o n8n conversa direto com uma API ou precisa de um passo a mais.
