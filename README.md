# Treinamento: Criação de Chatbots no Zendesk (com integração ERP + n8n)

Guia de estudo para criar chatbots (AI Agents) no Zendesk para atendimento de SAC multi-marca, incluindo consultas em tempo real ao ERP via n8n.

## Como usar este repositório

- [`INDICE.md`](./INDICE.md) — sumário do treinamento, dividido em módulos. Cada módulo vira um arquivo `.md` na pasta `modulos/` conforme for sendo construído. Use os checkboxes para acompanhar o progresso.
- [`pesquisa/plataforma-zendesk-ai.md`](./pesquisa/plataforma-zendesk-ai.md) — pesquisa bruta sobre a plataforma atual de IA/chatbots do Zendesk (fontes oficiais), incluindo um alerta importante sobre a descontinuação do Bot Builder legado.

## Status

🚧 Em construção — o índice está pronto, os módulos serão preenchidos um a um.

## Contexto do projeto

- Uso: gestão de SAC de múltiplas marcas na plataforma Zendesk.
- Necessidade central: o bot precisa consultar sistemas externos (ERP e automações no n8n) durante a conversa para responder o cliente (ex.: status de pedido, dados cadastrais).
- Pré-requisito a confirmar: plano Zendesk (Suite Professional/Enterprise) + add-on **AI Agents Advanced**, necessário para usar chamadas de API/ações customizadas dentro do bot. Sem isso, os módulos de integração (3 e 4) não funcionam na prática — confirme isso com o time de administração da conta antes de investir tempo nos módulos avançados.
