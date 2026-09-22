# Índice do treinamento

Legenda: ⬜ não iniciado · 🚧 em construção · ✅ pronto

Cada módulo vira um arquivo em `modulos/NN-nome.md` quando for escrito. Este índice é o roteiro — construímos módulo por módulo, na ordem, sem pular para integrações antes de fechar a base.

---

## Módulo 0 — Antes de começar ⬜
`modulos/00-antes-de-comecar.md`
- [ ] Confirmar se a conta já está na nova experiência de AI Agents ou ainda no Bot Builder legado
- [ ] Confirmar plano contratado e se o add-on AI Agents Advanced está ativo
- [ ] Mapear quantas marcas existem e como estão organizadas no Zendesk (contas separadas vs. multi-brand)
- [ ] Glossário rápido: termos legados x termos atuais (ver `pesquisa/plataforma-zendesk-ai.md`)

## Módulo 1 — Fundamentos da plataforma de IA do Zendesk ✅
`modulos/01-fundamentos.md`
- [x] O que é AI Agents e o fim da divisão Essential x Advanced (mudou em mai/2026)
- [x] Canais suportados (webchat, WhatsApp, Instagram, SMS, etc.) e diferenças por canal
- [x] Conceitos-chave: use cases, intents, dialogues, generative procedures, actions, handoff
- [x] Onde tudo isso vive no Admin Center (mapa de navegação)

## Módulo 2 — Construindo o agente (Dialogue Builder + Generative Procedures) ✅
`modulos/02-agent-builder.md`
- [x] Criando um agente de IA e navegando entre Dialogues e Procedures
- [x] Dialogue Builder: blocos principais e passo a passo de um fluxo guiado de exemplo
- [x] Generative procedures: como criar, boas práticas, mapa da procedure
- [x] Quando usar dialogue vs. generative procedure (e por que o fluxo de ERP é um dialogue)

## Módulo 3 — Integrações externas: conectando no ERP via n8n ✅
`modulos/03-integracoes-erp-n8n.md`
**Módulo central para o seu caso de uso.**
- [x] Action Builder / Custom Actions: connection, inputs, body e mapeamento de resposta
- [x] Autenticação (API key / Basic auth / Bearer token / OAuth 2.0) para a connection
- [x] Montando o lado n8n: nó Webhook (recebe do Zendesk) + nó HTTP Request (consulta o ERP) + formatação + Respond to Webhook
- [x] Formato JSON esperado pelo Zendesk na resposta
- [x] Exemplo prático guiado de ponta a ponta: consultar status de pedido no ERP durante a conversa

## Módulo 4 — Lógica de conversa e regras de negócio ✅
`modulos/04-logica-e-regras.md`
- [x] Variáveis, condições e ramificações (scenarios embutidos vs. bloco condicional)
- [x] Regras de escalonamento/handoff: bloco de escalonamento, messaging triggers, dialogue mestre
- [x] Tratamento de erro nas chamadas de API (timeout de 10s, fallback, pedido não encontrado com limite de tentativas)
- [x] Fluxo completo do exemplo do ERP juntando Módulos 2, 3 e 4

## Módulo 5 — Testes e publicação ⬜
`modulos/05-testes-e-publicacao.md`
- [ ] Testando o bot antes de publicar (modo de simulação)
- [ ] Publicando por canal
- [ ] Checklist de boas práticas de conversa para SAC (tom, clareza, saída para humano)

## Módulo 6 — Métricas e manutenção ⬜
`modulos/06-metricas-e-manutencao.md`
- [ ] Relatórios de desempenho (taxa de resolução, CSAT, deflection)
- [ ] Como iterar o fluxo com base em dados reais de atendimento
- [ ] Governança: quem edita, como versionar mudanças no fluxo

## Módulo 7 — Replicando para múltiplas marcas ⬜
`modulos/07-multi-marca.md`
- [ ] Checklist de descoberta por marca (integrações de ERP específicas, tom de voz, fluxos próprios)
- [ ] Template de documentação de fluxo por marca

## Anexos
- [ ] `anexos/glossario.md` — glossário completo de termos
- [ ] `anexos/links-oficiais.md` — links de referência da documentação oficial
- [ ] `anexos/checklist-pre-requisitos.md` — acessos e credenciais necessários (Zendesk admin, API do ERP, conta n8n)

---

## Próximo passo

Começar pelo **Módulo 0**, respondendo as perguntas em aberto listadas em `pesquisa/plataforma-zendesk-ai.md`. Isso evita construir os módulos 2–4 em cima de uma ferramenta ou plano que sua conta não tem.
