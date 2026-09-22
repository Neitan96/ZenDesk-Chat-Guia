# Módulo 7 — Replicando para múltiplas marcas

> Este módulo assume que você já tem um agente funcionando numa marca (Módulos 1-6) e agora precisa estender para as demais.

## 7.1 Realidade importante: não existe "um agente para todas as marcas"

**Nativamente, o Zendesk exige um AI Agent separado por marca** — cada agente só acessa a central de ajuda/base de conhecimento de uma marca específica. Não existe configuração para um único agente atender várias marcas com bases de conhecimento diferentes.

Existe um workaround parcial (conectar um agente ao canal de e-mail, coletar a marca via uma action, usar blocos condicionais no dialogue para separar os fluxos por marca, e configurar regras de busca para cada marca olhar só a base de conhecimento certa) — mas é mais frágil e mais difícil de manter do que simplesmente ter um agente por marca. **Recomendação: um agente por marca**, salvo se você tiver um motivo forte (ex.: volume baixíssimo em algumas marcas) para forçar o workaround.

**O que isso muda no seu planejamento:** "replicar" aqui significa recriar o agente e os dialogues em cada marca — o trabalho que se reaproveita é a **arquitetura por trás** (Módulos 3 e 4), não o agente publicado em si.

## 7.2 O que reaproveitar entre marcas (e o que não dá)

| Peça | Reaproveita? | Como |
|---|---|---|
| Workflow do n8n (Webhook + HTTP Request + formatação) | ✅ Sim, com adaptação | Se todas as marcas usam o **mesmo ERP**, um único workflow no n8n pode servir todas — recebendo um campo `marca` no body e usando ele para rotear internamente (ex.: schema/tenant diferente no ERP por marca). Se cada marca tem um **ERP diferente**, é mais simples ter um workflow n8n por marca (ou por ERP), mesmo que a estrutura interna seja parecida — copiar e adaptar é mais seguro que uma lógica condicional gigante dentro de um workflow só. |
| Estrutura do dialogue (blocos, lógica de erro do Módulo 4) | ✅ Sim, como template | O esqueleto (pergunta → ação de API → scenarios de sucesso/fallback → condicional de encontrado/não encontrado → escalonamento) é igual. Você duplica esse padrão em cada marca e só troca textos/endpoints. |
| Dialogue mestre de escalonamento (Módulo 4) | ⚠️ Parcial | A lógica é igual, mas o **destino** (fila/grupo) muda por marca — cada marca precisa do seu próprio dialogue mestre de escalonamento apontando pra fila certa. |
| Tom de voz, textos das mensagens | ❌ Não | Cada marca tem identidade própria — reescrever é obrigatório, não é preguiça pular essa etapa. |
| Credenciais/connections (API key, OAuth) | ❌ Não | Nunca reutilize a mesma credencial de ERP entre marcas, mesmo que tecnicamente funcione — isso mistura o rastro de acesso e dificulta auditoria/revogação por marca. |

## 7.3 Checklist de descoberta por marca

Antes de replicar, levante isso para **cada marca nova**:

- [ ] **ERP**: é o mesmo sistema das marcas já atendidas, ou é outro? Se for outro, o Módulo 3 precisa ser refeito (endpoint, autenticação, formato de resposta) — não é só trocar uma URL.
- [ ] **Canais**: em quais canais essa marca realmente atende hoje (Módulo 1)? Não assuma que é igual à primeira marca.
- [ ] **Tom de voz**: existe um guia de tom de marca já documentado, ou você vai precisar definir isso junto com quem cuida da marca?
- [ ] **Fluxos próprios**: essa marca tem alguma dúvida frequente que as outras não têm (ex.: um tipo de produto exclusivo)? Isso vira um use case/dialogue que não existe nas outras marcas.
- [ ] **Fila de escalonamento**: para qual grupo/fila os atendentes humanos dessa marca estão, no Zendesk?
- [ ] **Contatos técnicos**: quem no time de TI/ERP dessa marca você aciona se a integração falhar?

## 7.4 Template de documentação de fluxo por marca

Documentar cada marca da mesma forma facilita manutenção (Módulo 6) e onboarding de quem for mexer nisso depois de você. Sugestão de estrutura — crie um arquivo por marca em `marcas/<nome-da-marca>.md`:

```markdown
# Marca: [Nome da marca]

## Dados gerais
- Conta/instância Zendesk:
- Canais ativos:
- Fila de escalonamento (grupo/view):
- Contato técnico do ERP:

## Integração com ERP
- Sistema ERP:
- Mesmo ERP de outra marca? (se sim, qual):
- Workflow n8n usado: [link ou nome do workflow]
- Connection/credencial no Zendesk: [nome da connection, sem expor a credencial aqui]
- Timeout observado em teste (lembrando do limite de 10s do Zendesk):

## Dialogues ativos
| Dialogue | Use case | Última publicação | Responsável |
|---|---|---|---|
| Rastreamento de pedido | Consultar status no ERP | dd/mm/aaaa | |
| Escalonamento (mestre) | Handoff para humano | dd/mm/aaaa | |

## Particularidades desta marca
- Tom de voz: [ex.: formal / descontraído / etc.]
- Fluxos exclusivos desta marca (não existem nas outras):
- Diferenças de canal em relação às outras marcas:

## Métricas de referência (Módulo 6)
- Taxa de resolução (última checagem):
- BSAT (última checagem):
- Observações de Fallback frequente:
```

Mantenha um arquivo desses por marca — quando uma nova pessoa entrar no time ou você precisar debugar um problema numa marca específica, esse documento economiza ter que reconstruir o contexto do zero.

## Checklist de saída do módulo

- [ ] Você confirmou que vai criar um agente por marca (não o workaround de agente único)
- [ ] Você decidiu, para cada ERP diferente entre marcas, se vale um workflow n8n compartilhado (parametrizado) ou um por marca
- [ ] Você preencheu o checklist de descoberta (7.3) para a próxima marca a ser implementada
- [ ] Você criou o arquivo de documentação (7.4) para pelo menos a primeira marca já publicada

## Treinamento completo

Com os Módulos 1–7, você tem o ciclo inteiro: fundamentos → construir o agente → integrar com ERP via n8n → tratar erros e escalonamento → testar e publicar → medir e manter → replicar para outras marcas. Os itens que ainda restam no índice (Módulo 0 e os anexos) são material de apoio/checklist — não bloqueiam usar o que já foi construído.
