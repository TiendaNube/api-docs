Transaction
===========

Each movement of money is modeled through a Transaction object, which can be of different types (e.g. credit card, cash, wire transfer, refund, etc.). Since not all Transactions are atomic, each type has a Finite-state Machine (FSM) that defines its status.

A [Payment Provider](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/payment_provider.md) can create the number of Transactions it needs for a given order, and can update their status as they change over time.

Properties
---------

All Transactions types have the same attributes, but may differ in the values that their *status* field can take.

| Field          | Type   | Description                                                  |
| :------------- | :----- | :----------------------------------------------------------- |
| `provider_id`  | String | ID of the Payment Provider that processed this Transaction.  |
| `amount`       | Object | Object containing the value of this Transaction. See [Money](#Money). |
| `type`         | String | One of `credit_card`, `debit_card`, `boleto`, `ticket`, `wire_transfer`, `cash`, `wallet` or `refund`. See [Transaction Types](#Transaction-Types). |
| `status`       | String | The state of the FSM in which the Transaction is. See [Transaction Status](#Transaction-Status). |
| `external_id`  | String | [Optional] ID used by the Payment Provider.                  |
| `external_url` | String | [Optional] URL for the Payment Provider's website with the details of this Transaction for the merchant. |
| `created_at`   | Date   | [Optional] Creation date of this Transaction. Defaults to current time. |
| `context`      | Object | [Optional] Object containing context information that could be useful for fraud analysis. See [Payment Context](#Payment-Context). |

> ***Note:*** All URLs must be secure URLs (https).

Some Transaction types have specific *extra* fields.

| Field                   | Type    | Description                                                  |
| :---------------------- | :------ | :----------------------------------------------------------- |
| `installments`          | Integer | [Optional - Only for `credit_card`] Number of instalments.   |
| `original_transaction`  | String  | [Optional - Only for `refund`] ID of the Transaction that is being refunded. |
| `payment_method_type`   | String  | [Optional - Only for `refund`] Payment method type used for refund. See [Payment Methods](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/payment_provider.md#Payment-Methods). |
| `external_resource_url` | String  | [Optional - Only for `boleto` and `ticket`] URL of the boleto or ticket which can be shown to the consumer to resume the payment. |

### Money

| Field      | Type   | Description                                                 |
| ---------- | ------ | ----------------------------------------------------------- |
| `value`    | String | Value as a string. E.g. `"49.99"`                           |
| `currency` | String | ISO 4217 code for the currency, such as ARS, BRL, USD, etc. |

> ***Note:*** Decimal numbers will be represented as string format for better decimal precision handling. It must contain two decimal places and use a point as decimal separator.

### Payment Context

| Field            | Type   | Description                                                  |
| ---------------- | ------ | ------------------------------------------------------------ |
| `ip`             | String | [Optional] IP of the device that initiated this Transaction. |
| `source`         | String | [Optional] One of `web_desktop`, `web_mobile` or `pos`.      |
| `payment_method` | String | [Optional] Payment method used for this Transaction. See [Payment Methods](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/payment_provider.md#Payment-Methods). |

### Transaction Types

* `credit_card`: Transaction in which the consumer uses a credit card as payment method (E.g. VISA, Mastercard, AMEX).
* `debit_card`: Transaction in which the consumer uses a debit card as payment method (E.g. VISA Debit, Maestro).
* `boleto`: Transaction in which the consumer uses a Boleto Bancário as payment method. Boleto is a Brazilian payment method based on cash.
* `ticket`: Transaction in which the consumer uses a ticket as payment method. This ticket can be paid through a non-bank collection channel (E.g. Rapipago, Pago Fácil, OXXO).
* `wire_transfer`: Transaction in which the consumer uses a wire transfer as payment method.
* `cash`: Transaction in which the consumer uses cash as payment method.
* `wallet`: Transaction in which the consumer uses a wallet as payment method. A wallet is an application that allows you to transfer cryptocurrencies.
* `refund`: Transaction in which the merchant returns money to the consumer. A refund may involve any payment method.

### Transaction Status

Each type of Transaction has a Finite-state Machine (FSM) that defines its status:

#### Credit Card/Debit Card Transactions

<img src="https://i.imgur.com/pfi1CE5.png" alt="transaction_status_01" height="90"/>

* `pending`: The consumer's submission and payment have both been received; payment has been sent out for processing, but the payment gateway has not yet confirmed that the payment has gone through.
* `authorized`: The consumer's credit or debit card payment has been processed and accepted.
* `rejected`: The consumer's payment was not accepted when it was processed by the bank or credit card company.
* `captured`: The consumer's checking, savings or other bank account payment has been processed and accepted.
* `voided`: The consumer's payment was cancelled by the merchant before it settles through a consumer's debit or credit card account.
* `in_dispute`: The customer questions the validity of the Transaction that was registered to his account and decides to cancelled it through the issuer.
* `chargeback`: The money in the merchant's account is held due to a dispute initiated by the consumer.

#### Cash/Boleto/Wire Transfer/Ticket/Wallet Transactions

<img src="https://i.imgur.com/vephWFb.png" alt="transaction_status_01" height="85"/>

* `pending`: The consumer's submission and payment have both been received; payment has been sent out for processing, but the payment gateway has not yet confirmed that the payment has gone through.
* `confirmed`: The consumer's payment has been processed and accepted.
* `voided`: The consumer's payment was cancelled by the merchant before the consumer paid it.

#### Refund Transaction

The FSM for this Transaction is the same as for `cash` / `boleto` / `wire_transfer` / `ticket` / `wallet` types, but in this case, the money goes from the merchant to the consumer.


Endpoints
---------

### POST /orders/{*order_id*}/transactions

Create a Transaction for a given order.

#### Request

[Transaction Object](#Transaction)

E.g.

```json
{
  "provider_id": "815905d6-3518-479d-8ed6-5e0e4e6f305d",
  "amount": {
    "value": "132.95",
    "currency": "ARS"
  },
  "type": "credit_card",
  "installments": 3,
  "status": "pending",
  "external_id": "1234",
  "external_url": "https://mypayments.com/creditcard",
  "created_at": "2020-01-25T12:30:15.000Z",
  "context": {
    "ip": "192.168.0.25",
    "source": "web_desktop",
    "payment_method": "bbva"
  }
}
```

#### Response

`HTTP/1.1 201 Created`

E.g.

```json
{
  "id": "776a2a49-42ca-4e75-992d-15eaf89a2a2c"
}
```

Unique identifier of the created Transaction. 



### PUT /orders/{*order_id*}/transactions/{*transaction_id*}

Update the status of a Transaction. This is the only field that can be updated and must respect the [FSM](#Transaction-Status) of the transaction.

#### Request

E.g.

```json
{
  "status": "authorized"
}
```

#### Response

`HTTP/1.1 204 No Content` - the request was successful but there is no representation to return (i.e. the response is empty).

### GET /orders/{*order_id*}/transactions

List all Transactions of a given order.

#### Request

{}

#### Response

`HTTP/1.1 200 OK`

Array of [Transaction Objects](#Transaction)

### GET /orders/{*order_id*}/transactions/{*transaction_id*}

Get a specific Transaction of a given order.

#### Request

{}

#### Response

`HTTP/1.1 200 OK`

[Transaction Object](#Transaction)

## HTTP Errors List

* **400 Bad Request** - the request could not be understood or was missing required parameters.
* **401 Unauthorized** - authentication failed or user doesn't have permissions for requested operation.
* **403 Forbidden** - access denied.
* **404 Not Found** - resource was not found.
* **405 Method Not Allowed** - requested method is not supported for resource.
