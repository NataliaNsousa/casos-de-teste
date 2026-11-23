# Caso fictício de QA — Erro no cálculo de valores de venda

## Contexto e impacto

Neste cenário fictício, percebi que algumas vendas apresentavam divergências no total calculado. Como o problema se repetia, comecei a investigar quais produtos apareciam nas vendas afetadas.

## Investigação

1. Listei os produtos das vendas afetadas e comparei seus preços com os de vendas que haviam funcionado normalmente.
2. Identifiquei um padrão nos casos analisados: os produtos afetados tinham preços com finais em **,98** ou **,99**, como **R$ 10,98** e **R$ 10,99**.
3. Levei o padrão à equipe de desenvolvimento, junto com os valores esperados e os valores exibidos pelo sistema.
4. Analisei o problema com a equipe de desenvolvimento e identificamos que a divergência vinha do tratamento das casas decimais no cálculo do total da venda.

## Correção e revalidação

Depois que a equipe de desenvolvimento corrigiu o cálculo dos valores monetários, revalidei as vendas afetadas e confirmei que os totais correspondiam aos valores esperados.

## Casos de teste

**Pré-condições.** Produtos cadastrados com os preços indicados. Iniciar cada caso com um carrinho vazio e sem descontos, taxas ou frete.

**Passos comuns.** Adicionar os produtos e as quantidades indicados, conferir o subtotal e o total no carrinho e finalizar a venda no ambiente de testes. Conferir o valor no resumo da venda.

| ID | Cenário | Dados de teste | Resultado esperado |
| --- | --- | --- | --- |
| CT-01 | Revalidar preço terminado em ,98 | 1 unidade de R$ 10,98 | O total no carrinho e no resumo é R$ 10,98. |
| CT-02 | Revalidar preço terminado em ,99 | 1 unidade de R$ 10,99 | O total no carrinho e no resumo é R$ 10,99. |
| CT-03 | Somar produtos com centavos diferentes | 1 unidade de R$ 10,98 e 1 de R$ 10,99 | O total no carrinho e no resumo é R$ 21,97. |
| CT-04 | Multiplicar o preço pela quantidade | 3 unidades de R$ 10,99 | O total no carrinho e no resumo é R$ 32,97. |
| CT-05 | Validar a soma que resulta em valor inteiro | 1 unidade de R$ 10,98 e 1 de R$ 2,02 | O total no carrinho e no resumo é R$ 13,00. |
| CT-06 | Verificar regressão com outros centavos | 1 unidade de R$ 10,00 e 1 de R$ 5,50 | O total no carrinho e no resumo é R$ 15,50. |

**Critério de aceite.** Em cada cenário, o total deve corresponder à soma dos preços unitários multiplicados pelas quantidades e permanecer igual do carrinho ao resumo da venda, com duas casas decimais.

*Os preços e os resultados esperados da tabela são exemplos fictícios para execução dos testes.*
