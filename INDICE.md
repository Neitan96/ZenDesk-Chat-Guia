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

## Módulo 1 — Fundamentos da plataforma de IA do Zendesk ⬜
`modulos/01-fundamentos.md`
- [ ] O que é AI Agents (Essential x Advanced) e o que cada nível libera
- [ ] Canais suportados (webchat, WhatsApp, Instagram, e-mail, etc.)
- [ ] Conceitos-chave: use cases, intents, dialogues, generative procedures, handoff
- [ ] Onde tudo isso vive no Admin Center (mapa de navegação)

## Módulo 2 — Construindo o agente (Agent Builder + Dialogue Builder) ⬜
`modulos/02-agent-builder.md`
- [ ] Criando um agente de IA do zero
- [ ] Dialogue Builder: passo a passo de um fluxo guiado (pergunta → decisão → resposta)
- [ ] Generative procedures: respostas geradas por IA sobre a base de conhecimento
- [ ] Quando usar fluxo guiado vs. resposta generativa

## Módulo 3 — Integrações externas: conectando no ERP via n8n ⬜
`modulos/03-integracoes-erp-n8n.md`
**Módulo central para o seu caso de uso.**
- [ ] Action Builder / Custom Actions: como configurar um passo de chamada de API no fluxo
- [ ] Autenticação (API key / OAuth) para o passo de API
- [ ] Montando o lado n8n: nó Webhook (recebe do Zendesk) + nó HTTP Request (consulta o ERP)
- [ ] Formatando a resposta do n8n para o Zendesk exibir ao cliente
- [ ] Exemplo prático guiado: consultar status de pedido no ERP durante a conversa

## Módulo 4 — Lógica de conversa e regras de negócio ⬜
`modulos/04-logica-e-regras.md`
- [ ] Variáveis, condições e ramificações no fluxo
- [ ] Regras de escalonamento/handoff para atendente humano (com contexto transferido)
- [ ] Tratamento de erro nas chamadas de API (timeout, ERP fora do ar, dado não encontrado)

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
