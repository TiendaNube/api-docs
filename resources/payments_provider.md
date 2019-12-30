# Payments Provider

##### Index

- 1. [Endpoints](#link)
  - 1.1 [POST /_{store_id}_/payment_providers](#link)
    - [Request](#link)
    - [Response](#link)
  - 1.2 [GET /_{store_id}_/payment_providers](#link)
    - [Request](#link)
    - [Response](#link)
  - 1.3 [GET /_{store_id}_/payment_providers/_{payment_provider_id}_](#link)
    - [Request](#link)
    - [Response](#link)
  - 1.4 [GET /_{store_id}_/payment_providers/_{payment_provider_id}_](#link)
    - [Request](#link)
    - [Response](#link)
  - 1.5 [PUT /_{store_id}_/payment_providers/_{payment_provider_id}_](#link)
    - [Request](#link)
    - [Response](#link)
  - 1.6 [DELETE /_{store_id}_/payment_providers/_{payment_provider_id}_](#link)
    - [Request](#link)
    - [Response](#link)
- 2. [Resource Objects Description](#link)
  - 2.1 [Payments Provider](#link)
    - 2.1.1 [Payments Provider Object Properties](#link)
  - 2.2 [Logos](#link)
    - 2.2.1 [`logo_urls` field in `Payments Provider` Object Properties](#link)
  - 2.3 [Currencies](#link)
    - 2.3.1 [Currency Codes](#link)
  - 2.4 [Payment Methods](#link)
    - 2.4.1 [`supported_methods` field in `Payments Provider` Object Properties](#link)
  - 2.5 [Rates](#link)
    - 2.5.1 [`rates` field in `Payments Provider` Object Properties](#link)
  - 2.6 [Money](#link)
    - 2.6.1 Money Object Properties
  - 2.7 [Installments](#link)
    - 2.7.1 [Installments Object Properties](#link)
      - [Specification](#link)
        - [Simple `specification`](#link)
        - [Detailed `specification`](#link)
- 3. [Appendix](@link)
  - 3.1 [Supported Payment Methods by Payment Method Type](#link)

## 1. Endpoints

### 1.1 POST /_{store_id}_/payment_providers

#### Request

#### Response

##### HTTP Errors List

### 1.2 GET /_{store_id}_/payment_providers

#### Request

#### Response

##### HTTP Errors List

### 1.3 GET /_{store_id}_/payment_providers/_{payment_provider_id}_

#### Request

#### Response

##### HTTP Errors List

### 1.4 GET /_{store_id}_/payment_providers/_{payment_provider_id}_

#### Request

#### Response

##### HTTP Errors List

### 1.5 PUT /_{store_id}_/payment_providers/_{payment_provider_id}_

#### Request

#### Response

##### HTTP Errors List

### 1.6 DELETE /_{store_id}_/payment_providers/_{payment_provider_id}_

#### Request

#### Response

##### HTTP Errors List


## 2. Resource Objects Description

### 2.1 Payments Provider

A Payments Provider, shorter name for Payments Services Provider, represents any entity which provides all the necessary resources and infrastructure for Merchants and Consumers to exectute `Transactions` between them. This entities could be any of the following:

- Subadquirente
- Gateway
- Adquirente

Payments companies have many different and sometimes complex features which add value to the purchase experience, mainly providing multiple payments options and simpler checkout flows. They also provide Merchants with tools to make better management of their transactions as well as their incomes.

In our Platform, a Payment Provider is created for a specific `Store`.

#### 2.1.1 Payments Provider Object Properties

| Field                  | Type                      | Description                                                                                                          |
|:-----------------------|:--------------------------|:---------------------------------------------------------------------------------------------------------------------|
| `id`                   | String                    | Unique identifier of the `Payments Provider` object.                                                                 |
| `name`                 | String                    | Display name which Merchants and Consumers will see.                                                                 |
| `description`          | String                    | Short paragraph which provides Merchants with a short description of the `Payments Provider`.                        |
| `logo_urls`            | Object                    | `Version: URL` `key:value` pair for each version of the [Logos](#link) for the frontend.                             |
| `configuration_url`    | String                    | [Optional] `Payments Provider` Configuration UI URL.                                                                 |
| `support_url`          | String                    | [Optional] `Payments Provider` Support URL.                                                                          |
| `checkout_js`          | String&#124;Array(String) | URL of each JS file to be included in the [checkout](#link) frontend.                                                |
| `supported_currencies` | String&#124;Array(String) | [ISO.4217](https://en.wikipedia.org/wiki/ISO_4217) [Currency Codes](#link) supported by the `Payments Provider`.     |
| `supported_methods`    | Object                    | `PaymentMethodType: [PaymentMethodCodes]` `key:array` pair for each [Payment Methods](#link) available to Consumers. |
| `rates`                | Object                    | [Rates](#link) that build up the service cost to the Merchant.                                                       |
| `installments`         | Object                    | [Installments](#link) available to Consumers.                                                                        |

> _*Note:*_ All URLs must be Secure URLs.

### 2.2 Logos
At the moment, our Platformm requires the following versions of the `Payments Provider` logo.

| Key     | URL content description                                                                                |
|:--------|:-------------------------------------------------------------------------------------------------------|
| 400x120 | PNG file with Transparent Brackground. Dimensions not greater than 400px x 120px. _(As of 01/01/2019)_ |
| 160x100 | PNG file with White Background. Dimensions must be 160px x 100px. _(As of 01/01/2019)_                 |

#### 2.2.1 `logo_urls` field in `Payments Provider` Object Properties
At the moment, this object should look like the following example:

```json
{
  "400x120": "https://myapp.mypayments.com/example_logo1.png",
  "160x100": "https://myapp.mypayments.com/example_logo2.png"
}
```

### 2.3 Currencies
Every amount value needs to be complemented by a currency.

#### 2.3.1 Currency Codes

`Currency Codes` must be specified according to [ISO.4217](https://en.wikipedia.org/wiki/ISO_4217). A few examples of these are:

- `ARS`: Argentinean Peso
- `BRL`: Brazilian Real
- `CLP`: Chilean Peso
- `COP`: Colombian Peso
- `MXN`: Mexican Peso

### 2.4 Payment Methods
There are many companies providing different `Payment Methods` of different types. The currently supported `Payment Methods Types` by our Platform are:

- `credit_card`
- `debit_card`
- `boleto`
- `ticket`
- `wire_transfer`
- `cash`
- `wallet`

`Payment Providers` integrate our platform with different [`Payment Methods`](#link) of different `Payment Method Types`. Depending on the type of `Payment Provider` (Subadquirente, Gateway, Adquirente), they may provide one or many Payment Methods for each `Payment Methods Type`. Check the list of [Supported Payment Methods by Payment Method Type](#link).

#### 2.4.1 `supported_methods` field in `Payments Provider` Object Properties
An example of the value of this field would be:

```json
{
  "credit_card": ["visa", "mastercard", "amex"],
  "debit_card": ["visa_debit", "maestro"],
  "boleto": ["boleto"],
  "ticket": ["rapipago", "rapipago", "oxxo"],
}
```

### 2.5 Rates
`Payment Providers` may charge Merchants with different rates per payment transaction depending on the `Payment Method Type` and the time the Merchant chooses to withdraw the money. Hence, for each `Payment Method Type` there would be a list of rates depending on the withdrawal time specified in days.

| Field                    | Type             | Description                                                            |
|:-------------------------|:-----------------|:-----------------------------------------------------------------------|
| `percent_fee`            | Number as String | Percentage fee charged per payment. Example: `percent_fee: "3.82"`.    |
| `flat_fee`               | Money            | [Optional] [Money](#link) object for a flat fee charged per payment.   |
| `plus_tax`               | Boolean          | [Optional] Indicates whether VAT will be added to the specified rates. |
| `days_to_withdraw_money` | Number           | Days since transaction until de Merchant can withdraw the money.       |

_*Note:*_ `Number as String` would be a Number in String format for better decimal precision handling.

#### 2.5.1 `rates` field in `Payments Provider` Object Properties
An example of the value of this field would be:

```json
{
  "credit_card": [
    {"percent_fee": "2.99", "flat_fee": {"value": "0.39", "currency": "BRL"}, "days_to_withdraw_money": 30},
    {"percent_fee": "3.99", "flat_fee": {"value": "0.39", "currency": "BRL"}, "days_to_withdraw_money": 14}
  ],
  "boleto": [
    {"flat_fee": {"value": "2.99", "currency": "BRL"}, "days_to_withdraw_money": 2}
  ]
}
```

### 2.6 Money
Sums of money are represented by, both, an amount and a currency.

#### 2.6.1 Money Object Properties

| Field      | Type             | Description                                                                                     |
|:-----------|:-----------------|:------------------------------------------------------------------------------------------------|
| `value`    | Number as String | Value as a string for better decimal precision handling. Example: `value: "49.99"`              |
| `currency` | String           | [`Currency Code`](#link) string in [ISO.4217](https://en.wikipedia.org/wiki/ISO_4217) standard. |

### 2.7 Installments

Most `Payment Providers` provide different installment based payments options.

#### 2.7.1 Installments Object Properties

| Field                   | Type              | Description                                                                                                              |
|:------------------------|:------------------|:-------------------------------------------------------------------------------------------------------------------------|
| `type`                  | String            | Either `simple` or `detailed`, depending on the business rules that define them.                                         |
| `min_installment_value` | String            | [Optional] `CurrencyCode: Amount` `key:Number` pair of the minimum value an installment can be for that `Currency Code`. |
| `specification`         | Object&#124;Array | Check [`Specification`](#link) below for a description of the possible contents of this Object.                          |

> _*Note:*_ An example of `min_installment_value` would be `BRL: 5`. For instance, if the total amount to be payed is `50 BRL`, then the Customer can choose to make this payment in up to 10 installments because the value of each of them would be `50 / 10 = 5`. However, the can Customer won't be able to choose to spread the payment into 12 installments because `50 / 12 = 4.17` and `4.17 < 5`.

##### Specification
The two possible values are [`simple`](#link) and [`detailed`](#link), depending on the business rules that apply to each installment. When different banks or cards provide different interest rates or other similar properties, installments descriptions may need a some extra information.

###### Simple Specification
The `simple` `specification` is used when installments don't have any business rules that apply to them. In this case, the only difference between the different options are the number of installments and the interest rate, which is applied to the total amount. In this case, a list of `NumberOfInstallments:interest_rate` `key:value` pairs is all that needs to be specified. Example:

```json
{
  "type": "simple",
  "min_installment_value": {
    "BRL": 5
  },
  "specification": {
    "1": "0.0",
    "2": "0.0",
    "3": "0.0",
    "4": "0.065",
    "5": "0.075",
    "6": "0.085",
    "7": "0.095",
    "8": "0.105",
    "9": "0.115",
    "10": "0.125",
    "11": "0.135",
    "12": "0.145"
  }
}
```

> _*Note:*_ `interest_rates` are percentages expressed in fractions of 1 in `String` format for better decimal precision handling. For instance, an interest rate of `6.5%` would be expressed as `6.5 / 100 = 0.065`, which stringified would be "0.065".

###### Detailed Specification
The `detailed` `specification` allows the use of more specific business rules. This `specification` is intended to support future business rules as well, so expect this feature to support more fields in the future. Also, feel free to discuss more functionalities with Nuvemshop's Platform Team.

_Detailed Specification Object Properties:_

| Field           | Type               | Description                                                           |
|:----------------|:-------------------|:----------------------------------------------------------------------|
| `installments`  | Number as String   | Number of installments.                                               |
| `interest_rate` | String             | Rates to be applied to the total amount for this installments option. |
| `applies_to`    | String&#124;Arrays | List of `Payment Methods` for which this installments option applies. |


```json
{
  "type": "detailed",
  "specification": [
    {
      "installments": "6",
      "interest_rate": "0.0",
      "applies_to": ["hsbc"]
    },
    {
      "installments": "3",
      "interest_rate": "0.0",
      "applies_to": ["bbva"]
    },
    {
      "installments": "12",
      "interest_rate": "0.61",
      "applies_to": ["galicia", "icbc", "santander", "banco_provincia", ...]
    }
  ]
}
```

## 3. Appendix

### 3.1 Supported Payment Methods by Payment Method Type
The following is the list of Payment Methods currently supported by our platform.

#### Credit Card
- `visa`
- `mastercard`
- `amex`
- `diners`
- `nativa`
- `argencard`
- `cabal`
- `cordial`
- `cordobesa`
- `falabella`
- `hsbc_access_now`
- `tarjeta_naranja`
- `tarjeta_saenz`
- `tarjeta_shopping`
- `tarjeta_walmart`
- `provencred`
- `nativa`
- `rebanking`
- `aura`
- `elo`
- `hiper`
- `hipercard`
- `discover`
- `oipaggo`
- `magna`

#### Debit Card
- `visa_debit`
- `maestro`
- `cabal_debit`

#### Boleto
- `boleto`

#### Ticket
- `rapipago`
- `pagofacil`
- `servipag`
- `efecty`
- `viabaloto`
- `7eleven`
- `oxxo`

#### Bank Transfer
- `banelco`
- `link`
- `provincianet`
- `pse`

#### Banks
- `hsbc`
- `galicia`
- `icbc`
- `itau`
- `bbva`
- `macro`
- `santander`
- `scotiabank`
- `banco_chaco`
- `banco_chubut`
- `banco_ciudad`
- `banco_coinag`
- `banco_columbia`
- `banco_comafi`
- `banco_comercio`
- `banco_entre_rios`
- `banco_hipotecario`
- `banco_industrial`
- `banco_la_pampa`
- `banco_municipal`
- `banco_nacion`
- `banco_patagonia`
- `banco_provincia`
- `banco_sanjuan`
- `banco_santa_cruz`
- `banco_santa_fe`
- `banco_tierra_de_fuego`
- `banco_tucuman`
- `bica`
- `cencosud`
- `citi`
- `supervielle`
- `banco_do_brasil`
- `banrisul`
- `bradesco`
- `caixa`
- `itau`
- `banamex`