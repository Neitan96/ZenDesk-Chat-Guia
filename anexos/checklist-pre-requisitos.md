# Checklist de pré-requisitos

Reúna isso **antes** de começar a construir de verdade (Módulo 2 em diante). Boa parte depende de outras pessoas/times — vale levantar com antecedência para não travar no meio do treinamento esperando acesso.

## Acesso ao Zendesk

- [ ] Login de administrador (ou permissão equivalente) no Admin Center da conta
- [ ] Acesso ao menu **AI** (AI agents e Agent builder) — confirme que não está bloqueado por permissão de papel/role
- [ ] Confirmação de que a conta está em plano **Suite ou Support** (qualquer tier, desde a mudança de pacote de mai/2026 — ver `pesquisa/plataforma-zendesk-ai.md`)
- [ ] Confirmação se **Agent Builder / Custom agents (EAP)** já está habilitado, ou se precisa solicitar acesso antecipado à Zendesk
- [ ] Lista de marcas existentes na conta e quem é o responsável por cada uma (para o Módulo 7)
- [ ] Fila/grupo de destino de escalonamento já criada no Zendesk para cada marca (Módulo 4)

## Acesso ao(s) ERP(s)

Para cada ERP diferente entre as marcas (Módulo 7):

- [ ] Confirmação de que o ERP expõe uma **API** (não só banco de dados/relatórios) — se não expõe, isso é um projeto à parte antes de qualquer integração
- [ ] Documentação da API do ERP (endpoints, autenticação, formato de request/response)
- [ ] Credencial de API dedicada para essa integração (evite reusar uma credencial de uso geral — mais fácil de auditar/revogar se for exclusiva)
- [ ] Ambiente de teste/sandbox do ERP, se existir, separado de produção
- [ ] Contato técnico do time responsável pelo ERP, para quando a integração falhar (Módulo 7)
- [ ] Tempo médio de resposta da API do ERP conhecido — importante por causa do limite de **10 segundos de timeout** do Zendesk (Módulo 3)

## Acesso ao n8n

- [ ] Instância do n8n disponível (self-hosted ou n8n Cloud) — com espaço/plano suficiente para adicionar novos workflows
- [ ] Permissão para criar e publicar workflows nessa instância
- [ ] Forma de proteger os webhooks (header de autenticação, IP allowlist, ou equivalente) — nunca publicar um webhook sem nenhuma validação (Módulo 3)
- [ ] Ambiente de teste separado de produção no n8n, se possível, para validar antes de apontar pro ERP real

## Documentação e organização interna

- [ ] Guia de tom de voz de cada marca (ou alguém que possa fornecer isso), para o Módulo 5/7
- [ ] Acordo com o time sobre o processo informal de "avisar antes de editar" um dialogue (Módulo 6 — não existe trava de edição simultânea)
- [ ] Definição de quem é a pessoa "dona" de cada dialogue por marca (Módulo 7)

## Antes de publicar em produção (revisão final, ligada ao Módulo 5)

- [ ] Todos os cenários de erro testados manualmente (não só o caminho feliz)
- [ ] Conflitos de trigger revisados, se o canal for e-mail/formulário web
- [ ] Primeira publicação planejada para o canal de menor risco, não direto no canal de maior volume
