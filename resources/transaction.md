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

Each movement of money is modeled through a Transaction object, which can be of different types (for example, credit card, cash, wire transfer, refund, etc.). Since not all Transactions are atomic, each type can include a finite series of possible states.

A Payment Provider can create the amount of Transactions that it needs for a certain order, and can update their status as they change over time.

All types of Transactions have the same attributes, but may differ in the values that their status field takes.

| Field                 | Description                                                                                               |
| --------------------- | --------------------------------------------------------------------------------------------------------- |
| provider_id           | ID of the payment provider that processed this transaction.                                               |
| amount                | Money object containing the value of this transaction. See [Money](#Money).                                                     |
| type                  | One of "credit_card", "debit_card", "boleto", "ticket", "wire_transfer", "cash", "wallet" or "refund". See [Transaction Types](#Transaction Types).    |
| status                | Array containing the series of status of this transaction. See [Transaction Status](#Transaction Status).                                                                   |
| external_id           | [Optional] ID used by the payment provider.                                                               |
| external_url          | [Optional] URL for the payment provider's website with the details on this transaction for the merchant.  |
| created_at            | [Optional] Creation date for this transaction. Defaults to current time.                                 |
| context               | [Optional] Object containing context information that could be useful for fraud analysis. See [Payment Context](#Payment Context).           |

Some transaction types have specific *extra* fields.

| Field                 | Description                                                                                                                 |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| external_resource_url | [Optional - Only for boleto and ticket] Url of the boleto or ticket which can be shown to the consumer to resume the payment. |
| original_transaction  | [Optional - Only for refund] Id of the transaction that is being refunded.                                                    |
| payment_method_type   | [Optional - Only for refund] Payment method type used for refund (credit_card, cash, etc).                                    |

### Money
| Field    | Description                                                 |
| ---------| ----------------------------------------------------------- |
| value    | Value as a string. E.g. "49.99"                             |
| currency | ISO 4217 code for the currency, such as ARS, BRL, USD, etc. |

### Payent Context
| Field    | Description                                                         |
| ---------| ------------------------------------------------------------------- |
| ip              | [Optional] IP of the device that initiated this transaction. |
| source         | [Optional] One of `web_desktop`, `web_mobile` or `pos`.       |
| payment_method | [Optional] Payment method used for this transaction.          |

### Transaction Status

Each type of transaction has a finite series of possible status:

#### Credit Card/Debit Card Transactions

<img src="https://i.imgur.com/pfi1CE5.png" alt="transaction_status_01" height="100"/>

#### Cash/Boleto/Wire Transfer/Ticket/Wallet Transactions

<img src="https://i.imgur.com/N1kvoMN.png" alt="transaction_status_01" height="90"/>

#### Refund Transaction

The series of possible status for this type of transaction is the same as for Cash / Ticket / Wire Transfer / Ticket / Wallet, but in this case, the money goes from the merchant to the consumer.

### Transaction Types

* **Credit Card:**
* **Debit Card:**
* **Boleto:**
* **Ticket:**
* **Wire Transfer:**
* **Cash:**
* **Wallet:**
* **Refund:**
