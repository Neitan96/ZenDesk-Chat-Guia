# Módulo 4 — Lógica de conversa e regras de negócio

> Este módulo fecha o "caminho infeliz" que ficou pendente no Módulo 3: pedido não encontrado, ERP fora do ar, timeout — e define quando o bot deve parar de insistir sozinho e chamar um humano.

## 4.1 Variáveis, condições e ramificações (aprofundando o Módulo 2)

No Módulo 2 você já usou bloco de **pergunta** (ramifica pela resposta do cliente) e viu que existe o bloco **condicional**. Agora a ramificação não depende do que o cliente digitou, e sim do que **voltou da API**.

- O bloco condicional pode checar qualquer variável do dialogue — incluindo as que vieram do mapeamento da resposta do n8n (`encontrado`, `status_pedido`, etc., do Módulo 3).
- Dá para combinar mais de uma condição (E/OU) — ex.: "`encontrado` é `true` **E** `status_pedido` é diferente de `cancelado`".
- Cada ramo do condicional segue como um dialogue normal: mensagens, novas perguntas, ou outro bloco condicional dentro dele.

**Duas formas de checar o resultado da API:**

1. **Scenarios embutidos no próprio bloco de integração**: ao configurar o passo de API, o Zendesk já permite definir cenários de resposta diretamente ali (2 cenários customizáveis + 1 "Fallback" fixo, que dispara quando nenhum dos outros bate — inclusive em caso de erro técnico). É a forma mais direta para casos simples.
2. **Bloco condicional separado, depois da ação**, checando as variáveis mapeadas (ex.: `encontrado == true`). Mais flexível quando a lógica é mais complexa ou combina múltiplas variáveis.

Para o fluxo do ERP, a recomendação é: use os **scenarios embutidos** para separar "chamada funcionou" de "chamada falhou/timeout" (esse é exatamente o papel do Fallback), e um **bloco condicional** dentro do cenário de sucesso para tratar `encontrado: true` vs. `false`. Isso separa claramente "problema técnico" de "problema de negócio" (pedido não existe).

## 4.2 Tratando os cenários de erro da chamada ao ERP

Relembrando o limite do Módulo 3: **timeout fixo de 10 segundos**, sem exceção. Isso significa que "falha da API" não é caso raro — vai acontecer sempre que o ERP estiver lento ou fora do ar, e o fluxo precisa estar pronto pra isso desde o primeiro dia, não como um ajuste posterior.

| Cenário | O que aconteceu | O que o bot deve fazer |
|---|---|---|
| **Fallback do bloco de integração** | Timeout (>10s), erro HTTP do n8n, ou resposta que não é o JSON esperado | Nunca deixar o cliente sem resposta. Mensagem tipo: "Não consegui consultar seu pedido agora, deixa eu te transferir para um atendente." → bloco de escalonamento (4.3) |
| **Sucesso da API + `encontrado: false`** | O ERP respondeu normalmente, mas não achou o número informado | Não é erro técnico — é problema de dado. Peça para o cliente confirmar o número (permita 1–2 tentativas) antes de escalar |
| **Sucesso da API + `encontrado: true`** | Caminho feliz do Módulo 3 | Segue normalmente |

**Por que separar esses dois tipos de falha?** Porque a resposta certa é diferente: erro técnico não deve virar "confirme o número de novo" (o número pode estar certo, o problema é o ERP), e "não encontrado" não deve virar "transferindo para atendente" de cara (aumenta carga no time humano por algo que o próprio cliente resolve digitando de novo).

## 4.3 Escalonamento e handoff para atendente humano

### Bloco de escalonamento

No ponto do dialogue onde a conversa deve ser transferida:
1. Clique no **+** e adicione um **bloco de escalonamento**.
2. Escreva uma mensagem de transição para o cliente (ex.: "Vou te conectar com um especialista que pode ajudar com isso").
3. Configure a fila/grupo de destino do handoff — o contexto da conversa (incluindo as variáveis já capturadas, como `numero_pedido`) vai junto para o atendente humano ver.

### Gatilhos de escalonamento (quando isso deve acontecer)

Além de colocar o bloco de escalonamento manualmente em pontos do dialogue (como no caso de fallback acima), existem **gatilhos automáticos**, configurados em **Admin Center → Objects and rules → Business rules → Triggers → Messaging triggers** (triggers de mensageria — diferente de triggers de ticket ou de chat):

- Cliente pede explicitamente para falar com humano ("quero falar com atendente", "isso não resolveu").
- O bot falha em resolver após um número determinado de tentativas.
- O tópico está marcado como fora do escopo do agente.
- Uma condição de negócio bate (ex.: sentimento negativo detectado, cliente com tag VIP).

Para criar: **Add trigger** → escolha o evento (mensagem recebida, conversa atualizada) → defina as condições → defina a ação de roteamento.

### Boa prática: um dialogue mestre de escalonamento

Em vez de recriar a lógica de handoff (mensagem + fila + regras) em cada dialogue separado, crie **um dialogue único de escalonamento** e use blocos do tipo **"Link to"** nos outros fluxos para apontar pra ele. Assim, se a mensagem de transição ou a fila de destino mudar, você ajusta em um lugar só, em vez de caçar em cada fluxo.

## 4.4 Fluxo completo do exemplo do ERP, com tratamento de erro

Juntando Módulos 2, 3 e 4 num fluxo só:

```
Cliente escolhe "Rastrear pedido"
        │
        ▼
Pergunta: "Qual o número do pedido?" → variável numero_pedido
        │
        ▼
Bloco de ação/API (custom action → n8n → ERP)
        │
        ├── Fallback (timeout / erro técnico)
        │        └── Mensagem + Link to: dialogue mestre de escalonamento
        │
        └── Sucesso da API
                 │
                 ▼
           Condicional: encontrado == true?
                 │
                 ├── Não (1ª tentativa)
                 │      └── "Não encontrei esse pedido, pode confirmar o número?"
                 │           └── volta para a pergunta do número (máx. 2 tentativas)
                 │
                 ├── Não (após 2ª tentativa)
                 │      └── Link to: dialogue mestre de escalonamento
                 │
                 └── Sim
                        └── "Seu pedido está: {{status_pedido}}, previsão: {{previsao_entrega}}"
```

## Checklist de saída do módulo

- [ ] Você configurou os scenarios do bloco de integração (sucesso vs. Fallback) para a ação do Módulo 3
- [ ] Você tratou separadamente "erro técnico" e "pedido não encontrado", com limite de tentativas antes de escalar
- [ ] Você criou (ou identificou) um dialogue mestre de escalonamento e linkou os outros fluxos a ele, em vez de duplicar a lógica
- [ ] Você revisou se existe algum gatilho automático de escalonamento (mensageria) fazendo sentido para o seu SAC (ex.: sentimento negativo, cliente VIP)

## Próximo módulo

**Módulo 5 — Testes e publicação**: testar esse fluxo completo (incluindo os cenários de erro, não só o caminho feliz) antes de publicar para os clientes de verdade.
