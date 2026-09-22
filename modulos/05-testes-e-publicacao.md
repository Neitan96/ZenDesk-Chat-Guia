# Módulo 5 — Testes e publicação

> Até aqui o fluxo existe só dentro do editor. Este módulo cobre como testar de verdade antes de qualquer cliente real ver isso — incluindo os cenários de erro do Módulo 4, não só o caminho feliz — e como publicar sem quebrar coisa que já está no ar.

## 5.1 Regra de ouro: mudança salva ≠ mudança publicada

No Dialogue Builder, as alterações são salvas automaticamente enquanto você edita, **mas não ficam visíveis para os clientes até você clicar em Publish**. Isso é proposital: é a rede de segurança contra editar em produção sem querer. Na prática, significa que dá pra editar um fluxo já publicado, testar à vontade, e só afetar clientes reais no momento em que você decidir publicar.

## 5.2 Testando o dialogue antes de publicar

### Test dialogue (teste do fluxo inteiro)

No canto superior direito do editor do dialogue, o botão **"Test dialogue"** abre um widget de teste que simula a experiência do cliente de ponta a ponta — começando pela mensagem de boas-vindas e já considerando as configurações ativas (incluindo instruções do agente).

### Test branch (teste de um ramo específico)

Nem sempre você quer refazer a conversa inteira pra testar só o pedaço que mudou. Passe o mouse sobre o bloco onde quer começar e use **"Test branch"** — funciona a partir de qualquer tipo de bloco, exceto blocos de "Customer message" e "Link to". Isso é especialmente útil depois de mexer no Módulo 3/4: você pode testar só o trecho da chamada de API + tratamento de erro sem repetir a pergunta do número do pedido toda vez.

### Session parameters (testando com dados específicos)

No diálogo de **Session parameters**, dá para testar informando um parâmetro e um valor específico (ex.: simular um `numero_pedido` que você sabe que não existe no ERP, para forçar o caminho de "não encontrado") — ou testar sem parâmetros para simular um cliente novo, sem contexto prévio.

### O que testar obrigatoriamente antes de publicar (voltando ao Módulo 3/4)

Não basta testar o caminho feliz. Use o Test branch + Session parameters para forçar cada um destes cenários:

- [ ] Pedido existente → aparece o status certo
- [ ] Número de pedido inexistente → mensagem de "não encontrado" + pede confirmação (1ª tentativa)
- [ ] Número errado de novo → escala para atendente (2ª tentativa, conforme Módulo 4)
- [ ] ERP/n8n fora do ar ou lento (se der pra simular desligando o workflow no n8n temporariamente) → cai no Fallback e escala, sem travar o cliente
- [ ] Cliente pede "falar com atendente" no meio do fluxo → escalonamento manual funciona

Se qualquer um desses cenários não tiver sido testado manualmente, ele não está pronto pra ir pro ar — "deve funcionar" não é a mesma coisa que "eu vi funcionando".

## 5.3 Publicando por canal

1. No agente, escolha os **canais** em que ele deve ficar disponível (mensageria, e-mail, formulário web). Um agente pode ser publicado em vários canais e até vários tipos de canal ao mesmo tempo — mas **um canal só pode ter um agente publicado nele por vez** (não dá pra ter dois agentes competindo pelo mesmo canal).
2. Antes de finalizar, o Zendesk avisa sobre **conflitos de trigger** (principalmente em canais de e-mail e formulário web) — se o sistema encontrar triggers existentes que podem competir com o novo (ex.: risco de mandar e-mail duplicado pro cliente), ele mostra um aviso com link direto pros triggers conflitantes. Resolva isso (geralmente desativando o trigger antigo) antes de publicar.
3. Clique em **Publish**.

**Recomendação prática para múltiplas marcas:** comece publicando só no canal de menor risco (web widget do site, por exemplo) antes de estender para WhatsApp/Instagram — lembrando do Módulo 1 que nem todo componente visual (botões, carrossel) se comporta igual em todo canal. Validar num canal simples primeiro isola se um problema é do fluxo ou do canal.

## 5.4 Depois de publicar: os primeiros dias

- Espere pelo menos **48 horas** de interação real antes de tirar conclusões de desempenho — é o prazo que o próprio Zendesk recomenda antes de olhar o dashboard de Insights com significância mínima de dados (métricas detalhadas ficam para o Módulo 6).
- Nos primeiros dias, acompanhe de perto as conversas que caíram no Fallback (Módulo 4) — é o sinal mais rápido de que algo no n8n/ERP está mais instável ou lento do que o esperado em teste.

## 5.5 Checklist de boas práticas de conversa para SAC

Antes de publicar, revise o texto de cada mensagem do dialogue contra isso:

- **Tom**: consistente com a marca (formal/informal), sem soar robótico repetindo a mesma estrutura de frase em toda resposta.
- **Clareza**: uma pergunta por vez. Evite mensagens que peçam duas informações ao mesmo tempo ("me diga seu CPF e número do pedido") — quebre em dois passos, é mais fácil de responder e de capturar em variáveis separadas.
- **Transparência**: deixe claro quando é um bot falando, e nunca finja ser um humano.
- **Saída sempre visível**: em qualquer ponto do fluxo, o cliente devia conseguir pedir para falar com um atendente (o gatilho de escalonamento do Módulo 4 cobre isso, mas vale revisar se ele está ativo em todos os dialogues, não só no de exemplo).
- **Mensagens de espera**: ao redor da chamada de API (que pode levar até os 10 segundos de timeout do Módulo 3), inclua uma mensagem tipo "Só um momento, consultando..." — sem isso, o cliente pode achar que o bot travou.
- **Sem jargão interno**: nomes de sistemas, códigos de status internos do ERP etc. não devem vazar pro texto que o cliente vê — traduza (`status_pedido: "SHP"` não deve virar `SHP` na tela do cliente, e sim algo como "Em transporte").

## Checklist de saída do módulo

- [ ] Você testou o fluxo completo (caminho feliz) com Test dialogue
- [ ] Você testou os cenários de erro do Módulo 4 individualmente com Test branch + Session parameters
- [ ] Você revisou conflitos de trigger antes de publicar (se o canal for e-mail/formulário web)
- [ ] Você publicou primeiro num canal de menor risco antes de estender para os demais
- [ ] Você passou o texto de cada mensagem pelo checklist de tom/clareza/jargão

## Próximo módulo

**Módulo 6 — Métricas e manutenção**: agora que está no ar, como acompanhar taxa de resolução, CSAT e deflection, e como decidir o que ajustar no fluxo com base em dados reais em vez de achismo.
