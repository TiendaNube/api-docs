# Transaction

## Endpoints

### POST /{*store_id*}/orders/{*id*}/transactions

Create a transaction with *pending* status.

#### Request

#### Response

##### HTTP Errors List

### PUT /{*store_id*}/orders/{*id*}/transactions/{*id*}

Update the status of a transaction. Only the status field can be updated.

#### Request

#### Response

##### HTTP Errors List

### GET /{*store_id*}/orders/{*id*}/transactions

List transactions for a given order.

#### Request

#### Response

##### HTTP Errors List

### GET /{*store_id*}/orders/{*id*}/transactions/{*id*}

Get a specific transaction for a given order.

#### Request

#### Response

##### HTTP Errors List

## Resource Objects Description

### Transaction

Each movement of money is modeled through a Transaction object, which can be of different types (E.g. credit card, cash, wire transfer, refund, etc.). Since not all Transactions are atomic, each type can include a finite series of possible states.

A Payment Provider can create the number of transactions it needs for a given order, and can update their status as they change over time.

All transactions types have the same attributes, but may differ in the values that their *status* field can take.

| Field                 | Description                                                                                               |
| --------------------- | --------------------------------------------------------------------------------------------------------- |
| provider_id           | ID of the payment provider that processed this transaction.                                               |
| amount                | Money object containing the value of this transaction. See [Money](#Money).                                                     |
| type                  | One of `credit_card`, `debit_card`, `boleto`, `ticket`, `wire_transfer`, `cash`, `wallet` or `refund`. See [Transaction Types](#Transaction-Types).    |
| status                | Array containing the series of status of this transaction. See [Transaction Status](#Transaction-Status).                                                                   |
| external_id           | [Optional] ID used by the payment provider.                                                               |
| external_url          | [Optional] URL for the payment provider's website with the details on this transaction for the merchant.  |
| created_at            | [Optional] Creation date for this transaction. Defaults to current time.                                 |
| context               | [Optional] Object containing context information that could be useful for fraud analysis. See [Payment Context](#Payment-Context).           |

Some transaction types have specific *extra* fields.

| Field                 | Description                                                                                                                 |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| external_resource_url | [Optional - Only for `boleto` and `ticket`] URL of the boleto or ticket which can be shown to the consumer to resume the payment. |
| original_transaction  | [Optional - Only for `refund`] ID of the transaction that is being refunded.                                                    |
| payment_method_type   | [Optional - Only for `refund`] Payment method type used for refund. See [Payment Methods](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/payment_provider.md#Payment-Methods).                                      |

### Money
| Field    | Description                                                 |
| ---------| ----------------------------------------------------------- |
| value    | Value as a string. E.g. "49.99"                             |
| currency | ISO 4217 code for the currency, such as ARS, BRL, USD, etc. |

### Payment Context
| Field    | Description                                                         |
| ---------| ------------------------------------------------------------------- |
| ip              | [Optional] IP of the device that initiated this transaction. |
| source         | [Optional] One of `web_desktop`, `web_mobile` or `pos`.       |
| payment_method | [Optional] Payment method used for this transaction. See [Payment Methods](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/payment_provider.md#Payment-Methods).         |

### Transaction Status

Each type of transaction has a finite series of possible status:

#### Credit Card/Debit Card Transactions

<img src="https://i.imgur.com/pfi1CE5.png" alt="transaction_status_01" height="90"/>

* **Pending:**
* **Authorized:**
* **Rejected:**
* **Captured:**
* **Voided:**
* **In Dispute:**
* **Chargeback:**

#### Cash/Boleto/Wire Transfer/Ticket/Wallet Transactions

<img src="https://i.imgur.com/N1kvoMN.png" alt="transaction_status_01" height="85"/>

* **Pending:**
* **Paid:**
* **Voided:**

#### Refund Transaction

The series of possible status for this transaction is the same as for Cash / Ticket / Wire Transfer / Ticket / Wallet, but in this case, the money goes from the merchant to the consumer.

### Transaction Types

* **Credit Card:** Transaction in which the consumer uses a credit card as payment method (E.g. VISA, Mastercard, AMEX).
* **Debit Card:** Transaction in which the consumer uses a debit card as payment method (E.g. VISA Debit, Maestro)
* **Boleto:** Transaction in which the consumer uses a Boleto Bancário as payment method. Boleto is a Brazilian payment method based on cash.
* **Ticket:** Transaction in which the consumer uses a ticket as payment method. This ticket can be paid through a non-bank collection channel (E.g. Rapipago, Pago Fácil, OXXO)
* **Wire Transfer:** Transaction in which the consumer uses a wire transfer as payment method.
* **Cash:** Transaction in which the consumer uses cash as payment method.
* **Wallet:** Transaction in which the consumer uses a wallet as payment method. A wallet is an application that allows you to transfer cryptocurrencies.
* **Refund:** Transaction in which the merchant returns money to the consumer. This refund may involve any payment method.
