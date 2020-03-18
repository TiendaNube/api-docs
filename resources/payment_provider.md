Payment Provider
================

A Payment Provider, shorter name for Payments Services Provider, represents any entity which provides all the necessary resources and infrastructure for merchants and consumers to exectute [Transactions](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/transaction.md) between them. This entities could be any of the following:

- **Subadquirente**
- **Gateway**
- **Adquirente**

Payments companies have many different and sometimes complex features which add value to the purchase experience, mainly providing multiple payments options and simpler checkout flows. They also provide merchants with tools to make better management of their Transactions as well as their incomes.

In our platform, a Payment Provider is created for a specific `store`.

Properties
----------

| Field                       | Type          | Description                                                  |
| :-------------------------- | :------------ | :----------------------------------------------------------- |
| `name`                      | String        | Display name which merchants and consumers will see.         |
| `description`               | String        | Short paragraph which provides merchants with a description of the Payment Provider. |
| `logo_urls`                 | Object        | Object containing `key:value` pair for each version of the logos for the frontend. Only supports HTTPS URLs. See [Logos](#Logos). |
| `checkout_js_url`           | String        | HTTPS URL of the JS file to be included in the checkout frontend. See [Checkout JS](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/checkout_js.md). |
| `supported_currencies`      | Array(String) | ISO.4217 currency codes supported by the Payment Provider. See [Currency Codes](#Currency-Codes). |
| `supported_payment_methods` | Array(Object) | List of available payment methods for each payment method type. See [Payment Methods](#Payment-Methods). |
| `rates`                     | Array(Object) | List of rates definitions for merchants by payment method type. See [Rates](#Rates). |
| `configuration_url`         | String        | [Optional] Payment Provider configuration UI HTTPS URL.      |
| `support_url`               | String        | [Optional] Payment Provider support HTTPS URL.               |
| `id`                        | String        | [Informational] Unique identifier of the Payment Provider object. |
| `store_id`                  | Integer       | [Informational] Id of the store to which the Payment Provider belongs. |
| `enabled`                   | Boolean       | [Informational] Indicates whether Payment Provider is enabled in the store. |

> ***Note:*** All URLs must be secure URLs (https).

> ***Note:*** Informational properties will only appear in GET responses, which means that should not be included in POST/PUT requests.

### Logos

At the moment, our platform requires two versions of the Payment Provider logo. Each image must be sent as a `key:value` pair, being the key the dimension of the image and the value, the HTTPS URL of its content.

| Dimension | URL Content Description                                      |
| :-------- | :----------------------------------------------------------- |
| `400x120` | PNG file with Transparent Brackground. Dimensions not greater than 400px (width) x 120px (height). *(As of 01/01/2019)*. |
| `160x100` | PNG file with White Background. Dimensions must be 160px (width) x 100px (height). *(As of 01/01/2019)*. |

E.g.

```json
{
  "400x120": "https://myapp.mypayments.com/logo1.png",
  "160x100": "https://myapp.mypayments.com/logo2.png"
}
```

### Currency Codes

Every amount value needs to be complemented by a currency. Supported currency codes must be specified according to [ISO 4217](https://www.currency-iso.org/en/home/tables/table-a1.html). A few examples of these are:

- `ARS`: Argentinean Peso
- `BRL`: Brazilian Real
- `CLP`: Chilean Peso
- `COP`: Colombian Peso
- `MXN`: Mexican Peso
- `USD`: American Dollar

### Payment Methods Types

There are many companies providing payment methods of different types. Currently, our platform supports the following payment methods types:

- `credit_card`
- `debit_card`
- `boleto`
- `ticket`
- `wire_transfer`
- `cash`
- `wallet`

Depending on the kind of Payment Provider (Subadquirente, Gateway, Adquirente), they may integrate to our platform one or many payment pethods for each payment method type.

### Payment Methods

Depending on the kind of Payment Provider (Subadquirente, Gateway, Adquirente), they may integrate to our platform one or many payment pethods for each payment method type. If applicable, the installments data supported by the payment method type is detailed here.

| Field                 | Type          | Description                                                  |
| :-------------------- | :------------ | :----------------------------------------------------------- |
| `payment_method_type` | String        | One of the available [Payment Method Types](#Payment-Method-Types). |
| `payment_methods`     | Array(String) | The list of supported payments methods by the given payment method type. See [Supported Payment Methods by Payment Method Type](#Supported-Payment-Methods-by-Payment Method-Type). |
| `installments`        | Object        | [Optional] Object containing the installments available to consumers. See [Installments](#Installments). |

E.g.

```json
[
    {
      "payment_method_type": "credit_card",
      "payment_methods": ["visa", "mastercard", "amex"],
      "installments": {
        "min_installment_value": [
          {
            "currency": "ARS",
            "value": "100.00"
          }
        ],
        "specification": [
          {
            "installments": 1,
            "interest_rate": "0.00",
            "applies_to": ["bbva", "itau", "santander", "galicia"]
          },
          {
            "installments": 3,
            "interest_rate": "0.00",
            "applies_to": ["bbva", "itau"]
          },
          {
            "installments": 6,
            "interest_rate": "0.45",
            "applies_to": ["santander", "galicia"]
          },
        ]
      }
    },
    {
      "payment_method_type": "debit_card",
      "payment_methods": ["visa_debit", "maestro"]
    }
  ]
```

### Rates

Payment Providers may charge merchants with different rates per Transaction depending on the payment method type and the time the merchant chooses to withdraw the money. Hence, for each payment method type there would be a list of rates depending on the withdrawal time specified in days.

| Field                 | Type          | Description                                                  |
| :-------------------- | :------------ | :----------------------------------------------------------- |
| `payment_method_type` | String        | Payment method type to which the rates definition refer. See [Payment Method Types](#Payment-Method-Types). |
| `rates_definition`    | Array(Object) | Object containing the rates details. See [Rates Definition](#Rates-Definition) section bellow. |

#### Rates Definition

| Field                    | Type    | Description                                                  |
| :----------------------- | :------ | :----------------------------------------------------------- |
| `percent_fee`            | Number  | Percentage fee charged per payment.                          |
| `days_to_withdraw_money` | Integer | Days since Transaction creation until de merchant can withdraw the money. |
| `flat_fee`               | Object  | [Optional] Object containing the flat fee charged per payment. See [Money](#Money). |
| `plus_tax`               | Boolean | [Optional] Indicates whether VAT will be added to the specified rates. |

E.g.

```json
[
    {
        "payment_method_type": "credit_card",
        "rates_definition": [
            {
                "percent_fee": "30.35",
                "flat_fee": {
                    "currency": "ARS",
                    "value": "1000.00"
                },
                "plus_tax": true,
                "days_to_withdraw_money": 10
            }
        ]
    }
]
```

### Money

Sums of money will be represented by a value and its respective currency.

| Field      | Type   | Description                                                 |
| :--------- | :----- | :---------------------------------------------------------- |
| `value`    | String | Amount of money as a string. E.g `"49.99"`                  |
| `currency` | String | ISO 4217 code for the currency, such as ARS, BRL, USD, etc. |

> ***Note:*** Decimal numbers will be represented as string format for better decimal precision handling. It must contain two decimal places and use a point as decimal separator.

### Installments

Most Payment Providers provide different installment based payments options.

| Field                   | Type          | Description                                                  |
| :---------------------- | :------------ | :----------------------------------------------------------- |
| `specification`         | Array(Object) | Check [Specification](#Specification) section below for a description of this field. |
| `min_installment_value` | Array(Object) | [Optional] List of minimum installment values accepted by each currency. See [Money](#Money) for items format. |

> ***Note:*** An example for `min_installment_value` would be `"currency": "BRL` and `"amount": "5"` . For instance, if the total amount to be payed is `50 BRL`, then the consumer can choose to make the payment in up to 10 installments because the value of each of them would be `50 / 10 = 5`. However, the consumer won't be able to choose to spread the payment into 12 installments because `50 / 12 = 4.17` and `4.17 < 5`.

##### Specification

The specification field allows the use of specific business rules. This specification is intended to support future business rules as well, so expect this feature to support more fields in the future. Therefore, feel free to discuss more functionalities with *Tiendanube's Platform Team*.

| Field           | Type          | Description                                                  |
| :-------------- | :------------ | :----------------------------------------------------------- |
| `installments`  | Integer       | Number of installments.                                      |
| `interest_rate` | String        | Rate to be applied to the total amount for this installments option. |
| `applies_to`    | Array(String) | List of [banks](#bank) to which this installments option applies. |

E.g.

```json
{
  "specification": [
    {
      "installments": 3,
      "interest_rate": "0.00",
      "applies_to": ["bbva"]
    },
    {
      "installments": 6,
      "interest_rate": "0.32",
      "applies_to": ["hsbc"]
    },
    {
      "installments": 12,
      "interest_rate": "0.64",
      "applies_to": ["galicia", "icbc", "santander", "banco_provincia"]
    }
  ],
  "min_installment_value": [
      {
        "currency": "ARS",
        "value": "100.00"
      }
    ]
}
```

> ***Note:*** Interest rates are percentages expressed in fractions of 1 in `String` format for better decimal precision handling. For instance, an interest rate of `6.5%` would be expressed as `6.5 / 100 = 0.065`, which stringified would be "0.065".



Endpoints
---------

### POST /payment_providers

Create a Payment Provider for a given store.

#### Request

[Payment Provider Object](#Properties)

E.g.

```json
{
    "name": "My Payments",
    "description": "Some short description for merchants.",
    "logo_urls": {
        "400x120": "https://mypayments.com/logo1.png",
        "160x100": "https://mypayments.com/logo2.png"
    },
    "configuration_url": "https://mypayments.com/configuration",
    "support_url": "https://mypayments.com/support",
    "checkout_js_url": "https://mypayments.com/checkout.min.js",
    "supported_currencies": [
        "ARS"
    ],
    "supported_payment_methods": [
        {
            "payment_method_type": "credit_card",
            "payment_methods": [
                "visa",
                "mastercard"
            ],
            "installments": {
                "min_installment_value": [
                    {
                        "currency": "ARS",
                        "value": "100.00"
                    }
                ],
                "specification": [
                    {
                        "installments": 1,
                        "interest_rate": "0.00",
                        "applies_to": [
                            "bbva",
                            "itau",
                            "santander",
                            "galicia"
                        ]
                    },
                    {
                        "installments": 3,
                        "interest_rate": "0.00",
                        "applies_to": [
                            "bbva",
                            "itau"
                        ]
                    },
                    {
                        "installments": 6,
                        "interest_rate": "0.45",
                        "applies_to": [
                            "santander",
                            "galicia"
                        ]
                    }
                ]
            }
        }
    ],
    "rates": [
        {
            "payment_method_type": "credit_card",
            "rates_definition": [
                {
                    "percent_fee": "30.50",
                    "flat_fee": {
                        "value": "1000.00",
                        "currency": "ARS"
                    },
                    "plus_tax": true,
                    "days_to_withdraw_money": 10
                }
            ]
        }
    ]
}
```

#### Response

`HTTP/1.1 201 Created`

E.g.

```json
{
  "id": "815905d6-3518-479d-8ed6-5e0e4e6f305d"
}
```

Unique identifier of the created Payment Provider. 

### PUT /payment_providers/{*payment_provider_id*}

Update a Payment Provider for a given store. This is especially useful to update the installments specs.

#### Request

[Payment Provider Object](#Properties)

#### Response

`HTTP/1.1 204 No Content` - the request was successful but there is no representation to return (i.e. the response is empty).

### GET /payment_providers

Get all Payment Providers for a given store.

#### Request

```json
{}
```

#### Response

`HTTP/1.1 200 OK`

Array of [Payment Provider Objects](#Properties)

### GET /payment_providers/{*payment_provider_id*}

Get a specific Payment Provider for a given store.

#### Request

```json
{}
```

#### Response

`HTTP/1.1 200 OK`

[Payment Provider Object](#Properties)

### DELETE /payment_providers/{*payment_provider_id*}

Delete a Payment Provider for a given store.

#### Request

```json
{}
```

#### Response

`HTTP/1.1 204 No Content` - the request was successful but there is no representation to return (i.e. the response is empty).

## HTTP Errors List

* **400 Bad Request** - the request could not be understood or was missing required parameters.
* **401 Unauthorized** - authentication failed or user doesn't have permissions for requested operation.
* **403 Forbidden** - access denied.
* **404 Not Found** - resource was not found.
* **405 Method Not Allowed** - requested method is not supported for resource.

Appendix
--------

### Supported Payment Methods by Payment Method Type

The following is the list of payment methods currently supported by our platform.

#### Credit Card

- `visa`
- `mastercard`
- `amex`
- `diners`
- `aura`
- `elo`
- `hiper`
- `hipercard`
- `oipaggo`
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
- `magna`
- `discover`

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

#### Bank

- `banco_do_brasil`
- `banrisul`
- `bradesco`
- `caixa`
- `itau`
- `hsbc`
- `galicia`
- `icbc`
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
- `banamex`
