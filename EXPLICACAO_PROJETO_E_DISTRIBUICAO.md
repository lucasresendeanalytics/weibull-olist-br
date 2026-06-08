# Explicacao do Projeto, da Distribuicao Weibull e da Relevancia para Decisao

## 1. O que este projeto realmente quer resolver

Este projeto usa a base publica da Olist para responder uma pergunta de negocio muito comum em CRM:

**quem vale a pena ativar, quando, e com qual urgencia?**

A ideia central nao e prever apenas "quem vai comprar de novo", mas modelar o **tempo ate a proxima compra**. Isso muda bastante a qualidade da decisao, porque permite sair de regras arbitrarias como:

- "mandar campanha para todo mundo com 60 dias sem comprar"
- "reativar toda a base no mesmo intervalo"
- "tratar todos os clientes silenciosos da mesma forma"

No lugar disso, o projeto propoe uma decisao probabilistica:

> dado que o cliente esta ha `t0` dias sem comprar, qual a chance de ele recomprar nos proximos `delta_t` dias?

Quando essa probabilidade e combinada com ticket medio, margem e custo de ativacao, ela deixa de ser so uma estatistica e vira uma **prioridade economica**.

## 2. O que a distribuicao Weibull representa aqui

A distribuicao Weibull e usada para modelar **tempo ate um evento**. Neste caso, o evento e a **recompra**.

Em termos simples:

- cada cliente tem um intervalo entre compras
- esses intervalos formam um padrao na base
- a Weibull tenta descrever esse padrao matematicamente

Isso permite responder perguntas como:

- Clientes costumam recomprar rapido ou demoram?
- A chance de recompra cai devagar ou despenca logo no inicio?
- Depois de quantos dias o silencio passa a ser realmente preocupante?

## 3. Por que a Weibull e boa para esse tipo de problema

A Weibull e valiosa porque ela e flexivel e interpreta o comportamento ao longo do tempo.

O parametro mais importante para explicar em negocio e o `beta`:

- `beta < 1`: a chance instantanea de recomprar cai com o tempo
- `beta = 1`: o risco fica aproximadamente constante
- `beta > 1`: a chance instantanea cresce com o tempo

Para CRM, isso tem leitura direta:

- Se `beta < 1`, a janela de recuperacao e curta.
- Se `beta` fica abaixo de 1 com folga, perder os primeiros dias custa caro.
- Se a chance cai rapido, esperar demais para agir piora o retorno da campanha.

Ou seja, a distribuicao nao serve apenas para "ajustar uma curva". Ela mostra **como o tempo destrói valor**.

## 4. O que significa "hazard" sem linguagem academica

Na analise de sobrevivencia, hazard e a "chance instantanea" de o evento acontecer, dado que ele ainda nao aconteceu.

Traduzindo para o projeto:

- o cliente ainda nao recomprou
- entre hoje e logo adiante, qual e a intensidade dessa chance de recompra?

Se o hazard diminui rapido, isso quer dizer:

- clientes muito recentes ainda tem boa chance de voltar
- conforme o tempo passa, fica cada vez menos provavel que voltem sozinhos
- portanto, o custo de esperar sobe

Essa leitura e muito util para priorizacao de CRM, porque define **tempo de resposta**.

## 5. Como isso vira decisao de negocio

O projeto faz uma ponte entre estatistica e acao.

Fluxo logico:

1. Medir o comportamento de recompra da base.
2. Ajustar a Weibull aos intervalos entre compras.
3. Calcular a probabilidade condicional de recompra para cada cliente.
4. Traduzir essa probabilidade em valor esperado.
5. Comparar valor esperado com custo de ativacao.
6. Priorizar clientes, canais e intensidade de campanha.

Em linguagem de negocio:

- probabilidade sem valor financeiro e curiosidade analitica
- valor sem probabilidade e chute comercial
- os dois juntos viram criterio de priorizacao

## 6. O que e "custo de oportunidade da inacao"

Este e um dos pontos mais fortes do projeto.

Em vez de perguntar apenas:

- "quem tem maior chance de comprar?"

ele tambem pergunta:

- "quanto dinheiro deixo na mesa se eu nao agir?"

Uma forma intuitiva de pensar e:

`valor esperado da recompra = probabilidade de recompra x ticket medio x margem`

Depois:

`valor liquido potencial = valor esperado - custo de ativacao`

Assim, dois clientes com a mesma probabilidade podem ter prioridades diferentes:

