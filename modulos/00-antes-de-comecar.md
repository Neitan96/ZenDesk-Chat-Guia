# Módulo 0 — Antes de começar

> Diferente dos outros módulos, este não ensina uma ferramenta — é um roteiro para você descobrir, na **sua própria conta Zendesk**, informações que mudam o que os Módulos 1-7 significam na prática. Sem isso, dá pra estudar uma ferramenta que sua conta não tem, ou pular uma trava que só aparece na hora de publicar.

## 0.1 A conta já está na nova experiência de AI Agents, ou ainda no Bot Builder legado?

**Por que importa:** se a conta ainda estiver no legado, os Módulos 2-4 (Dialogue Builder, Action Builder) podem não estar disponíveis até a migração acontecer — e o legado desliga em 10/12/2026 de qualquer forma.

**Como checar:**
1. No Admin Center, procure o menu **AI** na barra lateral.
2. Se existir **AI → AI agents → AI agents** e **AI → Agent builder**, a conta já está na experiência nova.
3. Se o que aparece é algo como "Bot Builder" ou "Flow Builder" isolado, sem essas duas entradas, a conta provavelmente ainda está no legado.

**Se ainda estiver no legado:** não invista tempo aprendendo a interface antiga — ela desliga em breve. Abra um chamado com o suporte Zendesk perguntando o prazo de migração da sua conta para a nova experiência.

## 0.2 A conta já está no novo empacotamento (sem divisão Essential/Advanced)?

**Por que importa:** até 11/05/2026 as integrações via API (o Módulo 3 inteiro) exigiam o add-on pago "Advanced". Isso foi removido, mas nem toda conta recebe atualizações de pacote no mesmo dia.

**Como checar:**
1. Ao tentar configurar uma custom action (Módulo 3) ou abrir o Dialogue Builder, veja se aparece algum aviso de "recurso disponível apenas no plano Advanced" ou similar.
2. Se aparecer, confirme com o time financeiro/administrativo qual plano está contratado e pergunte ao suporte Zendesk se a conta já recebeu o novo empacotamento pós-maio/2026.

**Se ainda estiver no pacote antigo:** os Módulos 3 e 4 (integração e tratamento de erro via API) ficam bloqueados até resolver isso — vale tratar como bloqueio de projeto, não como detalhe técnico.

## 0.3 O Agent Builder / Custom agents (EAP) está habilitado?

**Por que importa:** é opcional para seguir o treinamento — os Módulos 2-4 funcionam também através do "agente padrão" em **AI → AI agents**, sem precisar do Agent Builder. Mas se você quiser construir um agente do zero (mais controle), precisa desse acesso.

**Como checar:**
1. Veja se **AI → Agent builder → Custom agents** existe no menu.
2. Se não existir, isso é esperado — é um recurso em Early Access Program (EAP). Não é erro de configuração sua.

**Se quiser usar:** solicite acesso antecipado ao time de sucesso do cliente/suporte da Zendesk.

## 0.4 Quantas marcas existem e como estão organizadas?

**Por que importa:** define diretamente o Módulo 7 — se são contas Zendesk separadas por marca ou uma conta multi-brand só, e quantos agentes você vai precisar criar (lembrando: um agente nativo por marca, sem exceção prática).

**Como levantar:**
1. Em **Admin Center → Account → Brands** (ou equivalente), liste todas as marcas cadastradas na conta.
2. Para cada marca, anote: canais ativos, fila de atendimento humano, e se o ERP usado é o mesmo das outras marcas ou diferente.
3. Comece esse levantamento já usando o [template de documentação por marca](../modulos/07-multi-marca.md#74-template-de-documentação-de-fluxo-por-marca) do Módulo 7 — evita fazer o levantamento duas vezes.

## 0.5 Glossário rápido: o que é legado x o que é atual

Antes de pesquisar por conta própria e cair em conteúdo desatualizado, tenha isso em mente (versão resumida — glossário completo em [`anexos/glossario.md`](../anexos/glossario.md)):

| Se você ver isso... | ...é legado. O atual é: |
|---|---|
| Bot Builder | Agent Builder |
| Flow Builder / "bot flows" | Dialogue Builder |
| Answer Bot | AI Agents |
| Divisão "Essential vs. Advanced" | Removida em mai/2026 — recursos inclusos em qualquer plano Suite/Support |
| Integration Builder (como recomendação para integração nova) | Action Builder / Custom Actions |

## Checklist de saída do módulo

Antes de abrir o Módulo 1 de verdade (ou revisitá-lo com mais atenção):

- [ ] Confirmei se a conta está na nova experiência de AI Agents (0.1)
- [ ] Confirmei se a conta já recebeu o novo empacotamento sem Essential/Advanced (0.2)
- [ ] Verifiquei se preciso do Agent Builder (EAP) ou se o agente padrão resolve (0.3)
- [ ] Levantei a lista de marcas e comecei o template de documentação por marca (0.4)
- [ ] Revisei o glossário rápido para não me perder em conteúdo desatualizado ao pesquisar por fora (0.5)

## Próximo módulo

Com essas respostas em mãos, siga para o **[Módulo 1 — Fundamentos da plataforma de IA do Zendesk](./01-fundamentos.md)** já sabendo em que ponto exato a sua conta está.
