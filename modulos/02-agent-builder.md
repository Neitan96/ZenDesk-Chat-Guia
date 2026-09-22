# Módulo 2 — Construindo o agente (Dialogue Builder + Generative Procedures)

> Objetivo deste módulo: montar um fluxo simples, **sem integração com ERP ainda** (isso é o Módulo 3), só para pegar o jeito da ferramenta. Resistir à tentação de já sair conectando API — primeiro entenda os blocos com um fluxo bobo, testável em minutos.

## 2.1 Criando o agente

1. No Admin Center: **AI → AI agents → AI agents** → criar um novo agente (ou editar o padrão, se a conta já vier com um).
2. Defina: nome do agente, idioma principal e os canais em que ele vai atender (comece só pelo Web Widget para testar — adicionar WhatsApp/Instagram depois é só habilitar o canal, o fluxo é o mesmo).
3. Dentro do agente, você vai ver duas abas principais de conteúdo: **Dialogues** (fluxos guiados) e **Procedures** (respostas geradas por IA). Este módulo cobre as duas.

Se sua conta tiver o **Agent Builder → Custom agents** habilitado (lembrando: EAP, ver Módulo 1), o caminho é parecido, mas o agente é construído do zero fora do "agente padrão" — a lógica de dialogue/procedure é a mesma.

## 2.2 Dialogue Builder: montando o primeiro fluxo guiado

O Dialogue Builder é um editor visual em forma de fluxograma. Você monta a conversa bloco por bloco, clicando no ícone **+** entre os blocos para inserir o próximo passo.

### Blocos principais

| Bloco | Para que serve |
|---|---|
| **Mensagem do agente** | O bot fala algo (texto fixo, pode incluir variáveis). Todo dialogue já começa com um. |
| **Mensagem do cliente / pergunta** | Captura o que o cliente digita ou clica, e define como o fluxo se ramifica a partir da resposta (ex.: "Rastrear pedido" vs. "Solicitar reembolso" → dois caminhos diferentes). |
| **Condicional** | Ramifica o fluxo com base em uma condição — não necessariamente algo que o cliente digitou agora: pode ser um dado de CRM, da sessão, ou (mais pra frente, Módulo 3) o retorno de uma chamada de API. |
| **Generative replies** | Insere uma resposta gerada por IA dentro de um ponto específico do dialogue, em vez de texto fixo — útil quando parte do fluxo é previsível mas um trecho precisa de resposta mais flexível. |
| **Variável** | Um "container" de dado da conversa (nome do cliente, número do pedido, etc.), reutilizável em qualquer mensagem depois de capturado. |

### Passo a passo de um fluxo de exemplo (sem API ainda)

Vamos montar isso: *"o bot pergunta se o cliente quer rastrear pedido ou falar com humano"*.

1. **Bloco inicial (mensagem do agente):** "Olá! Posso te ajudar com o quê hoje?"
2. **Bloco de pergunta:** duas opções de resposta — "Rastrear pedido" / "Falar com atendente".
3. **Ramo "Rastrear pedido":**
   - Bloco de pergunta: "Qual o número do pedido?"
   - Capture a resposta numa **variável** (ex.: `numero_pedido`).
   - Bloco de mensagem do agente: "Deixa eu verificar o pedido {{numero_pedido}}..." — aqui é onde, no Módulo 3, vai entrar a chamada de API para o ERP. Por enquanto, deixe uma mensagem fixa tipo "essa função ainda está em construção" para poder testar o fluxo de ponta a ponta.
4. **Ramo "Falar com atendente":**
   - Bloco de **handoff** (transferência para humano) — configure a fila/grupo de destino e a mensagem de transição.

### Boas práticas ao montar o dialogue

- Sempre dê às perguntas opções claras e finitas (botões) quando possível — texto livre aumenta a chance do bot não reconhecer a intenção certa.
- Nomeie variáveis de forma descritiva (`numero_pedido`, não `var1`) — você vai reusar isso no Módulo 3 e vai agradecer depois.
- Teste cada ramo isoladamente antes de conectar tudo (o Zendesk tem modo de pré-visualização/teste do dialogue — usar antes de publicar).

## 2.3 Generative Procedures: respostas geradas por IA

Procedures são o caminho para perguntas mais abertas, onde não faz sentido desenhar uma árvore fixa (ex.: "qual a política de troca de produto X?").

### Como criar uma procedure

1. Na aba **Procedures**, clique em **Create procedure**.
2. Descreva em texto livre e em ordem lógica os passos que um atendente humano seguiria para resolver aquele tipo de pedido — como se estivesse treinando uma pessoa nova.
3. Instrua explicitamente onde o agente deve buscar a resposta (ex.: "busque na central de ajuda os artigos sobre política de trocas") — a Zendesk recomenda ser explícito sobre a fonte, não deixar implícito.
4. Use `/` ou o ícone **+** para inserir, dentro do texto da procedure, referências a: ações/integrações de API, parâmetros, regras de busca, ou links para outras procedures/dialogues.
5. O sistema gera automaticamente um **mapa da procedure** (visualização dos passos que o agente vai seguir) — revise esse mapa e ajuste o texto até a lógica fazer sentido antes de publicar.

### Boas práticas específicas de procedures

- Escreva como instrução para humano, não como pseudocódigo — a Zendesk usa isso como linguagem natural interpretada pela IA, não uma DSL.
- Seja explícito sobre a fonte de dados em cada passo (evita a IA "inventar" resposta quando não teria a informação).
- Revise sempre o mapa gerado — é comum o primeiro rascunho pular um passo óbvio para um humano mas não óbvio pra IA.

## 2.4 Dialogue x Procedure: quando usar cada um

| Situação | Use |
|---|---|
| Fluxo tem regra de negócio exata e passos fixos (ex.: troca de produto com etapas obrigatórias) | **Dialogue** |
| Passo precisa buscar um dado exato de um sistema externo num ponto certo da conversa (ex.: consultar status de pedido no ERP) | **Dialogue** com um bloco de ação de API — mais controle sobre onde e como a chamada acontece |
| Pergunta é aberta e a resposta pode variar bastante, mas está coberta pela base de conhecimento (ex.: "quais as formas de pagamento?") | **Procedure** |
| Combinação dos dois | Comum e recomendado: procedures cobrindo perguntas abertas, dialogues cobrindo os fluxos que precisam de dado exato — os dois podem coexistir no mesmo agente, e uma procedure pode até linkar para um dialogue específico. |

Para o seu caso (consulta ao ERP), a recomendação é **dialogue**, não procedure — você precisa de controle exato sobre em que ponto a chamada de API acontece e o que fazer com cada tipo de resposta (pedido encontrado, não encontrado, erro). Isso é o assunto do Módulo 3.

## Checklist de saída do módulo

- [ ] Você criou (ou identificou) um agente de teste no Admin Center
- [ ] Você montou um dialogue simples de 2 ramos (mesmo sem API real) e testou os dois caminhos no modo de pré-visualização
- [ ] Você entende a diferença prática entre dialogue e generative procedure e por que o fluxo de ERP vai ser um dialogue
- [ ] Você sabe onde inserir um bloco de ação/API dentro de um dialogue (mesmo sem ainda ter configurado uma)

## Próximo módulo

**Módulo 3 — Integrações externas: conectando no ERP via n8n**: substituir a mensagem fixa "essa função ainda está em construção" por uma chamada de API real, passando pelo n8n até o ERP.
