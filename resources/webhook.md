# Webhook

A Webhook is a tool that allows you to receive a notification for a certain event. It allows you to register an _https_ URL which will receive the event data, stored in JSON. Webhooks can be registered for the following events:

| Category | Events                                          |
| -------- | ----------------------------------------------- |
| App      | uninstalled/suspended/resumed                   |
| Category | created/updated/deleted                         |
| Order    | created/updated/paid/packed/fulfilled/cancelled |
| Product  | created/updated/deleted                         |
| Domain   | updated                                         |
| Theme    | updated                                         |

To register for the product created event, for example, you should send `product/created` in the event field. Basically, you make a POST request in Webhooks endpoint providing a body with the type of webhook you want to create as well as the URL where you will be notified.

Now let's say you want to be notified whenever an order gets paid so you don't have to make scheduled calls to retrieve this information. Here you should create a Webhook with the event `order/paid` and the URL where you will be notified in your backend.

The `app/suspended` and `app/resumed` events refer to the [suspension of API access due to lack of payment](https://github.com/TiendaNube/api-docs/#suspension-of-api-access-due-to-lack-of-payment).

You are not allowed to use a localhost/tiendanube/nuvemshop domain for webhooks.

To aid in your development you can use [RequestCatcher](https://requestcatcher.com/). PostCatcher is a web app that generates a unique url that you can use as a webhook endpoint and display any POST requests sent to it for you to examine.

## Properties

| Property   | Explanation                                                                                        |
| ---------- | -------------------------------------------------------------------------------------------------- |
| id         | The unique numeric identifier for the Webhook                                                      |
| url        | The URL where the webhook should send the POST request when the event occurs. **Must be HTTPS**.   |
| event      | The event that will trigger the webhook                                                            |
| created_at | Date when the Webhook was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)      |
| updated_at | Date when the Webhook was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601) |

### Verifying a webhook

Webhooks created through the API can be verified by calculating a digital signature.

Each Webhook request includes a X-Linkedstore-HMAC-SHA256 header which is generated using the app's secret, along with the data sent in the request.

**Note that some frameworks will change the header to HTTP_X_LINKEDSTORE_HMAC_SHA256**.

See PHP example below:

```php
<?php

define('APP_SECRET', 'secret');

function verify_webhook($data, $hmac_header) {
  return $hmac_header == hash_hmac('sha256', $data, APP_SECRET);
}

$hmac_header = $_SERVER['HTTP_X_LINKEDSTORE_HMAC_SHA256'];
$data = file_get_contents('php://input');
$verified = verify_webhook($data, $hmac_header);
echo 'Webhook verified: ' . var_export($verified, true);
```

### Parameters

When doing the POST request, all webhooks will send the following parameters in JSON format:

- **store_id**: Store from where the event originated (received as `user_id` at the [authentication](./authentication.md) step)
- **event**: Event's name (product/created, product/updated, etc.)

Also, every webhook will send custom parameters, as follows:

#### app/uninstalled

- **id**: App's id.

#### category/created - category/updated - category/deleted

- **id**: Category's id.

#### order/created - order/updated - order/paid - order/packed - order/fulfilled - order/cancelled

- **id**: Order's id.

#### product/created - product/updated - product/deleted

- **id**: Product's id.

#### domain/updated

