# Exercícios — Pandas + Matplotlib
**Base de dados:** `vendas_tech.csv` (100.100 linhas) + `gerentes_lojas.xlsx`  
**Período:** 2023–2024 | **Produtos:** 8 | **Lojas:** 7 (com sujeira) | **Clientes únicos:** ~28.896

---

## BLOCO 1 — Limpeza e dtypes

**Ex 1. Normalização de Loja com rastreamento**  
A coluna `Loja` tem nulos, espaços extras e capitalização inconsistente. Normalize tudo para title case sem espaços. Crie um relatório mostrando quantos registros foram alterados em cada categoria de problema: era nulo / tinha espaço / estava em caixa errada. Isso exige rastrear o estado antes e depois.

**Ex 2. Engenharia de datas**  
A coluna `Data` chegou como string. Converta para datetime e crie três colunas derivadas: `Ano`, `Mes`, `DiaSemana` (como nome, em português). Depois identifique: qual dia da semana concentra mais receita? Há diferença de ticket médio entre dias úteis e fim de semana?

---

## BLOCO 2 — GroupBy avançado

**Ex 3. Métricas por produto em um único groupby**  
Calcule, por produto, as seguintes métricas em um único `groupby`: total de pedidos, receita total, receita média por pedido, desvio padrão da receita, percentual da receita total do dataset. O resultado deve ser um DataFrame formatado, com colunas nomeadas corretamente — sem renomear linha por linha.

**Ex 4. Rank por loja sem loop**  
Usando `groupby` + `transform`, crie uma coluna `Rank_Receita_Loja` que rankeia cada pedido dentro da sua loja por receita (1 = maior). Depois filtra os top 3 pedidos de cada loja. Proibido usar loop.

**Ex 5. Clientes multi-loja**  
Identifique clientes que compraram em mais de uma loja diferente. Para esses clientes, calcule a receita total e o número de lojas distintas visitadas. Ordena pelos mais "viajantes".

---

## BLOCO 3 — Merge e join cirúrgico

**Ex 6. Performance de gerentes vs meta**  
Carrega o `gerentes_lojas.xlsx`. Faz o merge com vendas (Loja normalizada). Para cada gerente, calcula receita total, número de meses ativos no dataset e receita média mensal. Compara com a `Meta_Mensal` — quem está acima da média mensal da meta? Quem nunca atingiu a meta em nenhum mês?

**Ex 7. Diagnóstico de merge com indicator**  
Usando o mesmo merge, aplica `merge` com `indicator=True` para identificar os três grupos: lojas só em vendas, lojas só no xlsx, e lojas nos dois. Apresenta a contagem e os nomes de cada grupo.

---

## BLOCO 4 — Window functions e análise temporal

**Ex 8. Série temporal com rolling e pct_change**  
Agrupa por mês e calcula a receita mensal total. Cria três colunas novas: variação percentual mês a mês (`pct_change`), média móvel de 3 meses (`rolling(3).mean()`), e uma coluna booleana `Acima_Media_Movel` indicando se aquele mês superou a média móvel. Apresenta como tabela formatada no terminal — sem matplotlib.

**Ex 9. Intervalo entre compras e segmentação de clientes**  
Para cada cliente que comprou mais de uma vez, calcule o intervalo médio em dias entre compras (`diff()` em datas, agrupado por cliente). Classifica os clientes em três faixas: `Frequente` (< 30 dias), `Regular` (30–90 dias), `Eventual` (> 90 dias). Qual faixa gera mais receita total?

---

## BLOCO 5 — Performance e otimização de memória

**Ex 10. Leitura otimizada vs leitura bruta**  
Leia o CSV inteiro duas vezes: uma sem nenhuma otimização, outra usando `usecols`, `dtype=` declarados explicitamente e `parse_dates`. Usa `df.memory_usage(deep=True)` para medir o consumo em cada caso. Quanto de memória você economizou? Apresenta a diferença em MB.

---

## BLOCO 6 — Matplotlib: fundação bem feita

