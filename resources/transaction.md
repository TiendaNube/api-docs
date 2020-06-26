Transaction
===========

Each movement of money is modeled through a `Transaction` object, which can be of different types (e.g. credit card, debit card, boleto, wire transfer, etc.). Each `Transaction` type has a Finite State Machine (FSM) that defines its current status. The `TransactionEvent` object represents transitions in the `Transaction`'s FSM.

A [Payment Provider](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/payment_provider.md) can create the number of Transactions it needs for a given order and update their status as they change over time.

Properties
---------

All `Transaction` types have the same attributes, but may generate different kinds of  *events* and contain some *info* fields specific to their type.

| Field                 | Type          | Description                                                  |
| :-------------------- | :------------ | :----------------------------------------------------------- |
| `id`                  | String        | [Read-only] Unique identifier of the Transaction object.     |
| `app_id`              | String        | [Read-only] ID of the application to which the Transaction belongs. |
| `payment_provider_id` | String        | ID of the [Payment Provider](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/payment_provider.md) that processed this Transaction. |
| `payment_method`      | Object        | Object containing the payment method used in this Transaction. See [Payment Method](#Payment-Method). |
| `info`                | Object        | Object containing specific info related to this Transaction. See [Transaction Info](#Transaction-Info). |
| `status`              | Object        | [Read-only] The state of the FSM in which the Transaction is. See [Transaction Status](#Transaction-Status). |
| `events`              | Array(Object) | [Read-only] List of fulfillment events related to this Transaction. See [Transaction Events](#Transaction-Events). |
| `captured_amount`     | Object        | [Read-only] Object containing the captured amount of this Transaction. See [Money](#Money). |
| `refunded_amount`     | Object        | [Read-only] Object containing the refunded amount of this Transaction. See [Money](#Money). |
| `failure_code`        | String        | [Read-only] If the transaction failed, this field is used to indicate the code related to the failure cause. See [Transaction Failure Codes](#Transaction-Failure-Codes). |

> ***Note:*** Read-only properties will only appear in our responses, which means that should not be part of the requests.

### Payment Method

| Field  | Type   | Description                                                  |
| ------ | ------ | ------------------------------------------------------------ |
| `type` | String | One of the available [Payment Method Types](#Payment-Method-Types). |
| `id`   | String | ID of the payment method used for this Transaction. See [Supported Payment Methods by Payment Method Type](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/payment_provider.md#Supported-Payment-Methods-by-Payment-Method-Type). |

### Payment Method Types

* `bank_debit`: Transaction in which the consumer uses bank debit as payment method.
* `boleto`: Transaction in which the consumer uses a Boleto Bancário as payment method. Boleto is a Brazilian payment method based on cash.
* `cash`: Transaction in which the consumer uses cash as payment method.
* `credit_card`: Transaction in which the consumer uses a credit card as payment method (E.g. VISA, Mastercard, AMEX).
* `debit_card`: Transaction in which the consumer uses a debit card as payment method (E.g. VISA Debit, Maestro).
* `refund`: Transaction in which the merchant returns money to the consumer. A refund may involve any payment method.
* `ticket`: Transaction in which the consumer uses a ticket as payment method. This ticket can be paid through a non-bank collection channel (E.g. Rapipago, Pago Fácil, OXXO).
* `wallet`: Transaction in which the consumer uses a wallet as payment method. A wallet is an application that allows you to transfer money.
* `wire_transfer`: Transaction in which the consumer uses a wire transfer as payment method.

### Transaction Info

| Field                   | Type   | Description                                                  |
| ----------------------- | ------ | ------------------------------------------------------------ |
| `card`                  | Object | [Optional - Only for `credit_card` and `debit_card`] Object containing data related to the consumer's card. See [Card Info](#Card-Info). |
| `installments`          | Object | [Optional - Only for `credit_card`] Object containing the installments data related to this Transaction. See [Installments Info](#Installments-Info). |
| `external_id`           | String | [Optional] ID used by the Payment Provider.                  |
| `external_url`          | String | [Optional] HTTPS URL with details of this Transaction for the merchant. |
| `external_resource_url` | String | [Optional - Only for `boleto` and `ticket`] HTTPS URL of the boleto or ticket to show to the consumer to resume the payment. |
| `external_code`         | String | [Optional - Only for `boleto` and `ticket`] Barcode for boleto, or code for ticket. |
| `ip`                    | String | [Optional] IP of the device that initiated this Transaction. |

> ***Note:*** All URLs must be secure URLs (https).

### Card Info

| Field              | Type   | Description                                                  |
| ------------------ | ------ | ------------------------------------------------------------ |
| `brand`            | String | The brand of the card.                                       |
| `issuer`           | String | [Optional] The issuer of the card.                           |
| `expiration_month` | Number | The expiration month of the card.                            |
| `expiration_year`  | Number | The expiration year of the card.                             |
| `first_digits`     | String | The first 6 (six) digits of the card.                        |
| `last_digits`      | String | The last 4 (four) digits of the card.                        |
| `masked_number`    | String | [Optional] Masked card number displaying only the last 4 (four) digits. E.g. `"XXXXXXXXXXXX1234"`. |
| `name`             | String | Name of the card holder.                                     |

### Installments Info

| Field      | Type   | Description                                              |
| ---------- | ------ | -------------------------------------------------------- |
| `quantity` | Number | The number of installments. E.g. `3`.                    |
| `interest` | String | The interest applied to each installment. E.g. `"0.15"`. |

### Transaction Events

| Field            | Type   | Description                                                  |
| ---------------- | ------ | ------------------------------------------------------------ |
| `id`             | String | [Read-only] Unique identifier of the Transaction Event object. |
| `transaction_id` | String | [Read-only] ID of the [Transaction](#Transaction) related to this Transaction Event. |
| `amount`         | Object | Object containing the amount of this Transaction Event. See [Money](#Money). |
| `type`           | String | One of the available [Transaction Event Types](#Transaction-Event-Types). |
| `status`         | Object | One of the available [Transaction Event Status](#Transaction-Event-Status). |
| `happend_at`     | Date   | ISO 8601 date for the date the Transaction Event was processed. Defaults to current time. E.g. `"2020-03-11T12:42:15.000Z"`. |
| `info`           | Object | Object containing specific info related to this Transaction Event. See [Transaction Event Info](#Transaction-Event-Info). |
| `failure_code`   | String | If the Transaction Event failed, this field is used to indicate the code related to the failure cause. See [Transaction Failure Codes](#Transaction-Failure-Codes). |
| `created_at`     | Date   | [Read-only] ISO 8601 date for the date the Transaction Event was created in our platform. Defaults to current time. E.g. `"2020-03-11T12:42:15.000Z"`. |

### Money

| Field      | Type   | Description                                                 |
| ---------- | ------ | ----------------------------------------------------------- |
| `value`    | String | Amount of money as a string. E.g. `"49.99"`                 |
| `currency` | String | ISO 4217 code for the currency, such as ARS, BRL, USD, etc. |

> ***Note:*** Decimal numbers will be represented as string format for better decimal precision handling. It must contain two decimal places and use a point as decimal separator.

### Transaction Status

Each type of Transaction has a Finite State Machine (FSM) that defines its status:

* `authorized`: The transaction is authorized.
* `failed`: The transaction failed.
* `in_fraud_analisys`: The transaction is under fraud analysis by the payment provider.
* `needs_merchant_review`: The transaction needs merchant action to continue.
* `paid`: The transaction is confirmed.
* `partially_refunded`: The transaction is partially refunded.
* `pending`: The transaction is pending.
* `refunded`: The transaction is refunded.
* `voided`: The transaction is voided.

### Transaction Event Types

* `authorization`: Authorization.
* `capture`: Capture.
* `in_fraud_analysis`: The transaction is being reviewed by the payment provider (no merchant action is required).
* `mark_as_paid`: The transaction was approved by the merchant manually.
* `refunded`: Refund.
* `request_merchant_review`: The transaction has to be approved or rejected by the merchant.
* `sale`: Represents authorization along with capture.
* `void`: The authorization was voided.

### Transaction Event Status

* `error`: There was an error processing the transaction event.
* `failure`: The transaction event failed.
* `pending`: The transaction event is pending.
* `success`: The transaction event succeded.

### Transaction Event Info

| Field         | Type   | Description                                                  |
| ------------- | ------ | ------------------------------------------------------------ |
| `message`     | String | [Optional] Description to explain a Transaction Event update. |
| `fraud_score` | String | [Optional] Decimal score between 0 to 1. The closer the score is to 1, the more likely the Transaction is fraudulent. |
| `risk_level`  | String | [Optional] Risk level that an Order is fraudulent. One of `low`, `medium`or `high`. |
| `accept_url`  | String | [Optional] HTTPS URL we will call to accept the Transaction from our platform. It should return a 2xx HTTP code or we will return an error to the merchant. |
| `cancel_url`  | String | [Optional] HTTPS URL we will call to cancel the Transaction from our platform. It should return a 2xx HTTP code or we will return an error to the merchant. |

Endpoints
---------

### POST /orders/{*order_id*}/transactions

Create a Transaction for a given order.

#### Request

| Field                 | Type   | Description                                                  |
| :-------------------- | :----- | :----------------------------------------------------------- |
| `payment_provider_id` | String | [Required] ID of the [Payment Provider](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/payment_provider.md) that processed this Transaction. |
| `payment_method`      | Object | [Required] Object containing the payment method used in this Transaction. See [Payment Method](#Payment-Method). |
| `first_event`         | Object | [Required] First transaction event that generated this Transaction. See [Transaction Events](#Transaction-Events). |
| `info`                | Object | [Optional] Object containing specific info related to this Transaction. See [Transaction Info](#Transaction-Info). |

#### Response

`HTTP/1.1 201 Created`

The created [Transaction Object](#Transaction) is returned.

### POST /orders/{*order_id*}/transactions/{*transaction_id*}/events

Update the events of a Transaction. 

#### Request

| Field          | Type   | Description                                                  |
| -------------- | ------ | ------------------------------------------------------------ |
| `type`         | String | [Required] One of the available [Transaction Event Types](#Transaction-Event-Types). |
| `status`       | Object | [Required] One of the available [Transaction Event Status](#Transaction-Event-Status). |
| `happend_at`   | Date   | [Required] ISO 8601 date for the date the Transaction Event was processed. Defaults to current time. E.g. `"2020-03-11T12:42:15.000Z"`. |
| `amount`       | Object | [Optional] Object containing the amount of this Transaction Event. See [Money](#Money). |
| `info`         | Object | [Optional] Object containing specific info related to this Transaction Event. See [Transaction Event Info](#Transaction-Event-Info). |
| `failure_code` | String | [Optional] If the Transaction Event failed, this field is used to indicate the code related to the failure cause. See [Transaction Failure Codes](#Transaction-Failure-Codes). |



#### Response

`HTTP/1.1 201 Created`

The created [Transaction Event Object](#Transaction-Events) is returned.


### GET /orders/{*order_id*}/transactions

List all Transactions of a given order.

#### Request

```
{}
```

#### Response

`HTTP/1.1 200 OK`

Array of [Transaction Objects](#Transaction)



### GET /orders/{*order_id*}/transactions/{*transaction_id*}

Get a specific Transaction of a given order.

#### Request

```
{}
```

#### Response

`HTTP/1.1 200 OK`

[Transaction Object](#Transaction)


## HTTP Errors List

* **400 Bad Request** - the request could not be understood or was missing required parameters.
* **401 Unauthorized** - authentication failed or user doesn't have permissions for requested operation.
* **403 Forbidden** - access denied.
* **404 Not Found** - resource was not found.
* **405 Method Not Allowed** - requested method is not supported for resource.


## Common examples

### Example Nº 1

*Credit card transaction that was authorized + captured within a single event.*

### POST /orders/{order_id}/transactions

#### Request

```json
{
  "payment_provider_id": "815905d6-3518-479d-8ed6-5e0e4e6f305d",
  "payment_method" : {
      "type": "credit_card",
      "id": "visa"
  },
  "info" : {
    "card": {
      "brand": "visa",
      "expiration_month": 12,
      "expiration_year": 2020,
      "first_digits": 4444,
      "last_digits": 1234,
      "masked_number": "XXXXXXXXXXXX1234",
      "name": "Ash Ketchum"
    },
    "installments": {
      "quantity": 3,
      "interest": "0.15"
    },
    "external_id": "1234",
    "external_url": "https://mypayments.com/account/transactions/1234",
    "ip": "192.168.0.25"
  },
  "first_event": {
    "amount": {
      "value": "132.95",
      "currency": "ARS"
    },
    "type": "sale",
    "status": "success",
    "happened_at": "2020-01-25T12:30:15.000Z",
  }
}
```

#### Response

`201 Created`

```json
{
  "id": "124123-4518-123f-8ed6-5e0e4e6f305d",
  "payment_provider_id": "815905d6-3518-479d-8ed6-5e0e4e6f305d",
  "captured_amount": {
    "value": "132.95",
    "currency": "ARS"
  },
  "refunded_amount": {
    "value": "0.00",
    "currency": "ARS"
  },
  "payment_method" : {
      "type": "credit_card",
      "id": "visa"
  },
  "status": "paid",
  "info" : {
    "card": {
      "brand": "visa",
      "issuer": "santander",
      "expiration_month": 12,
      "expiration_year": 2020,
      "first_digits": 4444,
      "last_digits": 1234,
      "masked_number": "XXXXXXXXXXXX1234",
      "name": "Ash Ketchum"
    },
    "installments": {
      "quantity": 3,
      "interest": "0.15"
    },
    "external_id": "1234",
    "external_url": "https://mypayments.com/account/transactions/1234",
    "ip": "192.168.0.25"
  },
  "failure_code": null,
  "created_at": "2020-01-25T12:30:20.000Z",
  "events" : [
    {
      "id": "423123-4518-123f-8ed6-5e0e4e6f305d",
      "transaction_id": "124123-4518-123f-8ed6-5e0e4e6f305d",
      "amount": {
        "value": "132.95",
        "currency": "ARS"
      },
      "type": "sale",
      "status": "success",
      "info": null,
      "failure_code": null,
      "created_at": "2020-01-25T12:30:20.000Z",
      "happened_at": "2020-01-25T12:30:15.000Z"
    }
  ]
}
```


### Example Nº 2

*Boleto transaction that starts pending and then is paid by the buyer.*

### POST /orders/{order_id}/transactions

#### Request

```json
{
  "payment_provider_id": "815905d6-3518-479d-8ed6-5e0e4e6f305d",
  "payment_method" : {
      "type": "boleto",
      "id": "bradesco"
  },
  "info" : {
    "external_id": "1234",
    "external_url": "https://mypayments.com/account/transactions/1234",
    "external_resource_url": "https://mypayments.com/boleto/1234",
    "external_resource_code": "00190500954014481606906809350314337370000000100",
    "ip": "192.168.0.25"
  },
  "first_event": {
    "amount": {
      "value": "132.95",
      "currency": "ARS"
    },
    "type": "sale",
    "status": "pending",
    "happened_at": "2020-01-25T12:30:15.000Z",
  }
}
```

#### Response

`201 Created`

```json
{
  "id": "124123-4518-123f-8ed6-5e0e4e6f305d",
  "payment_provider_id": "815905d6-3518-479d-8ed6-5e0e4e6f305d",
  "captured_amount": {
    "value": "0.00",
    "currency": "ARS"
  },
  "refunded_amount": {
    "value": "0.00",
    "currency": "ARS"
  },
  "payment_method" : {
      "type": "boleto",
      "id": "bradesco"
  },
  "status": "pending",
  "info" : {
    "installments": {
      "quantity": 1,
      "interest": "0.00"
    },
    "external_id": "1234",
    "external_url": "https://mypayments.com/account/transactions/1234",
    "external_resource_url": "https://mypayments.com/boleto/1234",
    "external_resource_code": "00190500954014481606906809350314337370000000100",
    "ip": "192.168.0.25"
  },
  "failure_code": null,
  "created_at": "2020-01-25T12:30:20.000Z",
  "events" : [
    {
      "id": "423123-4518-123f-8ed6-5e0e4e6f305d",
      "transaction_id": "124123-4518-123f-8ed6-5e0e4e6f305d",
      "amount": {
        "value": "132.95",
        "currency": "ARS"
      },
      "type": "sale",
      "status": "pending",
      "info": null,
      "failure_code": null,
      "created_at": "2020-01-25T12:30:20.000Z",
      "happened_at": "2020-01-25T12:30:15.000Z"
    }
  ]
}
```

### POST /orders/{order_id}/transactions/{transaction_id}/events

#### Request

```json
{
    "type": "sale",
    "status": "success",
    "happened_at": "2020-01-27T12:30:15.000Z",
  }
}
```

#### Response

`201 Created`

```json
{
  "id": "423124-4518-123f-8ed6-5e0e4e6f305d",
  "transaction_id": "124123-4518-123f-8ed6-5e0e4e6f305d",
  "amount": {
    "value": "132.95",
    "currency": "ARS"
  },
  "type": "sale",
  "status": "success",
  "info": null,
  "failure_code": null,
  "created_at": "2020-01-25T12:30:20.000Z",
  "happened_at": "2020-01-25T12:30:15.000Z"
}
```

### Example Nº 3

*Obtaining a Transaction resource by ID.*

### GET /orders/{order_id}/transactions/{transaction_id}

#### Request

```
{}
```

#### Response

```json
{
  "id": "124123-4518-123f-8ed6-5e0e4e6f305d",
  "payment_provider_id": "815905d6-3518-479d-8ed6-5e0e4e6f305d",
  "captured_amount": {
    "value": "132.95",
    "currency": "ARS"
  },
  "refunded_amount": {
    "value": "0.00",
    "currency": "ARS"
  },
  "payment_method" : {
      "type": "boleto",
      "id": "bradesco"
  },
  "status": "pending",
  "info" : {
    "installments": {
      "quantity": 1,
      "interest": "0.00"
    },
    "external_id": "1234",
    "external_url": "https://mypayments.com/account/transactions/1234",
    "external_resource_url": "https://mypayments.com/boleto/1234",
    "external_resource_code": "00190500954014481606906809350314337370000000100",
    "ip": "192.168.0.25"
  },
  "failure_code": null,
  "created_at": "2020-01-25T12:30:20.000Z",
  "events" : [
    {
      "id": "423123-4518-123f-8ed6-5e0e4e6f305d",
      "transaction_id": "124123-4518-123f-8ed6-5e0e4e6f305d",
      "amount": {
        "value": "132.95",
        "currency": "ARS"
      },
      "type": "sale",
      "status": "pending",
      "info": null,
      "failure_code": null,
      "created_at": "2020-01-25T12:30:20.000Z",
      "happened_at": "2020-01-25T12:30:15.000Z"
    },
    {
      "id": "423124-4518-123f-8ed6-5e0e4e6f305d",
      "transaction_id": "124123-4518-123f-8ed6-5e0e4e6f305d",
      "amount": {
        "value": "132.95",
        "currency": "ARS"
      },
      "type": "sale",
      "status": "success",
      "info": null,
      "failure_code": null,
      "created_at": "2020-01-25T12:30:20.000Z",
      "happened_at": "2020-01-25T12:30:15.000Z"
    }
  ]
}
```


## Appendix

### Transaction Failure Codes

The following list contains all the Transaction failures codes currently supported by our platform, organized by data groups.

### Consumer

| Failure Code                     | Description                                   |
| -------------------------------- | --------------------------------------------- |
| `consumer_city_invalid`          | The consumer city is invalid.                 |
| `consumer_country_invalid`       | The consumer country is invalid.              |
| `consumer_district_invalid`      | The consumer district is invalid.             |
| `consumer_email_invalid`         | The consumer email is invalid.                |
| `consumer_firstname_invalid`     | The consumer firstname is invalid.            |
| `consumer_floor_invalid`         | The consumer address floor is invalid.        |
| `consumer_id_type_invalid`       | The consumer identification type is invalid.  |
| `consumer_id_value_invalid`      | The consumer identification value is invalid. |
| `consumer_lastname_invalid`      | The consumer lastname is invalid.             |
| `consumer_phone_invalid`         | The consumer phone number is invalid.         |
| ``consumer_province_invalid`     | The consumer province is invalid.             |
| `consumer_region_invalid`        | The consumer region is invalid.               |
| `consumer_state_invalid`         | The consumer state is invalid.                |
| `consumer_street_invalid`        | The consumer address street is invalid.       |
| `consumer_street_number_invalid` | The consumer address number is invalid.       |
| `consumer_zip_invalid`           | The consumer ZIP code is invalid.             |

### Payment Method

#### Bank Debit

| Failure Code                        | Description                                           |
| ----------------------------------- | ----------------------------------------------------- |
| `bank_debit_bank_invalid`           | The bank debit bank is invalid.                       |
| `bank_debit_method_unavailable`     | The bank debit payment method is not available.       |
| `bank_debit_payer_id_type_invalid`  | The bank debit payer identification type is invalid.  |
| `bank_debit_payer_id_value_invalid` | The bank debit payer identification value is invalid. |
| `bank_debit_payer_name_invalid`     | The bank debit payer name is invalid.                 |

#### Boleto

| Failure Code                    | Description                                       |
| ------------------------------- | ------------------------------------------------- |
| `boleto_method_unavailable`     | The boleto method is not available.               |
| `boleto_payer_id_type_invalid`  | The boleto payer identification type is invalid.  |
| `boleto_payer_id_value_invalid` | The boleto payer identification value is invalid. |
| `boleto_payer_name_invalid`     | The boleto payer name is invalid.                 |

#### Card

| Failure Code                         | Description                                                  |
| ------------------------------------ | ------------------------------------------------------------ |
| `card_cvv_invalid`                   | The card security code is invalid.                           |
| `card_expiration_date_invalid`       | The card expiration date is invalid.                         |
| `card_holder_birthdate_invalid`      | The cardholder date of birth is invalid.                     |
| `card_holder_id_type_invalid`        | The cardholder identification type is invalid.               |
| `card_holder_id_value_invalid`       | The cardholder identification value is invalid.              |
| `card_holder_name_invalid`           | The cardholder name is invalid.                              |
| `card_holder_phone_invalid`          | The cardholder phone number is invalid.                      |
| `card_info_invalid`                  | The card information is invalid.                             |
| `card_issuer_invalid`                | The card issuer is invalid.                                  |
| `card_method_unavailable`            | The card method is not available.                            |
| `card_number_invalid`                | The card number is invalid.                                  |
| `card_rejected`                      | Card rejected.                                               |
| `card_rejected_deny_list`            | Card rejected for being on a list of non accepted cards.     |
| `card_rejected_disabled`             | Card rejected for being disabled.                            |
| `card_rejected_duplicated_payment`   | Card rejected because payment seems to be duplicated.        |
| `card_rejected_fraud_high_risk`      | Card rejected due to high fraud risk.                        |
| `card_rejected_insufficient_funds`   | Card rejected for insufficient funds.                        |
| `card_rejected_invalid_installments` | Card rejected for not supporting the specified installments. |
| `card_rejected_max_attemps`          | Card rejected for exceeding the number of attempts.          |

#### Ticket

| Failure Code                | Description                         |
| --------------------------- | ----------------------------------- |
| `ticket_method_unavailable` | The ticket method is not available. |
| `ticket_operator_invalid`   | The ticket operator is invalid.     |

### Shipping

| Failure Code                     | Description                                     |
| -------------------------------- | ----------------------------------------------- |
| `shipping_city_invalid`          | The shipping city is invalid.                   |
| `shipping_district_invalid`      | The shipping district is invalid.               |
| `shipping_email_invalid`         | The shipping recipient email is invalid.        |
| `shipping_firstname_invalid`     | The shipping recipient firstname is invalid.    |
| `shipping_floor_invalid`         | The shipping address floor is invalid.          |
| `shipping_lastname_invalid`      | The shipping recipient lastname is invalid.     |
| `shipping_method_invalid`        | The shipping method is invalid.                 |
| `shipping_method_unavailable`    | The shipping method is not available.           |
| `shipping_phone_invalid`         | The shipping recipient phone number is invalid. |
| `shipping_province_invalid`      | The shipping province is invalid.               |
| `shipping_region_invalid`        | The shipping region is invalid.                 |
| `shipping_state_invalid`         | The shipping state is invalid.                  |
| `shipping_street_invalid`        | The shipping address street is invalid.         |
| `shipping_street_number_invalid` | The shipping address number is invalid.         |
| `shipping_total_curreny_invalid` | The shipping amount currency is invalid.        |
| `shipping_total_value_invalid`   | The shipping amount value is invalid.           |
| `shipping_zip_invalid`           | The shipping ZIP code is invalid.               |

### Order

| Failure Code                     | Description                                               |
| -------------------------------- | --------------------------------------------------------- |
| `line_items_currency_invalid`    | An order item amount currency is invalid.                 |
| `line_items_description_invalid` | An order item description is invalid.                     |
| `line_items_quantity_invalid`    | An order item quantity is invalid.                        |
| `line_items_value_invalid`       | An order item amount value is invalid.                    |
| `order_total_currency_invalid`   | The order amount currency is invalid.                     |
| `order_total_value_invalid`      | The order amount value is invalid.                        |
| `order_total_value_too_small`    | The order value is less than the minimum supported value. |
