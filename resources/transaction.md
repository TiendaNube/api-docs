# Transaction

## Endpoints

#### Parameters List

* **store_id:** ID of the store that generated the transaction.
* **order_id:** ID of the order related to the transaction.
* **transaction_id:** Transaction ID.

### POST /{*store_id*}/orders/{*order_id*}/transactions

Create a transaction with *pending* status.

#### Request

E.g.

```json
{
  "provider_id": "1234",
  "amount": {
    "value": "132.95",
    "currency": "ARS"
  },
  "type": "credit_card",
  "status": [
    "pending"
  ],
  "external_id": "5678",
  "external_url": "https://externalurl.com/creditcard",
  "created_at": "2020-01-25T12:30:15.000Z",
  "context": {
    "ip": "192.168.0.25",
    "source": "web_desktop",
    "payment_method": "credit_card"
  }
}
```

#### Response

**201 Created** - the request was successful and a resource was created.

### PUT /{*store_id*}/orders/{*order_id*}/transactions/{*transaction_id*}

Update the status of a transaction. Only the status field can be updated.

#### Request

E.g.

```json
{
  "provider_id": "1234",
  "amount": {
    "value": "132.95",
    "currency": "ARS"
  },
  "type": "credit_card",
  "status": [
    "pending", 
    "authorized", 
    "captured"
  ],
  "external_id": "5678",
  "external_url": "https://externalurl.com/creditcard",
  "created_at": "2020-01-25T12:30:15.000Z",
  "context": {
    "ip": "192.168.0.25",
    "source": "web_desktop",
    "payment_method": "credit_card"
  }
}
```

#### Response

**200 OK** - the request was successful 

### GET /{*store_id*}/orders/{*order_id*}/transactions

List transactions for a given order.

#### Request

{}

#### Response

**200 OK** - the request was successful

E.g.

```json
[
	{
	  "provider_id": "1234",
	  "amount": {
	    "value": "132.95",
	    "currency": "ARS"
	  },
	  "type": "credit_card",
	  "status": [
	    "pending", 
	    "authorized", 
	    "captured"
	  ],
	  "external_id": "5678",
	  "external_url": "https://externalurl.com/creditcard",
	  "created_at": "2020-01-25T12:30:15.000Z",
	  "context": {
	    "ip": "192.168.0.25",
	    "source": "web_desktop",
	    "payment_method": "credit_card"
	  }
	},
	{
	  "provider_id": "1234",
	  "amount": {
	    "value": "500.00",
	    "currency": "ARS"
	  },
	  "type": "ticket",
	  "status": [
	    "pending", 
	    "paid"
	  ],
	  "external_id": "9045",
	  "external_url": "https://externalurl.com/ticket",
	  "created_at": "2020-01-25T12:41:55.000Z",
	  "context": {
	    "ip": "192.168.0.25",
	    "source": "web_desktop",
	    "payment_method": "ticket"
	  }
	}
]	
```

### GET /{*store_id*}/orders/{*order_id*}/transactions/{*transaction_id*}

Get a specific transaction for a given order.

#### Request

{}

#### Response

**200 OK** - the request was successful

E.g.

```json
{
  "provider_id": "1234",
  "amount": {
    "value": "500.00",
    "currency": "ARS"
  },
  "type": "ticket",
  "status": [
    "pending", 
    "paid"
  ],
  "external_id": "9045",
  "external_url": "https://externalurl.com/ticket",
  "created_at": "2020-01-25T12:41:55.000Z",
  "context": {
    "ip": "192.168.0.25",
    "source": "web_desktop",
    "payment_method": "ticket"
  }
}
```

## HTTP Errors List

* **204 No Content** - the request was successful but there is no representation to return (i.e. the response is empty).
* **400 Bad Request** - the request could not be understood or was missing required parameters.
* **401 Unauthorized** - authentication failed or user doesn't have permissions for requested operation.403 Forbidden - access denied.
* **404 Not Found** - resource was not found.
* **405 Method Not Allowed** - requested method is not supported for resource.

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

> _*Note:*_ All URLs must be secure URLs (https).

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

### Transaction Types

* **Credit Card:** Transaction in which the consumer uses a credit card as payment method (E.g. VISA, Mastercard, AMEX).
* **Debit Card:** Transaction in which the consumer uses a debit card as payment method (E.g. VISA Debit, Maestro)
* **Boleto:** Transaction in which the consumer uses a Boleto Bancário as payment method. Boleto is a Brazilian payment method based on cash.
* **Ticket:** Transaction in which the consumer uses a ticket as payment method. This ticket can be paid through a non-bank collection channel (E.g. Rapipago, Pago Fácil, OXXO)
* **Wire Transfer:** Transaction in which the consumer uses a wire transfer as payment method.
* **Cash:** Transaction in which the consumer uses cash as payment method.
* **Wallet:** Transaction in which the consumer uses a wallet as payment method. A wallet is an application that allows you to transfer cryptocurrencies.
* **Refund:** Transaction in which the merchant returns money to the consumer. This refund may involve any payment method.

### Transaction Status

Each type of transaction has a finite series of possible status:

#### Credit Card/Debit Card Transactions

<img src="https://i.imgur.com/pfi1CE5.png" alt="transaction_status_01" height="90"/>

* **Pending:** The consumer's submission and payment have both been received; payment has been sent out for processing, but the payment gateway has not yet confirmed that the payment has gone through.
* **Authorized:** The consumer's credit or debit card payment has been processed and accepted.
* **Rejected:** The consumer's payment was not accepted when it was processed by the bank or credit card company.
* **Captured:** The consumer's checking, savings or other bank account payment has been processed and accepted.
* **Voided:** The consumer's payment was cancelled by the merchant before it settles through a consumer's debit or credit card account.
* **In Dispute:** The customer questions the validity of the transaction that was registered to his account and decides to cancelled it through the issuer.
* **Chargeback:** The money in the merchant's account is held due to a dispute initiated by the consumer.

#### Cash/Boleto/Wire Transfer/Ticket/Wallet Transactions

<img src="https://i.imgur.com/N1kvoMN.png" alt="transaction_status_01" height="85"/>

* **Pending:** The consumer's submission and payment have both been received; payment has been sent out for processing, but the payment gateway has not yet confirmed that the payment has gone through.
* **Paid:** The consumer's payment has been processed and accepted.
* **Voided:** The consumer's payment was cancelled by the merchant before the consumer paid it.

#### Refund Transaction

The series of possible status for this transaction is the same as for Cash / Ticket / Wire Transfer / Ticket / Wallet, but in this case, the money goes from the merchant to the consumer.
