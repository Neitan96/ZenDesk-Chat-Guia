# Glossário

Termos usados ao longo dos Módulos 1-7, em ordem alfabética. Termos marcados **(legado)** não devem ser usados como referência para construir algo novo — existem aqui só para você reconhecer quando aparecerem em buscas, vídeos ou artigos antigos.

| Termo | Definição |
|---|---|
| **Action / Custom action** | Passo dentro de um dialogue ou procedure que faz uma chamada de API para um sistema externo (ex.: o webhook do n8n). Configurado no **Action Builder**. |
| **Action Builder** | Ferramenta do Zendesk onde se criam as connections e as custom actions usadas nos fluxos. |
| **Agent Builder** | Ferramenta no-code para criar um **agente customizado** de IA do zero. Em EAP (Early Access Program) — pode não estar disponível em toda conta. |
| **AI Agent** | Nome atual da linha de produto de chatbot/IA do Zendesk. Substituiu "Answer Bot" e o "bot builder" clássico. |
| **AI Agents – Essential / Advanced** **(legado desde mai/2026)** | Divisão de pacotes que existia até 11/05/2026. Foi removida — os recursos do antigo "Advanced" (integrações via API, raciocínio agentic) agora vêm inclusos em qualquer plano Suite/Support. |
| **Answer Bot** **(legado)** | Nome antigo do produto de chatbot do Zendesk, antes de virar "AI Agents". |
| **Bot Builder** **(legado, desligado em 10/12/2026)** | Ferramenta clássica de criação de bots. Sucessor: **Agent Builder**. |
| **BSAT** | Satisfação do cliente medida especificamente para interações com o bot — separada do CSAT geral do time humano. |
| **Connection** | Onde ficam guardadas de forma segura as credenciais (API key, Basic auth, Bearer token ou OAuth 2.0) usadas para autenticar uma custom action contra um sistema externo. |
| **Conversational flow / Conversation flow** | Termo genérico para o fluxo de conversa configurado no Dialogue Builder. |
| **CSAT** | Customer Satisfaction — satisfação geral do atendimento (não específica do bot; ver BSAT). |
| **Custom agent** | Um agente criado do zero pelo Agent Builder (em EAP), como alternativa ao "agente padrão" configurado em AI → AI agents. |
| **Deflection rate** | Percentual de interações que não geraram ticket para um humano — mesmo que o problema não tenha sido de fato resolvido. Ver Módulo 6: não confundir com taxa de resolução. |
| **Dialogue** | Fluxo de conversa guiado, desenhado visualmente (árvore de decisão) no **Dialogue Builder**. Usado quando a conversa precisa de passos e regras exatas. |
| **Dialogue Builder** | Editor visual em forma de fluxograma onde se monta um dialogue, bloco a bloco. |
| **EAP (Early Access Program)** | Programa de acesso antecipado da Zendesk — recurso ainda não disponível por padrão em todas as contas, pode exigir solicitação. |
| **Escalation block / bloco de escalonamento** | Bloco do dialogue que transfere a conversa para um atendente humano (handoff), com mensagem de transição e destino configuráveis. |
| **Fallback** | Cenário embutido no bloco de integração/API que dispara quando nenhum outro cenário configurado bate — inclusive em caso de erro técnico (timeout, falha de rede). Não pode ser editado/removido. |
| **Flow Builder** **(legado, desligado junto com o Bot Builder)** | Ferramenta visual antiga para montar "bot flows". Sucessor conceitual: **Dialogue Builder**. |
| **Generative procedure** | Alternativa ao dialogue fixo: o agente usa IA generativa para responder com base num procedimento descrito em linguagem natural + a base de conhecimento, adaptando-se à pergunta do cliente. |
| **Handoff** | Transferência da conversa do bot para um atendente humano, levando o contexto/histórico já coletado. |
| **Insights Dashboard** | Painel de métricas básico (vem em qualquer plano): volume de conversas, taxa de resolução, tendências de escalonamento. |
| **Integration Builder** **(ainda existe, mas não é mais o caminho recomendado)** | Ferramenta mais antiga para conectar a APIs externas — o Zendesk já indica **Action Builder / Custom Actions** como abordagem atual. |
| **Intent** | A forma como o sistema reconhece qual use case o cliente quer, a partir do texto/opção escolhida por ele. |
| **Messaging trigger** | Regra de automação (Admin Center → Objects and rules → Business rules → Triggers → Messaging triggers) que dispara uma ação com base em um evento da conversa — usada para escalonamento automático por condição (ex.: sentimento negativo, cliente VIP). Diferente de ticket triggers e chat triggers. |
| **Procedure map** | Visualização gerada automaticamente pelo Zendesk dos passos que o agente vai seguir ao executar uma generative procedure — deve ser revisada antes de publicar. |
| **Resolution rate (taxa de resolução)** | Percentual de conversas totalmente resolvidas pelo bot, sem escalonamento e sem o cliente voltar a contatar sobre o mesmo assunto em 24-48h. Métrica mais confiável que deflection rate. |
| **Scenario** | Condição configurada diretamente dentro do bloco de integração/API do dialogue, para separar o que fazer conforme a resposta recebida (além do Fallback padrão). |
| **Use case** | O "assunto" que o agente sabe resolver (ex.: "consultar status de pedido"). Um agente pode ter vários. |
| **Variável** | Container de dado da conversa (ex.: número do pedido, nome do cliente), capturado em um ponto do fluxo e reutilizável em mensagens/condições posteriores. |
| **Version history** | Painel com o histórico de versões salvas/publicadas de um dialogue, permitindo reverter para uma versão anterior. |
| **Webhook (n8n)** | Nó do n8n que gera uma URL única para receber chamadas HTTP de fora (no nosso caso, do Zendesk) — o ponto de entrada do workflow que consulta o ERP. |
