# Checkout

## Payment Options (Javascript Interface)

Our Checkout flow offers different Payment Options that provides consumers with the means to pay for an order with the payment method of their choice. Payments App developers can create their own Payment Options.

Payment Options configuration params are set via our [Payment Provider API](payment_provider.md) by adding [`checkout_payment_options`](payment_provider.md#Checkout-Options) on the Payment Provider object creation.

Our Checkout triggers a variety of events for which we provide a JavaScript API that allows you to handle these events freely to perform the payment process. Hence, you can implement their _(most likely)_ already existing and widely tested Javascript SDKs.

The file with the handlers implemented for the different options should be hosted on a CDN that must be capable of handling high traffic loads. The HTTPS URL to this file must be set in the [Payment Provider](payment_provider.md) `checkout_js_url` property.

## Examples

### External Payment Option

Let's take a look at a simple script for a hypothetical integration with a Payment Provider that redirects the user to *'acmepayments.com'* to complete the purchase in their checkout. This is what we call a `redirect` checkout.

```javascript
// Call 'LoadCheckoutPaymentContext' method and pass a function as parameter to get access to the Checkout context and the PaymentOptions object.

LoadCheckoutPaymentContext(function(Checkout, PaymentOptions) {
  
	// Create a new instance of external Payment Option and set its properties.
	var AcmeExternalPaymentOption = PaymentOptions.ExternalPayment({
		// Set the option's unique id as it is configured on the Payment Provider so Checkout can relate them.
		id: 'acme_redirect',

    // This parameter renders the billing information form and requires the information to the consumer.
		fields: {
			billing_address: true
		},

		// This function handles the order submission event.
		onSubmit: function(callback) {

			// Gather the minimum required information. You should include all the relevant data here.
			let acmeRelevantData = {
				orderId: Checkout.getData('order.cart.id'),
				currency: Checkout.getData('order.cart.currency'),
				total: Checkout.getData('order.cart.prices.total')
			};

			// Use the Checkout HTTP library to post a request to our server and fetch the redirect URL.
			Checkout.http
				.post('https://app.acme.com/generate-checkout-url', {
					data: acmeRelevantData
				})
				.then(function(responseBody) {
					// Once you get the redirect URL, invoke the callback by passing it as argument.
					if (responseBody.data.success) {
						callback({
							success: true,
							redirect: responseBody.data.redirect_url,
							extraAuthorize: true // Legacy paameter, but currently required with `true` value. Will be deprecrated soon.
						});
					} else {
						callback({
							success: false,
							error_code: responseBody.data.error_code
						});
					}
				})
				.catch(function(error) {
					// Handle a potential error in the HTTP request.

					callback({
						success: false,
						error_code: 'unknown_error'
					});
				});
		}
	});

	// Finally, add the Payment Option to the Checkout object so it can be render according to the configuration set on the Payment Provider.
	Checkout.addPaymentOption(AcmeExternalPaymentOption);
});
```

### Card Payment Option

This is a more complex example, since this is a richer interaction with more control over the user experience. The entire flow happens without leaving the Tiendanube checkout, in what we call a `transparent` checkout.

This type of payment renders a form where the consumer inputs their credit card information and with which you can interact.

In this example, whenever the consumer inputs or changes the credit card number we fetch the first 6 digits and populate the list of available installments.

```javascript
LoadCheckoutPaymentContext(function(Checkout, PaymentOptions) {

	var currentTotalPrice = Checkout.getData('order.cart.prices.total');
	var currencCardBin = null;

	// Some helper functions.

	// Get credit the card number from transparent form.
	var getCardNumber = function() {
		return Checkout.getData('form.cardNumber');
	};

	// Get the first 6 digits from the credit card number.
	var getCardNumberBin = function() {
		return getCardNumber().substring(0, 6);
	};

	// Check whether the BIN (first 6 digits of the credit card number) has changed. If so, we intend to update the available installments.
	var mustRefreshInstallments = function() {
		var cardBin = getCardNumberBin();
		var hasCardBin = cardBin && cardBin.length >= 6;
		var hasPrice = Boolean(Checkout.getData('totalPrice'));
		var changedCardBin = cardBin !== currencCardBin;
		var changedPrice = Checkout.getData('totalPrice') !== currentTotalPrice;
		return (hasCardBin && hasPrice) && (changedCardBin || changedPrice);
	};

	// Update the list of installments available to the consumer.
	var refreshInstallments = function() {
		// Let's imagine the App provides this endpoint to obtain installments.
    
		Checkout.http.post('https://acmepayments.com/card/installments', {
			amount: Checkout.getData('totalPrice'),
			bin: getCardNumberBin()
		}).then(function(response) {
			Checkout.setInstallments(response.data.installments);
		});
	};

	// Create a new instance of card Payment Option and set its properties.
	var AcmeCardPaymentOption = PaymentOptions.Transparent.CardPayment({

		// Set the option's unique `i`` as it is configured on the Payment Provider so Checkout can relate them.
		id: "acme_transparent_card",

		// Event handler for form field input.
		onDataChange: Checkout.utils.throttle(function() {
			if (mustRefreshInstallments()) {
				refreshInstallments();
			} else if (!getCardNumberBin()) {
				// Clear installments if customer remove credit card number.
				Checkout.setInstallments(null);
			}
		}),

		onSubmit: function(callback) {
			// Gather the minimum required information.
			var acmeCardRelevantData = {
				orderId: Checkout.getData('order.cart.id'),
				currency: Checkout.getData('order.cart.currency'),
				total: Checkout.getData('order.cart.prices.total'),
				card: {
					number: Checkout.getData('form.cardNumber'),
					name: Checkout.getData('form.cardHolderName'),
					expiration: Checkout.getData('form.cardExpiration'),
					cvv: Checkout.getData('form.cardCvv'),
					installments: Checkout.getData('form.cardInstallments')
				}
			};
			// Let's imagine the App provides this endpoint to process credit card payments.
			Checkout.http.post('https://acmepayments.com/charge', acmeCardRelevantData)
				.then(function(responseBody) {
					if (responseBody.data.success) {
						// If the charge was successful, invoke the callback to indicate you want to close order.
						callback({
							success: true
						});
					} else {
						callback({
							success: false,
							error_code: responseBody.data.error_code
						});

					}
				})
				.catch(function(error) {
					// Handle a potential error in the HTTP request.
        
					callback({
						success: false,
						error_code: 'unknown_error'
					});
				});
		}
	});

	// Finally, add the Payment Option to the Checkout object so it can be render according to the configuration set on the Payment Provider.
	Checkout.addPaymentOption(AcmeCardPaymentOption);
});
```

## Checkout Context

The `LoadCheckoutPaymentContext` function takes function as a argument, which will be invoked with two arguments, `Checkout` and `PaymentOptions`, to provide access to our Checkout's context.

| Name               | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| `updateFields`     | Let's you add or remove optional input fields from the payment option selection form. |
| `addPaymentOption` | Register the option so the checkout can inject the configuration params and render it. |
| `setInstallments`  | Update the attributes of the `data.installments` object. See [Installments](#Installments). |
| `getData`          | function to obtain the data of the shopping cart, the consumer and more. See [getData](#getData). |
| `http`             | Function to perform AJAX requests. See [HTTP](#HTTP).        |
| `utils`            | Collection of helper functions. See [Utils](#Utils).         |

### Checkout

#### HTTP

This object is an [Axios instance](https://github.com/axios/axios#request-config). Though the `fetch` is now available on all major object, using this method ensures cross-browser compatibility and it will also allow us to detect unexpected behaviours for which we'll be able to trigger alerts.

> Note: This instance of Axios already has a few params set by the Checkout.

##### Standard POST example

```js
Checkout.http.post('https://acmepayments.com/charge', {
  cartId: cartId,
}).then(function(response) {
  // Do something with the response.
});
```

##### Custom config request example

```js
Checkout.http({
  url: 'https://acmepayments.com/charge',
  method: 'post',
  data: {
    cartId: cartId,
  }
}).then(function(response) {
    // Do something with the response.
});
```

#### Utils

- `Checkout.utils.Throttle`
- `Checkout.utils.LoadScript`
- `Checkout.utils.FlattenObject`

#### getData

The Checkout object provides the app with access to all the data related with ongoing sale. We've got the following data groups:
- Cart Information: `Checkout.getData('order.cart')`.
- Total cart price: `Checkout.getData('totalPrice')` (also indicated by `Checkout.getData('order.cart.prices.total')`).
- ID of the store to which the cart belongs: `Checkout.getData('storeId')`.
- Customer Contact Information: `Checkout.getData('order.contact')`.
- Billing Information: `Checkout.getData('order.billingAddress')`.
- Shipping Information: `Checkout.getData('order.shippingAddress')`.
- Shipping Method Information: `Checkout.getData('order.cart.shipping')`.
- Payment Method Information: `Checkout.getData('form')`.

*Note:* No all Payment Method Information fields are rendered. They can be rendered as explained [here](./checkout.md#fields-property).

Here's an example of the data available in the `Checkout.getData()` object (rendered as JSON for better readability):

```json
{
   "form":{},
   "totalPrice":135,
   "country":"AR",
   "storeId":1196173,
   "order":{
      "cart":{
         "id":375854104,
         "hash":"aebe04afab671411e6d75352fb4f514898b1667a",
         "number":null,
         "prices":{
            "shipping":15,
            "discount_gateway":0,
            "discount_coupon":30,
            "discount_promotion":0,
            "discount_coupon_and_promotions":30,
            "subtotal_with_promotions_applied":150,
            "subtotal_with_promotions_and_coupon_applied":120,
            "subtotal":150,
            "total":135,
            "total_usd":0
         },
         "lineItems":[
            {
               "id":451294379,
               "name":"Example Product 1",
               "price":"50.50",
               "quantity":1,
               "free_shipping":false,
               "product_id":58979310,
               "variant_id":175499404,
               "thumbnail":"//d2qa76c3k7tf6c.cloudfront.net/stores/001/196/173/products/example-product-1.jpg",
               "variant_values":"",
               "sku":null,
               "properties":[],
               "url":"https://examplestore.com/productos/example-product-1/?variant=175499404",
               "is_ahora_12_eligible":true
            },
            {
               "id":451294230,
               "name":"Example Product 2",
               "price":"99.50",
               "quantity":1,
               "free_shipping":false,
               "product_id":58979280,
               "variant_id":175499176,
               "thumbnail":"//d2qa76c3k7tf6c.cloudfront.net/stores/001/196/173/products/example-product-2.jpg",
               "variant_values":"",
               "sku":null,
               "properties":[],
               "url":"https://examplestore.com/productos/example-product-2/?variant=175499176",
               "is_ahora_12_eligible":true
            }
         ],
         "currency":"ARS",
         "currencyFormat":{
            "short":"$%s",
            "long":"$%s ARS"
         },
         "lang":"es",
         "langCode":"es_AR",
         "coupon":{
            "id":1566261,
            "code":"DESC20",
            "type":"percentage",
            "value":"20.00",
            "valid":true,
            "used":0,
            "max_uses":null,
            "start_date":null,
            "end_date":null,
            "min_price":null,
            "categories":null
         },
         "shipping":{
            "type":"ship",
            "method":"correo-argentino",
            "option":6111227,
            "branch":null,
            "disabled":null,
            "raw_name":"Correo Argentino - Encomienda Clásica",
            "suboption":null
         },
         "status":{
            "order":"open",
            "order_cancellation_reason":null,
            "fulfillment":"unpacked",
            "payment":"pending"
         },
         "completedAt":null,
         "minimumValue":null,
         "hasNonShippableProducts":false,
         "hasShippableProducts":true,
         "isAhora12Eligible":true
      },
      "shippingAddress":{
         "first_name":"John",
         "last_name":"Doe",
         "phone":"+54123456789",
         "address":"Example Street",
         "number":"1234",
         "floor":"",
         "locality":"Valentín Alsina",
         "city":"Lanús",
         "state":"Buenos Aires",
         "zipcode":"1822",
         "country":"AR",
         "between_streets":"",
         "reference":"",
         "id_number":"11223344"
      },
      "billingAddress":{
         "first_name":"John",
         "last_name":"Doe",
         "phone":"+54123456789",
         "address":"Example Street",
         "number":"1234",
         "floor":"",
         "locality":"Valentín Alsina",
         "city":"Lanús",
         "state":"Buenos Aires",
         "zipcode":"1822",
         "country":"AR",
         "between_streets":"",
         "reference":"",
         "id_number":"11223344"
      },
      "contact":{
         "email":"john.doe@example.com",
         "name":"John Doe",
         "phone":"+54123456789"
      }
   }
}
```

### PaymentOptions

The second argument of the function passed as an argument to `LoadCheckoutPaymentContext` is `PaymentOptions`. It contains functions for each of the different possible integration types. Each of the functions take a configuration object as an argument and, in turn, will return a javascript instance of the `PaymentOption`.

| Name                | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| `ExternalPayment()` | Returns an instance of the PaymentOption for integration types that require redirecting the consumer to a different website. |
| `ModalPayment()`    | Returns an instance of the PaymentOption for integration types that require opening a Modal in the store's frontend. |
| `CustomPayment()`   | Used internally for the merchant's custom payment methods.    |
| `Transparent`       | Object that contains functions to obtain instances for transparent integration types. |

Note: ExternalPayment and ModalPayment won't render any input fields on the frontend. The main difference between them is on their `onSubmit` `callback` parameters.

#### Transparent Integration Type

The `PaymentOptions.Transparent` has one function per each of the payment methods for which we support transparent integration type. Each of these funcitons return an instance of the `PaymentOption` for their specific payment methods and, if added to the Checkout using `Checkout.addPaymentOption(paymentOptionInstance)` a form will be rendered with all the required input fields for that payment method.


| Name              | Description                                           |
| ----------------- | ----------------------------------------------------- |
| `CardPayment()`   | For `credit_card` and `debit_card` payment methods.   |
| `DebitPayment()`  | For `bank_debit` payment method (aka "online debit"). |
| `BoletoPayment()` | For payments with `boleto` payment method.            |
| `TicketPayment()` | For payments with `ticket` payment method.            |
| `PixPayment()`    | For payments with `pix` payment method.               |

##### CardPayment

These are the fields rendered and available on the `Checkout.getData('form')` object.

| Name                  | Description                                            | Required     | `fields` value           |
| --------------------- | ------------------------------------------------------ | ------------ | ------------------------ |
| `cardNumber`          | Card number.                                           | Always       |                          |
| `cardHolderName`      | Card holder's name.                                    | Always       |                          |
| `cardExpiration`      | Card's expiration date in `mm/yy` format.              | Always       |                          |
| `cardCvv`             | Card's verification code.                              | Always       |                          |
| `cardInstallments`    | Number of installments selected by the consumer.       | Always       |                          |
| `cardHolderIdNumber`  | Card holder's identification (CPF, DNI or equivalent). | Optional     | `card_holder_id_number`  |
| `cardHolderIdType`    | Card holder's identification (CPF, DNI or equivalent). | Optional     | `card_holder_id_types`   |
| `cardHolderBirthDate` | Card holder's birthday in `dd/mm/yy` format.           | Optional     | `card_holder_birth_date` |
| `cardHolderPhone`     | Card holder's phone number.                            | Optional     | `card_holder_phone`      |
| `bankId`              | Card's issuing bank.                                   | Optional     | `bankList`               |

> `cardBrand` is inserted after the user enters the first six credit card numbers

##### DebitPayment

These are the input fields rendered and available in the object `Checkout.getData('form')`.

| Name             | Description                                                | Required     | `fields` value           |
| ---------------- | ---------------------------------------------------------- | ------------ | ------------------------ |
| `bank`           | Bank to debit from.                                        | Optional     | `bank_list`              |
| `holderName`     | Account holder's name.                                     | Optional     | `debit_card_holder_name` |
| `holderIdNumber` | Account holder's identification (CPF, DNI, or equivalent). | Optional     | `debit_card_id_number`   |

##### BoletoPayment

These are the input fields rendered and available in the object `Checkout.getData('form')`.

| Name             | Description                                          | Required     | `fields` value       |
| ---------------- | ---------------------------------------------------- | ------------ | -------------------- |
| `holderName`     | Consumer's name.                                     | Optional     | `boleto_holder_name` |
| `holderIdNumber` | Consumer's identification (CPF, CNPJ or equivalent). | Optional     | `boleto_id_number`   |

##### TicketPayment

These are the input fields rendered and available in the object `Checkout.getData('form')`.

| Name    | Description                                  | Required     | `fields` value       |
| --------| -------------------------------------------- | ------------ | -------------------- |
| `brand` | Brand name for selected cash list option     | Always       | `efectivo_list`      |
| `efectivo_list` | Brand name for selected ticket or boleto list option | Always | [{ name: 'PagoFacil', code: 'pagofacil' }, { name: 'Rapipago', code: 'rapipago' }, { name: 'Caixa', code: 'caixa' }, { name: 'Itau', code: 'itau' }, ...]  |

##### PixPayment

These are the input fields rendered and available in the object `Checkout.getData('form')`.

| Name             | Description                      | Required     | `fields` value       |
| ---------------- | ---------------------------------| ------------ | -------------------- |
| `holderName`     | Consumer's name.                 | Optional     | `holder_name`        |
| `holderIdNumber` | Consumer's identification (CPF). | Optional     | `holder_id_number`   |

#### PaymentOption Configuration Object and it's properties

All `PaymentOptions` functions take a configuration object. The generic properties of the configuration object for all `PaymentOptions` are:

| Name           | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `id`           | Must match the `id` set in the Payment Provider's `checkout_payment_options[i]`. |
| `name`         | Payment option display name.                                 |
| `fields`       | Object containing a propertires of extra input fields for transparent payment options and a boolean value to wither render it or not. |
| `scripts`      | List of external JavaScript files to be loaded before registering this method. |
| `onLoad`       | Function to be invoked after registering this method.        |
| `onDataChange` | Function to be invoked whenever there's a change in checkout data. |
| `onSubmit`     | Function to be invoked whenever the consumer clicks on "Finish checkout" and all mandatory fields are filled correctly. |

##### `fields` property

For each of the transparent payment options, the following extra input fields can be rendered if specified in this property.

###### CreditPayment

| Name                     | Description                               |
| ------------------------ | ----------------------------------------- |
| `bankList`               | Banks list.                               |
| `issuerList`             | Banks list. _(AR only)_                   |
| `card_holder_id_types`   | Card holder identification type selector. |
| `card_holder_id_number`  | Card holder identification number.        |
| `card_holder_birth_date` | Card holder birth date.                   |
| `card_holder_phone`      | Card holder number.                       |


###### DebitPayment

| Name        | Description |
| ----------- | ----------- |
| `bank_list` | Banks list. |

###### BoletoPayment

| Name                 | Description      |
| -------------------- | ---------------- |
| `boleto_holder_name` | Payer name.      |
| `boleto_id_number`   | Payer id number. |

> No including an input field on this object is enough to prevent it from rendering. It's not necessary to set it as `false`.

###### For all payment options

| Name              | Description          |
| ----------------- | -------------------- |
| `billing_address` | Billing information. |


> `PixPayment` doesn't render any input. The payment instructions are displayed after closing the order.

###### Updating the fields dinamically

```javascript
Checkout.updateFields({
	method: 'acme_transparent_card',
	value: {
		bankId: true
	}
});
```

##### onSubmit

The handler function for the `onSubmit` event property receives a callback function argument which must be invoked immediately after finishing the necessary requests to initiate the payment process.

The `callback` function must be invoked with an object containing the following properties.

| Name          | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| `success`     | If true, the checkout process continues and the order is completed. Otherwise, a customizable error message is shown to the consumer. |
| `error_code` | *(Optional)* If `success` is false, the specified error code will be used to provide the user with all the necessary information to allow them to, either correct the problem, or at least understand what went wrong to know what the correct action course is. See [Transaction Failure Codes](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/transaction.md#Transaction-Failure-Codes) and [Checkout Runtime Error Codes](#Checkout-Runtime-Error-Codes). |
| `message`     | _(Legacy)_ If `success` is false, this message will be displayed to the consumer. |
| `redirect`    | _(Optional)_ External URL to which the consumer will be redirected to continue the payment process. _(Only for `ExternalPayment()`)._ |


##### Sample Arguments

Here's an example summarizing all the definitions above.

```javascript
LoadCheckoutPaymentContext(function(Checkout, PaymentOptions) {

  var Custom = PaymentOptions.Transparent.CardPayment({
    id: 'acme_transparent_card',

    fields: {
      card_holder_birth_date: true
    },

    onLoad: function() {
      // Do something after the script loads.
      // Example: generate a token.
    },

    onDataChange: Checkout.utils.throttle(function() {
      // Do something when the input form data changes.
      // Data changed is already available on `Checkout.getData()`.
      // Example: update credit card installments when the order value changes.
    }, 700),
    
    onSubmit: function(callback) {
      // Do something when user submits the payment.
      callback({
          success: true, // Or false.
      });
    }
  });

    Checkout.addPaymentOption(Custom);
});
```

#### Installments

In order to offer installment's options you must update the object `data.installments` by calling `setInstallments`. This will update the select input for the installments options on the card information form. 

Each element of the list must be an object with the following fields.

| Name                | Description                      |
| ------------------- | -------------------------------- |
| `quantity`          | Number of installments.          |
| `installmentAmount` | Value of a single installment.   |
| `totalAmount`       | Total value of all installments. |
| `cft`               | (optional)[String] Total financial cost. |

##### Example

```js
Checkout.setInstallments(
   [
    {
      quantity: 1,
      installmentAmount: 25,
      totalAmount: 25,
      cft: '0,00%'
    },
    {
      quantity: 2,
      installmentAmount: 13,
      totalAmount: 26,
      cft: '199,26%'
    },
    {
      quantity: 3,
      installmentAmount: 10,
      totalAmount: 30,
      cft: '196,59%'
    }
  ]
);
```

## Appendix

### Checkout Runtime Error Codes

If for some reason the `onSubmit` event handler fails to execute the action expected by the payment option chosen by the user, together with a `success: false` callback parameter, a `error_code` must be specified in order to allow us to give the consumer clear information on what went wrong so the consumer understands what they need to do in order be able to fix the problem.

All [Transaction Failure Codes](https://github.com/TiendaNube/api-docs/blob/master/resources/transaction.md#transaction-failure-codes) are valid `error_codes`, plus the extra ones specified on the following list.

| Failure Code                | Description                                                  |
| --------------------------- | ------------------------------------------------------------ |
| `consumer_same_as_merchant` | The redirection fails because the consumer account is the same as the merchant account and merchants cannot pay themselves. |
| `server_error`              | There is a problem accessing the server which prevents either the execution of the payment or the generation of the redirect url. |
| `server_error_timeout`      | Same as `server_error` but the problem is due to a timeout condition. |
| `unknown_error`             | An unknown error occurred while trying to process the payment. |