No additional parameter is sent along with this event. To get the list of domains one may refer to [Store](https://github.com/tiendanube/api-docs/blob/master/resources/store.md) resources.

#### theme/updated

- **old_theme**: Old Theme's code.
- **new_theme**: New Theme's code.

#### Example webhook content

```javascript
{
  'store_id':123,
  'event':'product/created',
  'id':1948209
}
```

### Retry policies

A Webhook notification expects a 2XX status code in response (to guarantee that the App has received the notification) and has a timeout of 40 seconds. That is, we wait 40 seconds for a 2XX code in response (it can be any code that starts with 2 - 200/201, etc).

When a webhook notification cannot be delivered due to a problem (e.g when the App is down) then retry policy is on.

The first 4 retries are: one immediately after timeout and then at approximately 5, 10 and 15 minutes. Then it starts to do exponential backoff multiplying the waiting time of the previous one by 1.4 within the next 48 hours (up to 18 attempts).

Overview of the process:

- We send the webhook.
- We wait 40 seconds for response confirmation by the App.
- If we do not receive any responses, then retry policy is activated.

A success response code will stop next notifications. But, if that does not happen at all, notifications will be sent 18 times.

## Required Webhooks

In order for our company to comply with data protection laws (example [LGPD](https://www.lgpdbrasil.com.br/wp-content/uploads/2019/06/LGPD-english-version.pdf) in Brazil), we provide Webhooks that facilitate communication between consumers, merchants and integrated applications, to trigger notices of removal or update of shared data, when a request is received for us.

Basically, the LPGD obliges technology platforms to provide mechanisms through which data owners can:

- Request a report from the platform to find out what personal data of that person it has.
- Request the platform to delete personal data.

Tiendanube apps are digital platforms that can store data from both merchants and consumers. That is why partners also have the responsibility to comply with this law. They are obliged to delete data or to report what data they have, whenever requested.

So that partners can find out about these orders, from Tiendanube we created this mechanism of webhooks. Webhooks represent automatic notifications that Tiendanube will send to partners when specific events occur that require deleting/reporting data. These webhooks are three:

![image](https://user-images.githubusercontent.com/4175639/116715345-0009a300-a9ad-11eb-9b91-3a408150a343.png)

### store/redact

Request to delete store information. 48 hours after a store uninstalls your app, Nuvemshop|Tiendanube sends this webhook with the store ID so that you can delete the shopkeeper's information from your database.
Payload:

```javascript
{
  store_id: 123;
}
```

### customers/redact

Request to erase consumer information. If the consumer did not place an order at the store 6 months ago, the webhook will be sent after 5 days. If the customer has placed an order in the last 6 months, the deleted order will be held until 6 months have passed.
Payload:

```javascript
{
store_id: 123,
customer: {
	id: 1,
	email: email@email.com
	phone: +55...,
	identification: ...
},
orders_to_redact: [213, 3415, 21515]
}
```

### customers/data_request

Request for customer information. It is the application's responsibility to send the information directly to the merchant.
Payload:

```javascript

{
store_id: 123,
customer: {
	id: 1,
	email: email@email.com
	phone: +55...,
	identification: ...
},
orders_requested: [213, 3415, 21515],
checkouts_requested: [214, 3416, 21518],
drafts_orders_requested: [10, 1245, 5456],
data_request: {
	id: 456,
}
}
```

## Endpoints

### GET /webhooks

Receive a list of all Webhooks for your application.

| Parameter      | Explanation                                                                                       |
| -------------- | ------------------------------------------------------------------------------------------------- |
| since_id       | Restrict results to after the specified ID                                                        |
| url            | Show webhooks tags with a given URL                                                               |
| event          | Show webhooks with a given event                                                                  |
| created_at_min | Show Webhooks created after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))       |
| created_at_max | Show Webhooks created before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))      |
| updated_at_min | Show Webhooks last updated after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))  |
| updated_at_max | Show Webhooks last updated before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)) |
| page           | Page to show                                                                                      |
| per_page       | Amount of results                                                                                 |
| fields         | Comma-separated list of fields to include in the response                                         |

#### GET /webhooks

`HTTP/1.1 200 OK`

```json
[
  {
    "created_at": "2013-01-03T09:11:51-03:00",
    "event": "app/uninstalled",
    "id": 101,
    "updated_at": "2013-03-11T09:14:11-03:00",
    "url": "https://myapp.com/uninstall"
  },
  {
    "created_at": "2013-04-07T09:11:51-03:00",
    "event": "order/created",
    "id": 5670,
    "updated_at": "2013-04-08T11:11:51-03:00",
    "url": "https://myapp.com/order_created_hook"
  }
]
```

#### GET /webhooks?since_id=1234

`HTTP/1.1 200 OK`

```json
[
  {
    "created_at": "2013-04-07T09:11:51-03:00",
    "event": "order/created",
    "id": 5670,
    "updated_at": "2013-04-08T11:11:51-03:00",
    "url": "https://myapp.com/order_created_hook"
  }
]
```

### GET /webhooks/{id}

Receive a single Webhook

| Parameter | Explanation                                               |
| --------- | --------------------------------------------------------- |
| fields    | Comma-separated list of fields to include in the response |

#### GET /webhooks/5670

`HTTP/1.1 200 OK`

```json
{
  "created_at": "2013-04-07T09:11:51-03:00",
  "event": "order/created",
  "id": 5670,
  "updated_at": "2013-04-08T11:11:51-03:00",
  "url": "https://myapp.com/order_created_hook"
}
```

### POST /webhooks

Create a new Webhook

#### POST /webhooks

```json
{
  "url": "foobar",
  "event": "invalid_event"
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
  "event": [
    "Invalid event specified. Events allowed: app/uninstalled, cart/created, cart/updated, category/created, category/updated, category/deleted, order/created, order/updated, order/paid, order/fulfilled, order/cancelled, product/created, product/updated, product/deleted"
  ],
  "url": ["invalid url specified"]
}
```

#### POST /webhooks

```json
{
  "event": "product/created",
  "url": "https://myapp.com/product_created_hook"
}
```

`HTTP/1.1 201 Created`

```json
{
  "created_at": "2013-06-01T15:12:15-03:00",
  "event": "product/created",
  "id": 8901,
  "updated_at": "2013-06-01T15:12:15-03:00",
  "url": "https://myapp.com/product_created_hook"
}
```

### PUT /webhooks/{id}

Modify an existing Webhook

#### PUT /webhooks/5670

```json
{
  "created_at": "2013-04-07T09:11:51-03:00",
  "event": "category/created",
  "id": 5670,
  "updated_at": "2013-04-08T11:11:51-03:00",
  "url": "https://myapp.com/category_created_hook"
}
```

`HTTP/1.1 200 OK`

```json
{
  "created_at": "2013-04-07T09:11:51-03:00",
  "event": "category/created",
  "id": 5670,
  "updated_at": "2013-06-01T12:11:14-03:00",
  "url": "https://myapp.com/category_created_hook"
}
```

### DELETE /webhooks/{id}

Remove a Webhook

#### DELETE /webhooks/5670

`HTTP/1.1 200 OK`

```json
{}
```
