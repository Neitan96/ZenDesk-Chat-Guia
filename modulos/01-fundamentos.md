# Módulo 1 — Fundamentos da plataforma de IA do Zendesk

> Antes de abrir o Admin Center: este módulo é conceitual. O objetivo é você entender o vocabulário e o mapa da ferramenta antes de construir qualquer fluxo — evita perder tempo clicando sem saber o que cada peça faz.

## 1.1 O que é "AI Agents"

**AI Agents** é o nome atual da linha de produtos de chatbot/IA do Zendesk. Substituiu o que antes se chamava "Answer Bot" e o "bot builder" clássico (esse último sendo desligado em 10/12/2026 — ver `pesquisa/plataforma-zendesk-ai.md`).

Na prática, um AI Agent é o "atendente virtual" que:
1. Conversa com o cliente pelo canal de mensageria (site, WhatsApp, Instagram, etc.);
2. Tenta resolver a dúvida sozinho — seguindo um fluxo guiado (dialogue) e/ou gerando respostas com IA a partir da sua base de conhecimento;
3. Quando não consegue, transfere ("handoff") para um atendente humano, levando o histórico da conversa junto.

## 1.2 Fim da divisão Essential x Advanced (mudança recente, maio/2026)

Até 11/05/2026, existiam dois pacotes:

| | Essential (incluso) | Advanced (add-on pago) |
|---|---|---|
| Autoresposta e sugestão de artigos | ✅ | ✅ |
| Respostas geradas por IA sobre a base de conhecimento | ✅ | ✅ |
| Fluxos de conversa customizados (dialogues) | ❌ | ✅ |
| Raciocínio agentic / procedures multi-etapas | ❌ | ✅ |
| Integrações externas via API (ex.: consultar ERP) | ❌ | ✅ |
| Analytics avançado (Conversation Journey Explorer, Knowledge Gap Analysis) | ❌ | ✅ |

**Isso mudou.** Em 11/05/2026 a Zendesk **removeu essa divisão**: os recursos que antes exigiam o Advanced — incluindo integrações externas via API, que é exatamente o que você precisa para consultar o ERP — passaram a vir **inclusos em qualquer plano Suite ou Support**, sem add-on separado.

**Ação prática:** confirme no Admin Center (`AI` no menu lateral) se a sua conta já reflete esse novo empacotamento. Se ainda aparecer a divisão Essential/Advanced na tela, pode ser que a conta ainda não tenha recebido a migração — nesse caso, os recursos de integração deste treinamento podem estar bloqueados até a atualização chegar.

## 1.3 Canais suportados

O AI Agent roda dentro da mensageria do Zendesk e se estende ao mesmo fluxo, sem reconstruir nada, para:

- Web Widget (chat do site)
- WhatsApp
- Facebook Messenger
- Instagram Direct
- X (Twitter) DM
- WeChat, LINE
- SMS (via Twilio)
- SDKs mobile (iOS, Android, Unity)

**Ponto de atenção para SAC multi-marca:** nem todo recurso se comporta igual em todo canal. Botões, carrosséis e outros elementos visuais do dialogue podem não existir em canais mais limitados (ex.: SMS). Antes de publicar um fluxo novo num canal, é sempre bom checar se ele usa algum componente visual que aquele canal não suporta — senão o cliente recebe uma mensagem quebrada.

## 1.4 Conceitos-chave (vocabulário)

| Termo | O que é |
|---|---|
| **Use case** | O "assunto" que o agente sabe resolver (ex.: "consultar status de pedido"). Um agente pode ter vários use cases. |
| **Intent** | A forma como o sistema reconhece qual use case o cliente quer, a partir do que ele escreveu. |
| **Dialogue** | O fluxo guiado — uma árvore de decisão desenhada visualmente (pergunta → opções → próximo passo), montada no **Dialogue Builder**. |
| **Generative procedure** | Em vez de um fluxo fixo, o agente usa IA generativa para responder com base num "procedimento" descrito em linguagem natural + sua base de conhecimento. Mais flexível, porém menos previsível que um dialogue. |
| **Action / Custom action** | Um passo dentro do fluxo (dialogue ou procedure) que faz uma chamada de API para um sistema externo — é a peça que vai conectar no ERP via n8n (Módulo 3). |
| **Handoff** | A transferência da conversa do bot para um atendente humano, incluindo o contexto já coletado. |
| **Agent Builder** | A ferramenta onde se monta um agente customizado do zero (hoje em EAP — Early Access Program, pode não estar disponível em toda conta ainda). |

**Quando usar dialogue vs. generative procedure?**
- **Dialogue**: quando a resposta precisa ser previsível e controlada (ex.: fluxo de troca de produto, onde cada passo tem regra de negócio exata). É o que você vai usar mais no caso do ERP, porque a chamada de API precisa acontecer num ponto exato do fluxo.
- **Generative procedure**: quando a pergunta é mais aberta e a resposta pode variar (ex.: dúvidas gerais sobre política de troca), respondendo com base em artigos da central de ajuda.

Na prática, os dois podem coexistir no mesmo agente: procedures cobrindo perguntas abertas, dialogues cobrindo os fluxos que precisam de dados exatos (como consulta ao ERP).

## 1.5 Onde tudo isso vive no Admin Center

- **AI → AI agents → AI agents**: configura o agente "padrão" (dialogues + generative procedures rodando sobre o fluxo existente). É provavelmente onde você vai passar mais tempo no dia a dia.
- **AI → Agent builder → Custom agents**: cria um agente customizado do zero. Está em EAP — se não aparecer na sua conta, pode ser necessário solicitar acesso antecipado à Zendesk.
- Depois de escolher um agente específico, a aba **Settings** dele reúne as configurações daquele agente (nome, canais ativos, idioma, etc.).

## Checklist de saída do módulo

Antes de seguir para o Módulo 2 (construir o agente), confirme:

- [ ] Você já localizou o menu `AI` no Admin Center da sua conta
- [ ] Você conferiu se a conta já está no novo empacotamento (sem divisão Essential/Advanced) — se ainda tiver a divisão antiga, anote isso, pode limitar o Módulo 3
- [ ] Você sabe a diferença entre dialogue e generative procedure e já tem uma ideia de qual usar para "consultar status de pedido no ERP" (spoiler: dialogue, por causa da previsibilidade)
- [ ] Você identificou em quais canais o SAC das suas marcas realmente atende hoje (isso define quais canais testar primeiro)

## Próximo módulo

**Módulo 2 — Construindo o agente (Agent Builder + Dialogue Builder)**: colocar a mão na massa, criar o primeiro fluxo guiado simples (sem integração ainda) para pegar o jeito da ferramenta antes de partir para a complexidade de chamar o ERP.
