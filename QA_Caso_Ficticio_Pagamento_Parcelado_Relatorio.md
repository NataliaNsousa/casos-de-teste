# Caso fictício de QA — Pagamento parcelado ausente no relatório de recebimentos

## Contexto e investigação

Ao revisar o relatório de recebimentos, encontrei registros de pagamentos por Pix, cartão de crédito, cartão de débito e dinheiro. Quando comparei o relatório com as vendas realizadas, percebi que havia compras parceladas no cartão de crédito sem identificação de parcelamento no relatório.

Para confirmar a divergência, simulei uma compra parcelada no ambiente de testes e consultei o relatório do mesmo período. O valor da compra aumentou o total da categoria de cartão de crédito, mas o campo de parcelados ficou em branco. Com isso, eu conseguia ver o valor no total do cartão, mas não conseguia distinguir o que havia sido pago à vista e o que havia sido parcelado.

## Registro do bug

- **Resultado observado.** O valor da compra entra no total de cartão de crédito e o campo de parcelados fica em branco.
- **Resultado esperado.** O relatório identifica a compra como parcelada e apresenta o valor correspondente no campo de parcelados. Se a categoria de crédito mostrar um total consolidado, o valor deve ser contabilizado uma única vez no total geral.
- **Impacto.** A falta de identificação dificulta a conferência das vendas e a conciliação dos recebimentos por modalidade.

No cenário fictício, registrei o bug no ClickUp com os passos de reprodução e a comparação entre o resultado observado e o esperado. Encaminhei o registro à equipe de desenvolvimento para investigação e correção. A revalidação ficou pendente.

## Passos para reproduzir

1. Em ambiente de testes, consulte o relatório de recebimentos e anote os totais de crédito e parcelados.
2. Simule uma compra aprovada de R$ 120,00 no cartão de crédito em 3 parcelas de R$ 40,00, sem juros.
3. Gere novamente o relatório com os mesmos filtros, garantindo que o período inclua a compra.
4. Compare os totais antes e depois da compra e confira o campo de parcelados.

## Casos de teste para revalidação e regressão

Os valores são fictícios. Compare a variação dos totais antes e depois de cada cenário.

| ID | Cenário | Dados de teste | Resultado esperado |
| --- | --- | --- | --- |
| CT-01 | Crédito parcelado | R$ 120,00 em 3× de R$ 40,00 | O relatório identifica R$ 120,00 como parcelado. O total geral aumenta R$ 120,00. |
| CT-02 | Crédito à vista | R$ 80,00 em 1× | O relatório identifica R$ 80,00 como crédito à vista. O valor de parcelados não aumenta. |
| CT-03 | Crédito à vista e parcelado no mesmo período | R$ 80,00 em 1× e R$ 120,00 em 3× | O relatório permite distinguir R$ 80,00 à vista e R$ 120,00 parcelados. O total geral aumenta R$ 200,00. |
| CT-04 | Regressão em Pix, débito e dinheiro | Uma compra de R$ 50,00 em cada modalidade | Cada modalidade aumenta R$ 50,00 e o total geral aumenta R$ 150,00. |

**Critério de aceite.** O relatório distingue crédito à vista de crédito parcelado e os totais refletem as compras aprovadas no período sem dupla contagem.
