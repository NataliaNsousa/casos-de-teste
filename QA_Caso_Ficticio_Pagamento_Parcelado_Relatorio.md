# Caso fictício de QA — Pagamento parcelado ausente no relatório de recebimentos

## Contexto e investigação

Ao revisar o relatório de recebimentos, encontrei registros de pagamentos por Pix, cartão de crédito, cartão de débito e dinheiro. Quando comparei o relatório com as vendas realizadas, percebi que havia compras parceladas no cartão de crédito sem identificação de parcelamento no relatório.

Para confirmar a divergência, simulei uma compra parcelada no ambiente de testes e consultei o relatório do mesmo período. Vi que o valor da compra aumentou o total de cartão de crédito e que o campo de parcelados continuou em branco. Ao conferir os valores, não consegui distinguir as compras à vista das parceladas.

## Registro do bug

- **O que observei.** Encontrei o valor da compra no total de cartão de crédito e vi que o campo de parcelados ficou em branco.
- **O que eu esperava encontrar.** Esperava ver a compra identificada como parcelada e seu valor no campo correspondente. Se o relatório exibisse um total consolidado de crédito, eu esperava encontrar o valor contabilizado uma única vez no total geral.
- **Impacto.** Tive dificuldade para conferir as vendas e conciliar os recebimentos por modalidade.

No cenário fictício, registrei o bug no ClickUp com os passos de reprodução e a comparação entre o resultado observado e o esperado. Encaminhei o registro à equipe de desenvolvimento para investigação e correção. Deixei a revalidação pendente até a correção.

## Como reproduzi a falha

1. No ambiente de testes, consultei o relatório de recebimentos e anotei os totais de crédito e parcelados.
2. Simulei uma compra aprovada de R$ 120,00 no cartão de crédito em 3 parcelas de R$ 40,00, sem juros.
3. Gerei novamente o relatório com os mesmos filtros e conferi se o período incluía a compra.
4. Comparei os totais antes e depois da compra e conferi o campo de parcelados.

## Casos de teste para revalidação e regressão

Usei valores fictícios para montar os cenários abaixo. Na revalidação, vou comparar a variação dos totais antes e depois de cada teste.

| ID | Cenário | Dados de teste | O que espero verificar |
| --- | --- | --- | --- |
| CT-01 | Crédito parcelado | R$ 120,00 em 3× de R$ 40,00 | Espero ver R$ 120,00 como parcelado e um aumento de R$ 120,00 no total geral. |
| CT-02 | Crédito à vista | R$ 80,00 em 1× | Espero ver R$ 80,00 como crédito à vista e o valor de parcelados permanecer igual. |
| CT-03 | Crédito à vista e parcelado no mesmo período | R$ 80,00 em 1× e R$ 120,00 em 3× | Espero distinguir R$ 80,00 à vista de R$ 120,00 parcelados e verificar um aumento de R$ 200,00 no total geral. |
| CT-04 | Regressão em Pix, débito e dinheiro | Uma compra de R$ 50,00 em cada modalidade | Espero verificar um aumento de R$ 50,00 em cada modalidade e de R$ 150,00 no total geral. |

**Critério de aceite.** Vou considerar a correção validada quando conseguir distinguir o crédito à vista do parcelado e conferir que cada compra aprovada no período foi contabilizada uma única vez no total geral.
