# Payment Provider

## Endpoints

#### Parameters List

* **store_id:** Store ID linked to the Payment Provider.
* **payment_provider_id:** Payment Provider ID.

### POST /{*store_id*}/payment_providers

Create a new Payment Provider associated with a store.

#### Request

[Payment Provider Object](#Payment-Provider)

E.g.

```json
{
  "id": "1234",
  "name": "My Payments",
  "description": "Some short description for merchants.",
  "logo_urls": {
    "400x120": "https://myapp.mypayments.com/logo1.png",
    "160x100": "https://myapp.mypayments.com/logo2.png"
  },
  "checkout_js": {...},
  "supported_currencies": ["ARS", "BRL"],
  "supported_methods": {
    "credit_card": ["visa", "mastercard", "amex"],
    "debit_card": ["visa_debit", "maestro"],
    "boleto": ["boleto"],
    "ticket": ["rapipago", "rapipago", "oxxo"],
  },
  "rates": {
    "credit_card": [
      {"percent_fee": "2.99", "flat_fee": {"value": "0.39", "currency": "BRL"}, "days_to_withdraw_money": 30},
      {"percent_fee": "3.99", "flat_fee": {"value": "0.39", "currency": "BRL"}, "days_to_withdraw_money": 14}
    ],
    "boleto": [
      {"flat_fee": {"value": "2.99", "currency": "BRL"}, "days_to_withdraw_money": 2}
    ]
  },
  "installments": {
    "type": "simple",
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
    },
      "min_installment_value": {
      "BRL": 5
    }
  },
  "configuration_url": "https://myapp.mypayments.com/configuration",
  "support_url": "https://myapp.mypayments.com/support"
}
```

#### Response

**201 Created** - the request was successful and a resource was created.

**Body:**

URL to redirect the merchant to the Tiendanube Admin Panel.

```json
{
  "return_url": "https://mystore.mitiendanube.com/admin/..."
}
```

### PUT /{*store_id*}/payment_providers/{*payment_provider_id*}

Update a Payment Provider. This is especially useful to update the installments specs.

#### Request

[Payment Provider Object](#Payment-Provider)

#### Response

**200 OK** - the request was successful and the resource was updated.

### GET /{*store_id*}/payment_providers

Get all Payment Providers for a given store.

#### Request

{}

#### Response

**200 OK** - the request was successful.

**Body:**

[Payment Provider Object](#Payment-Provider)

### GET /{*store_id*}/payment_providers/{*payment_provider_id*}

Get a specific Payment Provider for a given store.

#### Request

{}

#### Response

**200 OK** - the request was successful.

**Body:**

[Payment Provider Object](#Payment-Provider)

### DELETE /{*store_id*}/payment_providers/{*payment_provider_id*}

Delete a Payment Provider.

#### Request

{}

#### Response

**200 OK** - the request was successful and the resource was deleted.

## HTTP Errors List

* **204 No Content** - the request was successful but there is no representation to return (i.e. the response is empty).
* **400 Bad Request** - the request could not be understood or was missing required parameters.
* **401 Unauthorized** - authentication failed or user doesn't have permissions for requested operation.403 Forbidden - access denied.
* **404 Not Found** - resource was not found.
* **405 Method Not Allowed** - requested method is not supported for resource.

## Resource Objects Description

### Payment Provider

A Payment Provider, shorter name for Payments Services Provider, represents any entity which provides all the necessary resources and infrastructure for merchants and consumers to exectute [Transactions](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/transaction.md) between them. This entities could be any of the following:

- **Subadquirente**
- **Gateway**
- **Adquirente**

Payments companies have many different and sometimes complex features which add value to the purchase experience, mainly providing multiple payments options and simpler checkout flows. They also provide merchants with tools to make better management of their transactions as well as their incomes.

In our platform, a Payment Provider is created for a specific `store`.

#### Payment Provider Object Properties

| Field                  | Type          | Description                                                                                                          |
|:-----------------------|:--------------|:---------------------------------------------------------------------------------------------------------------------|
| `id`                   | String        | Unique identifier of the Payment Provider object.                                                                  |
| `name`                 | String        | Display name which merchants and consumers will see.                                                                 |
| `description`          | String        | Short paragraph which provides merchants with a description of the Payment Provider.                         |
| `logo_urls`            | Object        | Object containing `key:value` pair for each version of the logos for the frontend. See [Logos](#Logos).                             |
| `checkout_js`          | Array(String) | URL of each JS file to be included in the checkout frontend. See [Checkout](#Checkout).                                                |
| `supported_currencies` | Array(String) | ISO.4217 currency codes supported by the Payment Provider. See [Currency Codes](#Currency-Codes).      |
| `supported_methods`    | Object        | Object containing `key:array` pair for each payment method available to consumers. See [Payment Methods](#Payment-Methods). |
| `rates`                | Object        | Object containing the rates that build up the service cost to the merchant. See [Rates](#Rates).                                                  |
| `installments`         | Object        | Object containing the installments available to consumers. See [Installments](#Installments).                                                                        |
| `configuration_url`    | String        | [Optional] Payment Provider configuration UI URL.                                                                  |
| `support_url`          | String        | [Optional] Payment Provider support URL.                                                                           |

> ***Note:*** All URLs must be secure URLs (https).

### Logos

At the moment, our platform requires two versions of the Payment Provider logo. Each image must be sent as a `key:value` pair, being the key the dimension of the image and the value, the URL of its content.

| Dimension     | URL Content Description                                                                                |
|:--------|:-------------------------------------------------------------------------------------------------------|
| 400x120 | PNG file with Transparent Brackground. Dimensions not greater than 400px (width) x 120px (height). *(As of 01/01/2019)*. |
| 160x100 | PNG file with White Background. Dimensions must be 160px (width) x 100px (height). *(As of 01/01/2019)*.                 |

E.g.

```json
{
  "400x120": "https://myapp.mypayments.com/logo1.png",
  "160x100": "https://myapp.mypayments.com/logo2.png"
}
```

### Currency Codes

Every amount value needs to be complemented by a currency.

Currency codes must be specified according to [ISO 4217](https://www.currency-iso.org/en/home/tables/table-a1.html). A few examples of these are:

- `ARS`: Argentinean Peso
- `BRL`: Brazilian Real
- `CLP`: Chilean Peso
- `COP`: Colombian Peso
- `MXN`: Mexican Peso
- `USD`: American Dollar

### Payment Methods

There are many companies providing payment methods of different types. The currently supported payment methods types by our platform are:

- `credit_card`
- `debit_card`
- `boleto`
- `ticket`
- `wire_transfer`
- `cash`
- `wallet`

Depending on the kind of Payment Provider (Subadquirente, Gateway, Adquirente), they may integrate to our platform one or many payment pethods for each payment method type. Check the list of [Supported Payment Methods by Payment Method Type](#Supported-Payment-Methods-by-Payment Method-Type).

E.g.

```json
{
  "credit_card": ["visa", "mastercard", "amex"],
  "debit_card": ["visa_debit", "maestro"],
  "boleto": ["boleto"],
  "ticket": ["rapipago", "pagofacil", "oxxo"],
}
```

### Rates

Payment Providers may charge merchants with different rates per payment transaction depending on the payment method type and the time the merchant chooses to withdraw the money. Hence, for each payment method type there would be a list of rates depending on the withdrawal time specified in days.

| Field                    | Type             | Description                                                            |
|:-------------------------|:-----------------|:-----------------------------------------------------------------------|
| `percent_fee`            | String | Percentage fee charged per payment. E.g. `"3.82"`.    |
| `days_to_withdraw_money` | Number           | Days since transaction until de merchant can withdraw the money.       |
| `flat_fee`               | Money            | [Optional] Object containing the flat fee charged per payment. See [Money](#Money).  |
| `plus_tax`               | Boolean          | [Optional] Indicates whether VAT will be added to the specified rates. |

> ***Note:*** Decimal numbers will be represented as string format for better decimal precision handling.

E.g.

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

### Money

Sums of money will be represented by an amount and its respective currency.

| Field      | Type             | Description                                                                                     |
|:-----------|:-----------------|:------------------------------------------------------------------------------------------------|
| `value`    | String | Value as a string. E.g `"49.99"`              |
| `currency` | String           | ISO 4217 code for the currency, such as ARS, BRL, USD, etc. |

### Installments

Most Payment Providers provide different installment based payments options.

| Field                   | Type   | Description                                                                                                              |
|:------------------------|:-------|:-------------------------------------------------------------------------------------------------------------------------|
| `type`                  | String | Either `simple` or `detailed`, depending on the business rules that define it.                                         |
| `specification`         | Object | Check [Specification](#Specification) section below for a description of the possible contents of this Object.                          |
| `min_installment_value` | String | [Optional] `key:value` pair where the key is a currency code and the value is the minimum amount an installment can have for that currency. |

> ***Note:*** An example of `min_installment_value` would be `BRL: 5`. For instance, if the total amount to be payed is `50 BRL`, then the consumer can choose to make the payment in up to 10 installments because the value of each of them would be `50 / 10 = 5`. However, the consumer won't be able to choose to spread the payment into 12 installments because `50 / 12 = 4.17` and `4.17 < 5`.

##### Specification

The two possible values are [`simple`](#Simple-Specification) and [`detailed`](#Detailed-Specification), depending on the business rules that apply to each installment. When different banks or cards provide different interest rates or other similar properties, installments descriptions may need some extra information.

###### Simple Specification

The *simple specification* is used when installments don't have any business rules that apply to them. In this case, the only difference between the different options are the number of installments and the interest rate, which is applied to the total amount. In this case, a list of  `key:value` pairs is all that needs to be specified. The key must be a number of installments and the value must indicate the interest rate for it.

E.g.

```json
{
  "type": "simple",
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
  },
    "min_installment_value": {
    "BRL": 5
  }
}
```

> ***Note:*** Interest rates are percentages expressed in fractions of 1 in `String` format for better decimal precision handling. For instance, an interest rate of `6.5%` would be expressed as `6.5 / 100 = 0.065`, which stringified would be "0.065".

###### Detailed Specification

The *detailed specification* allows the use of more specific business rules. This specification is intended to support future business rules as well, so expect this feature to support more fields in the future. Also, feel free to discuss more functionalities with *Nuvemshop's Platform Team*.

| Field           | Type             | Description                                                           |
|:----------------|:-----------------|:----------------------------------------------------------------------|
| `installments`  | String | Number of installments in string format.                                               |
| `interest_rate` | String           | Rates to be applied to the total amount for this installments option. |
| `applies_to`    | Array(String)            | List of [payment methods](#Payment-Methods) for which this installments option applies. |

E.g.

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

## Appendix

### Supported Payment Methods by Payment Method Type

The following is the list of payment methods currently supported by our platform.

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

#### Boleto Bancário
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
