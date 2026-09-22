# Treinamento: Criação de Chatbots no Zendesk (com integração ERP + n8n)

Guia de estudo para criar chatbots (AI Agents) no Zendesk para atendimento de SAC multi-marca, incluindo consultas em tempo real ao ERP via n8n.

## Como usar este repositório

- [`INDICE.md`](./INDICE.md) — sumário do treinamento, com link para cada módulo. Use os checkboxes para acompanhar o que já foi aplicado na prática.
- [`modulos/`](./modulos/) — os 8 módulos do treinamento (0 a 7), do levantamento inicial na sua conta até a replicação para múltiplas marcas.
- [`anexos/`](./anexos/) — glossário, links de referência oficiais e checklist de pré-requisitos.
- [`pesquisa/plataforma-zendesk-ai.md`](./pesquisa/plataforma-zendesk-ai.md) — pesquisa bruta sobre a plataforma atual de IA/chatbots do Zendesk (fontes oficiais), incluindo o alerta sobre a descontinuação do Bot Builder legado.

## Status

✅ Conteúdo completo (Módulos 0–7 + anexos). Comece pelo [Módulo 0](./modulos/00-antes-de-comecar.md) para confirmar o estado da sua conta antes de seguir para o resto.

## Contexto do projeto

- Uso: gestão de SAC de múltiplas marcas na plataforma Zendesk.
- Necessidade central: o bot precisa consultar sistemas externos (ERP e automações no n8n) durante a conversa para responder o cliente (ex.: status de pedido, dados cadastrais).
- Pré-requisito: plano Zendesk **Suite ou Support** (qualquer tier). Desde maio/2026 as integrações via API não exigem mais um add-on separado (ver [Módulo 0](./modulos/00-antes-de-comecar.md) e [`anexos/checklist-pre-requisitos.md`](./anexos/checklist-pre-requisitos.md) para confirmar isso na sua conta especificamente).
