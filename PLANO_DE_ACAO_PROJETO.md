# Plano de Acao para Transformar o Projeto em Entregas Reais

## Objetivo

Transformar a ideia analitica do projeto em um fluxo repetivel de decisao para CRM, retencao e priorizacao comercial.

## 1. Entrega minima viavel

O primeiro marco pratico nao precisa ser uma plataforma completa. Pode ser um processo simples e util:

- entrada: historico de pedidos por cliente
- processamento: ajuste da Weibull e calculo da probabilidade condicional
- saida: tabela priorizada de clientes com recomendacao de acao

Exemplo de colunas da saida:

- `customer_id`
- `dias_desde_ultima_compra`
- `prob_recompra_30d`
- `ticket_medio`
- `margem_estimada`
- `valor_esperado`
- `custo_ativacao`
- `custo_oportunidade`
- `segmento`
- `acao_recomendada`

## 2. Traducao do projeto em etapas operacionais

### Etapa 1: consolidar os dados

Pergunta que responde:

> temos historico confiavel para medir recompra?

Acoes:

- Unir pedidos, pagamentos e clientes.
- Garantir uma linha do tempo consistente por cliente.
- Remover cancelamentos, duplicidades e datas invalidas.
- Escolher a data que vale como referencia de compra efetiva.

Entregavel:

- base analitica por cliente e por pedido

### Etapa 2: construir a metrica de intervalo entre compras

Pergunta que responde:

> qual o tempo observado entre compras sucessivas?

Acoes:

- Ordenar compras por cliente.
- Calcular diferenca entre compras consecutivas.
- Criar distribuicao desses intervalos.
- Separar clientes com 1 compra dos clientes com recompra observada.

Entregavel:

- tabela de intervalos entre compras

### Etapa 3: ajustar a distribuicao Weibull

Pergunta que responde:

> qual e o formato matematico do comportamento de recompra?

Acoes:

- Estimar `lambda` e `beta`.
- Validar se o ajuste faz sentido visualmente e estatisticamente.
- Verificar se `beta < 1`, `= 1` ou `> 1`.

Entregavel:

- parametros do modelo e interpretacao de negocio

### Etapa 4: calcular probabilidade condicional de recompra

Pergunta que responde:

> dado o silencio atual do cliente, qual a chance de recompra no horizonte escolhido?

Acoes:

- Definir horizonte de decisao, por exemplo 7, 15 ou 30 dias.
- Calcular a chance de recompra condicional para cada cliente ativo na base.
- Comparar probabilidades entre faixas de recencia.

Entregavel:

- score de probabilidade por cliente

### Etapa 5: transformar probabilidade em valor economico

Pergunta que responde:

> onde esta o dinheiro que justifica acao?

Acoes:

- Estimar ticket medio por cliente ou segmento.
- Aplicar margem esperada.
- Descontar custo de ativacao.
- Calcular valor esperado e custo de oportunidade.

Entregavel:

- ranking economico de priorizacao

### Etapa 6: segmentar e definir acao

Pergunta que responde:

> o que fazer com cada tipo de cliente?

Acoes:

- Montar matriz por probabilidade e valor.
- Associar estrategia para cada quadrante.
- Definir canal, urgencia e intensidade de incentivo.

Entregavel:

- playbook de CRM por segmento

## 3. Como o negocio pode usar isso na pratica

### Caso 1: CRM de rotina

Uso:

- rodada diaria ou semanal
- priorizacao automatica da base
- acionamento por jornada e valor esperado

Decisao:

- quem entra na campanha agora
- quem pode esperar
- quem vai para fluxo barato

### Caso 2: win-back

Uso:

- detectar clientes valiosos cuja chance de retorno esta se deteriorando

Decisao:

- em quem vale investir cupom, frete ou contato humano

### Caso 3: budget allocation

Uso:

- distribuir verba entre segmentos com maior concentracao de oportunidade

Decisao:

- parar de pulverizar verba igualmente

### Caso 4: gestao comercial

Uso:

- enviar para time comercial uma fila priorizada de contato

Decisao:

- ligar primeiro para quem combina alto valor com risco de perda

## 4. Perguntas executivas que o projeto deve responder

Se o projeto for bem finalizado, ele deve entregar respostas objetivas para perguntas como:

- Depois de quantos dias o risco de perda acelera?
- Qual o melhor horizonte para ativacao?
- Quanto valor esperado existe na base silenciosa hoje?
- Qual percentual dos clientes concentra a maior parte da oportunidade?
- Qual segmento merece canal caro e qual segmento merece automacao?

## 5. KPIs recomendados

Para a analise:

- `beta` estimado
- `lambda` estimado
- probabilidade media de recompra por faixa de recencia
- Gini do custo de oportunidade

Para a operacao:

- taxa de recompra apos ativacao
- uplift por segmento
- ROI por canal
- custo por cliente reativado
- valor recuperado por campanha

## 6. Como explicar para lideranca em uma reuniao

Mensagem curta:

> hoje tratamos o silencio do cliente com regras fixas. Este modelo mostra que o risco de perda muda ao longo do tempo e que esse risco pode ser convertido em dinheiro esperado. Assim, a gente deixa de disparar campanha por volume e passa a priorizar por retorno.

## 7. Roadmap sugerido

### Fase 1: prova analitica

- Rodar o notebook ponta a ponta.
- Validar distribuicao e principais graficos.
- Confirmar se o sinal economico faz sentido.

### Fase 2: produto analitico

- Gerar tabela final por cliente.
- Criar export CSV ou XLSX.
- Formalizar segmentos e regras de acao.

### Fase 3: rotina operacional

- Agendar execucao recorrente.
- Entregar lista priorizada para CRM ou comercial.
- Medir retorno e recalibrar.

### Fase 4: refinamento

- Separar curvas por categoria, canal ou cohort.
- Testar outros horizontes.
- Comparar Weibull com modelos alternativos.

## 8. Proximo passo tecnico recomendado neste repositorio

Hoje, os arquivos do repositorio descrevem uma proposta forte, mas a implementacao local parece incompleta. O passo mais util agora seria criar uma versao funcional da pipeline com:

- carga da base Olist
- engenharia de variaveis de recompra
- ajuste Weibull
- score por cliente
- tabela final de priorizacao
- graficos exportados

## 9. Proximo passo de negocio recomendado

Se a meta for convencer stakeholders, o melhor proximo entregavel e um material curto com:

- problema atual
- o que a Weibull resolve
- exemplo de clientes priorizados
- potencial de ganho
- como virar rotina operacional

## 10. Definicao simples do sucesso

O projeto sera realmente util quando deixar de ser um estudo sobre distribuicao e passar a responder, todos os dias ou semanas:

**quem devemos acionar agora, por qual canal, e quanto dinheiro essa decisao protege ou recupera?**