**Ex 11. Receita mensal como linha (pandas .plot())**  
Plota a receita mensal total como linha usando `.plot()` do pandas. Obrigatório: título, label nos eixos, formato de moeda no eixo Y (R$ 1.200.000), rotação nos ticks do eixo X, grid suave e tamanho de figura adequado. Salva como PNG. Sem `plt.show()` — o exercício entrega arquivo.

**Ex 12. Bar chart horizontal por produto com anotações**  
Cria um bar chart horizontal com a receita total por produto, ordenado do maior pro menor. Cada barra com cor diferente. Adiciona o valor exato no final de cada barra com `ax.text()`. Label, título, sem bordas desnecessárias (`ax.spines`).

---

## BLOCO 7 — Subplots e layouts compostos

**Ex 13. Dashboard 2×2 por produto**  
Cria uma figura com `plt.subplots(2, 2)` contendo quatro gráficos sobre produtos: (1) receita total em barras, (2) quantidade total vendida em barras, (3) ticket médio em barras horizontais, (4) distribuição de Qtd por produto em boxplot. Título geral com `fig.suptitle()`. Todos os subplots com label nos eixos.

**Ex 14. Layout assimétrico com GridSpec**  
Usando `GridSpec`, cria um layout assimétrico: um gráfico grande de linha (receita mensal) ocupando a linha de cima inteira, e dois gráficos menores embaixo lado a lado (receita por loja e receita por dia da semana). `GridSpec` obrigatório — não vale `subplots()`.

**Ex 15. Três métricas empilhadas com eixo X compartilhado**  
Cria um dashboard com `plt.subplots(3, 1, figsize=(14, 18))` mostrando a evolução mensal de três métricas: receita total, número de pedidos e ticket médio. Os três compartilham o eixo X (`sharex=True`). Cada gráfico tem uma linha de média geral tracejada como referência.

---

## BLOCO 8 — Estilização avançada

**Ex 16. Anotações, setas e faixas temporais**  
Pega o gráfico de receita mensal e adiciona: anotação com seta (`ax.annotate()`) apontando pro mês de pico, anotação apontando pro mês de menor receita, e uma faixa sombreada (`ax.axvspan()`) destacando o segundo semestre de cada ano. Usa `plt.style.use('seaborn-v0_8-whitegrid')`.

**Ex 17. Scatter com tamanho e cor por variável**  
Cria um scatter com Preco_Unitario no eixo X, Qtd no eixo Y, tamanho dos pontos proporcional à receita do pedido (`s=receita / fator`) e cor por produto (cada produto uma cor, com legenda). Usa `ax.legend()` com handles manuais.

**Ex 18. Heatmap sem seaborn — matplotlib puro**  
Cria um heatmap mostrando a receita total por Loja (linhas) × Mês (colunas) usando `ax.imshow()`. Adiciona os valores dentro de cada célula com `ax.text()`. Colorbar lateral. Eixos rotacionados. Proibido usar seaborn.

---

## BLOCO 9 — Integração pandas + matplotlib pesado

**Ex 19. Subplots dinâmicos por loja com tendência linear**  
Para cada loja normalizada, plota a série temporal de receita mensal em um subplot separado. O número de subplots é calculado em runtime — você não sabe quantas lojas existem antes de rodar. Cada subplot tem o nome da loja como título e uma linha de tendência linear (`np.polyfit`) sobreposta em vermelho tracejado.

**Ex 20. Barras empilhadas: composição de receita por produto ao longo do tempo**  
Cria um gráfico de barras empilhadas mostrando a composição da receita por produto em cada mês — cada barra é um mês, dividida em segmentos por produto. Usa `.unstack()` no groupby e `.plot(kind='bar', stacked=True)`. Legenda fora do gráfico (`bbox_to_anchor`), cores customizadas por produto, título e eixos formatados. Salva com `dpi=150`.

---

*20 exercícios | Pandas + Matplotlib | Gerado por Vic*