- um tem ticket alto e merece acao imediata
- outro tem ticket baixo e pode ir para um fluxo mais barato

Isso evita uma distorcao comum: campanhas massivas que parecem grandes em volume, mas fracas em retorno.

## 7. Como explicar a "distribuicao" para pessoas nao tecnicas

Uma boa forma de explicar distribuicao e:

> distribuicao e o desenho matematico de como os intervalos entre compras se espalham no tempo.

Mais simples ainda:

- nao basta saber a media
- precisamos saber a forma do comportamento
- a distribuicao mostra se a recompra se concentra cedo, se espalha no tempo ou quase desaparece depois de certo ponto

Exemplo pratico:

- duas bases podem ter media de 40 dias entre compras
- na primeira, quase todo mundo recompra entre 10 e 25 dias
- na segunda, uma parte recompra em 7 dias e outra parte so em 90

A media pode ser parecida, mas a estrategia de CRM deveria ser completamente diferente.

Por isso a distribuicao importa mais do que um unico numero resumo.

## 8. O que este projeto ensina para tomada de decisao

### Regra 1: tempo e uma variavel economica

Dias sem compra nao sao apenas um marcador operacional. Eles alteram a probabilidade de retorno e, portanto, o valor esperado do cliente.

### Regra 2: nem todo silencio significa o mesmo risco

Um cliente silencioso ha 12 dias pode ser normal.
Um cliente silencioso ha 45 dias pode estar escapando.
Um cliente silencioso ha 120 dias talvez so justifique acao barata.

### Regra 3: priorizar por retorno esperado e melhor do que priorizar por volume

Uma fila de campanha guiada por oportunidade economica tende a usar melhor verba, equipe e canais.

### Regra 4: segmentacao boa junta comportamento e valor

O projeto sugere uma matriz 2x2:

- alta probabilidade + alto ticket
- baixa probabilidade + alto ticket
- alta probabilidade + baixo ticket
- baixa probabilidade + baixo ticket

Isso e melhor do que segmentar so por recencia ou so por gasto.

## 9. Como traduzir isso em acoes reais

### A. Acoes de CRM

- Criar uma fila diaria de clientes ordenada por custo de oportunidade.
- Definir janelas de ativacao por canal: email, WhatsApp, push, midia paga, time comercial.
- Separar campanhas de fidelizacao, cross-sell, win-back e automacao barata.

### B. Acoes de BI e analytics

- Monitorar a curva de recompra por cohort.
- Reestimar os parametros da Weibull com frequencia.
- Medir ROI por segmento e por timing de ativacao.

### C. Acoes de marketing

- Testar mensagem diferente por fase de risco.
- Concentrar incentivos onde o valor esperado supera o custo.
- Reduzir investimento em clientes com baixa chance e baixo valor.

### D. Acoes de gestao

- Trocar meta de "volume de disparos" por "valor economico protegido ou recuperado".
- Criar SLA de resposta para segmentos mais urgentes.
- Usar a distribuicao como criterio de planejamento de verba.

## 10. Exemplos de decisao que o modelo ajuda a responder

- Vale disparar campanha para toda a base silenciosa?
- Depois de quantos dias a probabilidade cai forte?
- Qual grupo merece cupom e qual grupo merece so lembrete?
- Em quem o time comercial deve ligar primeiro?
- Em qual ponto o custo da acao passa a ser maior que o valor esperado?

## 11. Riscos de interpretar errado

- Confundir alta probabilidade com alta prioridade financeira.
- Ignorar margem e olhar so para receita bruta.
- Usar o modelo uma vez e nunca recalibrar.
- Aplicar a mesma curva para categorias ou perfis muito diferentes.
- Assumir que o modelo substitui comunicacao, oferta ou canal.

O modelo melhora a decisao sobre **para quem agir e quando agir**. Ele nao substitui a qualidade da oferta nem a execucao da campanha.

## 12. Resumo executivo em uma frase

Este projeto usa a distribuicao Weibull para transformar tempo sem compra em probabilidade de recompra e, depois, em prioridade economica de CRM.

## 13. Resumo executivo em tres linhas

- A distribuicao mostra como a recompra se comporta no tempo.
- Isso permite estimar urgencia real, e nao apenas recencia arbitraria.
- Combinando probabilidade, ticket, margem e custo de ativacao, a empresa decide onde agir primeiro para proteger retorno.
