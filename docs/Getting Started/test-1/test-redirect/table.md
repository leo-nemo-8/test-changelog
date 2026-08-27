---
title: Table
deprecated: false
hidden: false
metadata:
  robots: index
---
# Especificações dos Campos da API

<Table>
  <thead>
    <tr>
      <th>
        Campo
      </th>

      <th>
        Tamanho
      </th>

      <th>
        Tipo
      </th>

      <th>
        Presença
      </th>

      <th>
        Descrição
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        apiKey
      </td>

      <td>
        Até 50 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Obrigatória
      </td>

      <td>
        Campo composto de números, letras, traços e hífen. Sempre começa por **apk\_**. Exemplo: apk_48040241-OqCWOKczcjutZaFRSfTlVBDpHFXpkdzz. Utilizado para identificar o vendedor
      </td>
    </tr>

    <tr>
      <td>
        order_id
      </td>

      <td>
        Até 64 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Obrigatória
      </td>

      <td>
        **Código de referência da venda**. Define um código para referenciar o pagamento. Útil para vincular o pagamento a um pedido criado pelo sistema do lojista. Recomendamos que seja um código único para cada transação.
      </td>
    </tr>

    <tr>
      <td>
        payer_email
      </td>

      <td>
        Até 255 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Obrigatória
      </td>

      <td>
        e-mail válido do cliente pagador
      </td>
    </tr>

    <tr>
      <td>
        payer_name
      </td>

      <td>
        Até 255 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Obrigatória
      </td>

      <td>
        Nome ou Razão social do cliente pagador
      </td>
    </tr>

    <tr>
      <td>
        payer_cpf_cnpj
      </td>

      <td>
        Até 14 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Obrigatória
      </td>

      <td>
        CPF ou CNPJ do pagador. De preferência apenas os números do cpf ou cnpj
      </td>
    </tr>

    <tr>
      <td>
        payer_phone
      </td>

      <td>
        Até 15 caracteres
      </td>

      <td>
        Numérico
      </td>

      <td>
        Opcional
      </td>

      <td>
        Número de telefone ou celular do cliente. Telefone: (11) 4063-8785. Exemplo: 1140638785. Sempre informar o DDD + Número
      </td>
    </tr>

    <tr>
      <td>
        payer_street
      </td>

      <td>
        Até 255 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Endereço** do cliente pagador. Exemplo: Av Brigadeiro Faria Lima
      </td>
    </tr>

    <tr>
      <td>
        payer_number
      </td>

      <td>
        Até 15 caracteres
      </td>

      <td>
        Numérico
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Número do endereço do** cliente pagador. Exemplo: 1461
      </td>
    </tr>

    <tr>
      <td>
        payer_complement
      </td>

      <td>
        Até 200 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Complemento do endereço** do cliente pagador. Exemplo: Torre Sul 4º Andar
      </td>
    </tr>

    <tr>
      <td>
        payer_district
      </td>

      <td>
        Até 255 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Bairro do cliente** pagador. Exemplo: Jardim Paulistano
      </td>
    </tr>

    <tr>
      <td>
        payer_city
      </td>

      <td>
        Até 100 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Cidade** do cliente pagador. Exemplo: São Paulo
      </td>
    </tr>

    <tr>
      <td>
        payer_state
      </td>

      <td>
        Até 2 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Estado** do cliente pagador. Exemplo: SP. Deve ser representado pela sigla de cada estado
      </td>
    </tr>

    <tr>
      <td>
        payer_zip_code
      </td>

      <td>
        Até 8 caracteres
      </td>

      <td>
        Numérico
      </td>

      <td>
        Opcional
      </td>

      <td>
        **CEP** do cliente pagador. Exemplo: 01452002
      </td>
    </tr>

    <tr>
      <td>
        days_due_date
      </td>

      <td>
        Até 3 caracteres
      </td>

      <td>
        Numérico
      </td>

      <td>
        Obrigatória
      </td>

      <td>
        **Dias corridos até o vencimento**. **Exemplo: 4** O número representa diferença de dias entre a data da requisição e a data de vencimento. A diferença entre as datas: Data requisição: 2017-07-01, Data do vencimento: 2017-07-05, days_due_date: 4. **Esperado de 0 a 400 dias.** Por padrão o valor máximo é de 400 dias, caso necessite emitir boleto com prazo superior a 400 dias, entre em contato através do e-mail [comercial@paghiper.com](mailto:comercial@paghiper.com)
      </td>
    </tr>

    <tr>
      <td>
        type_bank_slip
      </td>

      <td>
        Até 11 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Obrigatória
      </td>

      <td>
        **Formato do boleto bancário**. Valores esperados: **boletoA4** (Boleto do tamanho de uma folha A4) ou **boletoCarne** (Boleto em tamanho carnê, onde é possível imprimir até três boletos por folha A4). Comentário: o formato mais popular é o boletoA4
      </td>
    </tr>

    <tr>
      <td>
        notification_url
      </td>

      <td>
        Até 255 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Opcional
      </td>

      <td>
        **URL de retorno automático de dados**. Endereço da página onde o PagHiper enviará o POST com as informações da transação. Note que, este campo tem prioridade sobre a url que estiver configurada no painel PagHiper. Qualquer alteração de status de uma transação, será está url que iremos notificar através de um post
      </td>
    </tr>

    <tr>
      <td>
        discount_cents
      </td>

      <td>
        Até 15 caracteres
      </td>

      <td>
        Numérico
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Valor total do desconto da compra em centavos**. Exemplo, em um desconto aplicado de R$ 11,58 reais, por exemplo, deve ser informado: 1158 (total de centavos). Se o desconto for aplicado em porcentagem, a sua aplicação deverá realizar o cálculo e nos informar apenas o valor já calculado em centavos.
      </td>
    </tr>

    <tr>
      <td>
        shipping_price_cents
      </td>

      <td>
        Até 15 caracteres
      </td>

      <td>
        Numérico
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Valor total do frete em centavos**. Exemplo: o frete custa R$ 15,99, deve ser informado: 1599 (total em centavos)
      </td>
    </tr>

    <tr>
      <td>
        shipping_methods
      </td>

      <td>
        Até 45 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Método de entrega**. Exemplo: SEDEX, SEDEX10, PAC, TRANSPORTADORA, MOTOBOY, RETIRADA NO LOCAL, etc.
      </td>
    </tr>

    <tr>
      <td>
        partners_id
      </td>

      <td>
        Até 15 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Id do parceiro**. Útil apenas para integração de plataformas parceiras. Na maioria dos casos, esse campo deve ser ignorado.
      </td>
    </tr>

    <tr>
      <td>
        number_ntfiscal
      </td>

      <td>
        Até 15 caracteres
      </td>

      <td>
        Numérico
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Número da nota fiscal**. Se informado **123456**, exibirá o número da nota fiscal no boleto bancário na caixa de descrição da seguinte forma: "Referente a nota fiscal número: 123456"
      </td>
    </tr>

    <tr>
      <td>
        fixed_description
      </td>

      <td>
        Até 5 caracteres
      </td>

      <td>
        Booleano
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Frase fixa**. Frase pré-configurada no painel do PagHiper, esta frase passa por uma pré análise antes de ser exibida nos boletos. Valores esperados: **true** (Exibirá a frase na caixa de descrição do boleto) ou **false** (Nenhuma frase pré-configurada)
      </td>
    </tr>

    <tr>
      <td>
        seller_description
      </td>

      <td>
        Até 85 caracteres
      </td>

      <td>
        Texto
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Frase variável do vendedor**. Texto que irá variar de acordo com cada boleto em específico, podendo colocar informações que remetam ao pedido/serviço adquirido pelo cliente. A frase variável será exibida no corpo do boleto bancário, no campo onde traz informações sobre os prazos de pagamento do boleto. A frase será exibida no seguinte formato: "**Texto do vendedor**: (conteúdo da frase variável aqui)". Obs.: A informação "**Texto do vendedor**" é permanente, não sendo retirada ao acrescentar uma frase variável no boleto. Tamanho máximo de 85 caracteres.
      </td>
    </tr>

    <tr>
      <td>
        late_payment_fine
      </td>

      <td>
        Até 2 caracteres
      </td>

      <td>
        Numérico
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Percentual da multa**. O percentual máximo autorizado é de 2%, de acordo artigo 52, parágrafo primeiro do Código de Defesa do Consumidor, Lei 8.078/90. Exemplo: multa de 2% deve ser enviado o valor: 2. Qualquer valor acima do máximo autorizado será levado em consideração 2% = 2. Observações: A utilização deste parâmetro implica na dilatação no tempo de cancelamento do boleto, passando a ser de 30 dias após o seu vencimento. Utilize o parâmetro "open_after_day_due" para personalizar esse tempo. Aceito apenas números inteiros: 1 e 2
      </td>
    </tr>

    <tr>
      <td>
        per_day_interest
      </td>

      <td>
        - <br />
      </td>

      <td>
        Booleano
      </td>

      <td>
        Opcional
      </td>

      <td>
        **Juros por atraso**. Aplicar 1% de juros máximo ao mês, esse percentual será cobrado proporcionalmente aos dias de atraso. Dividindo 1% por 30 dias = 0,033% por dia de atraso. Valores esperados: **true** (Aplicará o juros de 1% ao mês por atraso) ou **false** (Nenhum juro será aplicado). Observações: A utilização deste parâmetro implica na dilatação no tempo de cancelamento do boleto, passando a ser de 30 dias após o seu vencimento. Utilize o parâmetro "open_after_day_due" para personalizar esse tempo
      </td>
    </tr>

    <tr>
      <td>
        early_payment_discounts<br />\_days
      </td>

      <td>
        Até 2 caracteres
      </td>

      <td>
        Numérico
      </td>

      <td>
        Opcional
      </td>

      <td>
        Número de dias em que o pagamento pode ser realizado com antecedência recebendo o desconto extra. **Exemplo:** O valor do boleto é R$ 100,00 e será concedido um desconto extra caso o pagador realize o pagamento com até 5 dias antes da data do vencimento. Neste caso deve ser enviado o número 5 simbolizando o número máximo de dias de antecedência. Nota: Este campo não pode ser utilizado, se o valor informado ser maior que o valor do campo days_due_date (vencimento). O valor máximo não deve ultrapassar 99 dias do vencimento
      </td>
    </tr>

    <tr>
      <td>
        early_payment_discounts<br />\_cents
      </td>

      <td>
        Até 15 caracteres
      </td>

      <td>
        Numérico
      </td>

      <td>
        Opcional
      </td>

      <td>
        Valor do desconto em centavos que será aplicado caso o pagamento ocorra de forma antecipada. Exemplo: O valor do boleto é R$100,00, porém, caso seja pago com antecedência mínima de 5 dias antes da data do vencimento, será concedido um desconto extra de R$5,00. Neste caso, o valor a ser enviado será o número 500, valor em centavos, que representará o desconto extra pelo pagamento antecipado. Se o desconto extra pelo pagamento antecipado for aplicado em porcentagem, a sua aplicação deverá realizar o cálculo e nos informar apenas o valor já calculado em centavos.
      </td>
    </tr>

    <tr>
      <td>
        open_after_day_due
      </td>

      <td>
        Até 2 caracteres
      </td>

      <td>
        Numérico
      </td>

      <td>
        Opcional
      </td>

      <td>
        Número máximo de dias em que o boleto poderá ser pago **após o vencimento**. (Prática comum para quem opta por cobrar juros e multas). Neste campo será aceito, qualquer número maior ou igual a 2, e menor ou igual a 90. Exemplo: Se optar em receber após o vencimento por até 15 dias, deverá ser enviado o número 15, e a frase será exibida no boleto da seguinte forma: "Não receber após 15 dias do vencimento." Recomendamos o uso deste campo apenas se existir o interesse em permitir que o pagador realize o pagamento fora do prazo de vencimento. É útil para se trabalhar em conjunto com a aplicação de juros e multas.
      </td>
    </tr>
  </tbody>
</Table>

## Detalhes de itens (array)

| Campo       | Tamanho            | Tipo     | Presença    | Descrição                                                                                                                                                                                     |
| ----------- | ------------------ | -------- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Item_id     | Até 64 caracteres  | Texto    | Obrigatória | **Código do item**. Útil para identificar, por exemplo, o código do produto. Caso não deseje utilizar esse campo, enviar o número: 1                                                          |
| description | Até 255 caracteres | Texto    | Obrigatória | **Descrição do item**. Útil para identificar o nome do produto ou serviço.                                                                                                                    |
| quantity    | Até 15 caracteres  | Numérico | Obrigatória | **Quantidade do item**. Define a quantidade de cada item. Utilizado para calcular o valor total da transação. deve ser enviado número inteiro igual ou maior que 1                            |
| price_cents | Até 15 caracteres  | Numérico | Obrigatória | **Valor unitário do item em centavos**. Define o valor unitário de cada item. Exemplo: Determinado item tem o preço definido em R$ 1.901,95, deverá ser informado: 190195 (total em centavos) |
