# Módulo 6 — Métricas e manutenção

> O bot publicado (Módulo 5) não é o fim do trabalho — é o início da coleta de dados reais. Este módulo cobre o que medir, como decidir o que ajustar, e como não perder controle de quem mexeu no quê.

## 6.1 As métricas certas (e uma confusão comum)

### Resolução x Deflection — não são a mesma coisa

Essa é a confusão mais comum em métricas de bot, e vale entender bem porque muda a forma como você reporta resultado pro seu time/gestão:

- **Taxa de resolução (resolution rate)**: percentual de conversas em que o problema foi **totalmente resolvido** pelo bot — sem escalar, e sem o cliente voltar a contatar sobre o mesmo assunto em 24–48h. É a métrica que importa de verdade.
- **Deflection rate**: percentual de interações que **não geraram um ticket para um humano** — mesmo que o problema não tenha sido resolvido de fato (ex.: cliente desistiu, ou a resposta não serviu e ele simplesmente não voltou).

Um bot pode ter deflection alto e resolução baixa — ou seja, está "parecendo" economizar atendimento, mas na prática só está empurrando clientes insatisfeitos pra fora sem resolver nada. **Reporte e otimize para resolução, não para deflection.**

### BSAT (satisfação específica do bot)

O **BSAT** mede a satisfação do cliente especificamente com a interação com o bot — separado do CSAT geral do time humano. Isso importa porque um bot com resolução ok mas BSAT baixo indica que a experiência de conversa (Módulo 5 — tom, clareza) está incomodando o cliente mesmo quando ele "tecnicamente" resolveu o problema.

### Onde ver isso

- **Insights Dashboard** (vem em qualquer plano): volume de conversas, taxa de resolução, tendências de escalonamento — o básico do dia a dia.
- **Advanced AI dashboard**: quebra mais fina — conversas entendidas vs. não entendidas, BSAT, analytics do fluxo de conversa (útil para achar em qual bloco do dialogue os clientes mais abandonam ou escalam).

## 6.2 Iterando o fluxo com base em dados reais

Depois dos primeiros dias/semanas no ar (lembrando do Módulo 5: espere pelo menos 48h antes de tirar conclusão), use os dados pra guiar mudanças — não achismo:

1. **Olhe onde o bot mais escala ou é abandonado.** Se um ponto específico do dialogue (ex.: a pergunta do número do pedido) tem taxa alta de abandono, o problema pode ser a redação da pergunta, não a lógica.
2. **Olhe os casos que caíram no Fallback** (Módulo 4) com frequência maior que o esperado — isso é sinal de instabilidade no n8n/ERP, não do dialogue em si. Trate como incidente técnico, não como "ajustar o texto do bot".
3. **Olhe conversas com BSAT baixo mesmo com resolução "sim"** — geralmente aponta pra tom ruim ou demora percebida (a mensagem de espera do Módulo 5 ajuda aqui).
4. **Priorize por volume, não por acaso.** Se 80% das conversas são sobre rastreamento de pedido e 5% sobre outro assunto, otimizar o fluxo de rastreamento move o ponteiro muito mais do que polir um fluxo raramente usado.

Trate isso como ciclo contínuo, não uma tarefa de "lançar e esquecer": publique, colete pelo menos 1–2 semanas de dado real, ajuste, publique de novo.

## 6.3 Governança: quem edita e como não perder controle

### Não existe trava de edição simultânea

**Ponto de atenção real:** se duas pessoas editam o mesmo dialogue ao mesmo tempo, **quem salvar por último sobrescreve tudo** — não há bloqueio nem merge automático. Em um time de SAC multi-marca com mais de uma pessoa mexendo em fluxos, isso é risco concreto de perda de trabalho. Combine informalmente ("avisa no chat antes de editar o dialogue X") até existir algo mais robusto — o Zendesk não resolve isso por você.

### Save vs. Publish, com notas

- **Save** / **Save with note**: salva o progresso sem afetar o que está no ar (retomando o Módulo 5: salvo ≠ publicado).
- **Publish** / **Publish with note**: coloca a mudança em produção.
- **Use sempre a versão "with note"** para descrever o que mudou e por quê (ex.: "ajustei mensagem de espera após BSAT baixo em set/2026"). Isso é o que transforma o histórico de "uma lista de timestamps" em algo que alguém (inclusive você, 3 meses depois) consegue entender sem adivinhar.

### Version history

- Acesse pelo menu de opções (canto superior direito do editor) → **Version history**.
- As versões são agrupadas por dia, e dá pra filtrar por tipo (só publicadas / só suas / salvas e publicadas).
- Cada versão mostra quem salvou, quando, e a nota (se você seguiu a recomendação acima).
- É possível **reverter para uma versão anterior** — útil se uma mudança publicada teve efeito pior que o esperado (ex.: nova redação de mensagem derrubou o BSAT).

### Prática recomendada para múltiplas marcas

- Defina uma pessoa "dona" de cada dialogue por marca, mesmo que mais de uma consiga editar — reduz a chance de duas pessoas mexerem ao mesmo tempo sem saber.
- Sempre publique com nota, principalmente antes de sair do horário de trabalho — se algo quebrar à noite, quem for investigar não vai precisar adivinhar o que mudou.

## Checklist de saída do módulo

- [ ] Você sabe diferenciar taxa de resolução de deflection rate e reporta pela primeira, não pela segunda
- [ ] Você identificou onde ver o Insights Dashboard (e o Advanced, se disponível) da sua conta
- [ ] Você tem um processo (mesmo que informal) para revisar dados reais periodicamente e decidir ajustes — não só "lançar e esquecer"
- [ ] Seu time está ciente de que não há trava de edição simultânea, e combinou uma forma de evitar sobrescrever o trabalho um do outro
- [ ] Vocês adotaram o hábito de publicar sempre com nota

## Próximo módulo

**Módulo 7 — Replicando para múltiplas marcas**: usar tudo dos Módulos 1–6 como base e organizar a expansão para as outras marcas que você atende, sem recomeçar do zero em cada uma.
