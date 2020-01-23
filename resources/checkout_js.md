# Checkout JS

Since there are a lot of possible flows for integrating a payment solution with our platform, we've decided to give the Payment Provider's app full control of how to handle the integration.

When you define a [Payment Provider](https://github.com/TiendaNube/api-docs/blob/payments-api-docs/resources/payment_provider.md) you must specify one or more JS files in the field `checkout_js_urls`. These scripts will be loaded during the checkout process and can interact with it through some primitives described below.

## Examples

### Redirect to External Checkout

Let's take a look at a simple script for a hypothetical integration with a Payment Provider that redirects the user to *'acmepayments.com'* to finish the purchase in their checkout. This is what we call a `redirect` checkout.

```js
LoadPaymentMethod(function(Checkout, Methods) {

  // Define an object to encapsulate the integration.
  
  var AcmeRedirect = new Methods.RedirectPayment({
    name: 'ACME Payments',
    
    // This function will be called when the consumer finishes the checkout flow so you can initiate the Transaction.
    
    onSubmit: function(callback) {
    
      // Let's imagine the app providies this endpoint to generate an ACME Payments checkout URL.
      
      Checkout.http.post('https://app.acmepayments.com/generate-checkout-url', Checkout.data)
        .then(function(response){
        
          // Now that we have the ACME Payments checkout, invoke the callback telling it we want to redirect to its URL.
          
          callback({
            success: true,
            redirect: response.redirect_url
          });
        })
        .catch(function(error){
        
          // Handle a potential error in the HTTP request.
          
          callback({
            success: false
          });
        });
    }
  });

  // Register the object in the checkout.
  
  Checkout.addMethod(AcmeRedirect);

});
```

### Credit Card Payment
This is a more complex example, since this is a richer interaction with more control over the user experience.

The entire flow happens without leaving the Tiendanube checkout, in what we call a `transparent` checkout. This type of payment renders a form where the consumer inputs their credit card information and with which you can interact. In this example, whenever the consumer inputs or changes the credit card number we fetch the first 6 digits and populate the list of available installments.

```js
LoadPaymentMethod(function(Checkout, Methods) {

  var currentTotalPrice = Checkout.data.order.cart.prices.total;
  var currencCreditCardBin = null;

  /**
   * First, we define some helper functions.
   */

  // Get credit card number from transparent form.
  
  var getCardNumber = function() {
    var cardNumber = '';

    if (Checkout.data.form.cardNumber) {
      cardNumber = Checkout.data.form.cardNumber.split(' ').join('');
    }
    return cardNumber;
  };

  // Get the first 6 digits from the credit card number.
  
  var getCardNumberBin = function() {
    return getCardNumber().substring(0, 6);
  };

  // Check whether the BIN (first 6 digits of the credit card number) has changed. If so, we'll want to update the available installments.
  
  var mustRefreshInstallments = function() {
    var creditCardBin = getCardNumberBin();

    var hasCreditCardBin = creditCardBin && creditCardBin.length >= 6;
    var hasPrice = Boolean(Checkout.data.totalPrice);
    var changedCreditCardBin = creditCardBin !== currencCreditCardBin;
    var changedPrice = Checkout.data.totalPrice !== currentTotalPrice;

    return (hasCreditCardBin && hasPrice) && (changedCreditCardBin || changedPrice);
  };

  // Update the list of installments available to the consumer.
  
  var refreshInstallments = function() {
  
    // Let's imagine the app provides this endpoint to obtain installments.
    
    Checkout.http.post('https://app.acmepayments.com/installments', {
      amount: Checkout.data.totalPrice,
      bin: getCardNumberBin()
    }).then(function(response) {
      Checkout.setCheckoutData({ installments: response });
    })
  }

  /**
   * Now, onto the integration flows.
   */

  // Define an object to encapsulate the integration.
  
  var AcmeTransparentCreditCard = new Methods.Transparent.CreditPayment({
    name: 'ACME Credit Payment',

    // This function will be called when the checkout data changes, such as the price or the value of the credit card form inputs.
    
    onDataChange: Checkout.utils.throttle(function() {
      if (mustRefreshInstallments()) {
        refreshInstallments()
      } else if (!getCardNumberBin()) {
        // Clear installments if customer remove credit card number
        Checkout.setCheckoutData({ installments: null });
      }
    }, 700),

    // This function will be called when the consumer finishes the checkout flow so you can initiate the Transaction.
    
    onSubmit: function(callback) {
    
      // Let's imagine the app provides this endpoint to process credit card payments.
      
      Checkout.http.post('https://app.acmepayments.com/charge', Checkout.data.form)
        .then(function(response) {
          if (response.success){
          
            // If the charge was successful, invoke the callback indicating we want to close order.
            
            callback({
              success: true,
              close: true,
              confirmed: true
            });
          } else {
            callback({
              success: false
            });
          }
        })
        .catch(function(error){
        
          // Handle a potential error in the HTTP request.
          
          callback({
            success: false
          });
        });
    }
  });

  // Register the object in the checkout.
  
  Checkout.addMethod(AcmeTransparentCreditCard);

});
```

## Checkout

The `LoadPaymentMethod` function takes a callback with two arguments, `Checkout` and `Methods`. Here's what's available in each of them.

| Name             | Description                                                                                |
| ---------------- | ------------------------------------------------------------------------------------------ |
| `addMethod`        | Register the integration in the checkout.                                                  |
| `setCheckoutData`  | **TODO: Replace this with something like `setInstallments`.**                                    |
| `data`             | Object containing the data of the shopping cart, the consumer and more. See [Data](#Data). |
| `http`             | Function to perform AJAX requests. See [HTTP](#HTTP).                                                         |
| `utils`            | Collection of helper functions. See [Utils](#Utils).                                     |

### HTTP

This function allows you to easily make an HTTP request to your app. The response is a Promise.

```js
Checkout.http.post('https://acmepayments.com/charge', {
  cartId: cartId,
}).then(function(response) {
  // Do something with response
});
```

### Utils

**TODO: Document these**

- **Throttle**  
- **LoadScript**  
- **FlattenObject**

### Data
Here's an example of the data available in this object.

**TODO: Simplify this, we don't need to expose everything.**

```js
data: {
    form: {},
    order: {
      cart: {
        id: 139439691,
        hash: '1134e8f7a55f926991b5086f52815977eb11f789',
        number: null,
        prices: {
          shipping: 0,
          discount_gateway: 0,
          discount_coupon: 0,
          discount_promotion: 0,
          discount_coupon_and_promotions: 0,
          subtotal_with_promotions_and_coupon_applied: 20,
          subtotal: 20,
          total: 20,
          total_usd: 0
        },
        lineItems: [
          {
            id: 159519581,
            name: 'Camisa preta',
            price: '20.00',
            quantity: 1,
            free_shipping: false,
            product_id: 27177360,
            variant_id: 63612746,
            thumbnail: '//d26lpennugtm8s.cloudfront.net/stores/781/091/products/camisa-preta1-27bb549540fd46599815284694977523-100-0.jpg',
            variant_values: '',
            sku: '56868',
            properties: [],
            url: 'https://valmirarproduction.mitiendanube.com/productos/camisa-preta/?variant=63612746'
          }
        ],
        currency: 'ARS',
        currencyFormat: {
          'short': '$%s',
          'long': '$%s ARS'
        },
        lang: 'es',
        langCode: 'es_AR',
        coupon: {
          id: 451625,
          code: 'FREESHIPPING',
          type: 'shipping',
          value: '10.00',
          valid: true,
          used: 3,
          max_uses: null,
          start_date: null,
          end_date: null,
          min_price: null,
          categories: null
        },
        shipping: {
          type: 'ship',
          method: 'correo-argentino',
          option: '1',
          branch: null,
          disabled: null,
          raw_name: 'Correo Argentino - Encomienda Clásica',
          suboption: null
        },
        status: {
          order: 'open',
          order_cancellation_reason: null,
          fulfillment: 'unpacked',
          payment: 'pending'
        },
        completedAt: null
      },
      promotionalDiscount: {
        id: null,
        store_id: 781091,
        order_id: 139439691,
        created_at: '2019-12-13T13:31:05+0000',
        total_discount_amount: '0.00',
        contents: [],
        promotions_applied: []
      },
      defaultAddressId: 17670513,
      addresses: [],
      contact: {
        email: 'comprador01@mailinator.com',
        name: 'João Cesar',
        phone: '31912345678'
      },
      shippingAddress: {
        zipcode: '4652',
        first_name: 'João',
        last_name: 'Cesar',
        address: 'Miguel Perrela',
        number: '23',
        floor: '',
        locality: '',
        city: 'Buenos Aires',
        state: 'Salta',
        country: 'AR',
        phone: '31912345678',
        between_streets: '',
        reference: '',
        id_number: '213'
      },
      billingAddress: {
        zipcode: '4652',
        first_name: 'João',
        last_name: 'Cesar',
        address: 'Miguel Perrela',
        number: '23',
        floor: '',
        locality: '',
        city: 'Buenos Aires',
        state: 'Salta',
        country: 'AR',
        phone: '31912345678',
        between_streets: '',
        reference: '',
        id_number: '213'
      },
      customer: {
        id: 29056585,
        first_name: 'João',
        last_name: 'Cesar',
        email: 'comprador01@mailinator.com',
        id_number: '10073734667',
        phone: '31912345678',
        default_address_id: 17670513,
        addresses: [
          {
            id: 17670513,
            first_name: 'João',
            last_name: 'Cesar',
            address: 'Miguel perrela',
            city: 'Buenos Aires',
            country: 'AR',
            created_at: '2019-12-11T13:39:04+0000',
            'default': true,
            floor: '',
            locality: '',
            number: '23',
            phone: '31912345678',
            state: 'Salta',
            updated_at: '2019-12-11T13:39:04+0000',
            zipcode: '4652'
          }
        ],
        has_password: true
      },
      payment: {
        option: null,
        category: null,
        boleto_url: null,
        went_to_gateway: false,
        brand: null,
        logo: null,
        external_id: null
      },
      orderStatus: {
        shipping_eta: '2019-12-23',
        shipping_tracking_code: null,
        pickup_location: {
          name: null,
          address: null,
          city: null,
          province: null
        },
        pickup_hours: [],
        payment_state: 'pending',
        payment_state_reason: null,
        shipping_extra: {
          show_time: true,
          id_required: false,
          shippable: true,
          phone_required: false
        },
        history: [
          {
            type: 'finished_checkout',
            timestamp: null
          },
          {
            type: 'payment_confirmed',
            timestamp: null
          },
          {
            type: 'packed',
            timestamp: null
          },
          {
            type: 'shipped',
            timestamp: null
          }
        ]
      },
      storeId: 781091,
      conversionCode: null,
      conversionCodeGatewayRedirect: null,
      purchaseNotifications: {
        analytics: false,
        fb_pixel: false
      },
      checkout: 'micro-service',
      theme: 'amazonas',
      abTests: {
        zipcode_validation: 'a',
        new_payment_screen: 'b'
      },
      loaded: true
    },
    country: 'AR',
    totalPrice: 20
  }
```

## Methods

This is the second argument to the callback in `LoadPaymentMethod`. It contains the different possible integration flows, the difference among each of them being the UI that is rendered and the data that's available to you as `Checkout.data.form`.

| Name            | Description                                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------------------------ |
| `CustomPayment`   | **TODO What would this be? Is there a use case for it in PAPI?**                                                 |
| `RedirectPayment` | For integration flows that redirect the consumer to a different website.                                     |
| `ModalPayment`    | For integration flows that open a modal with an iframe.                                                      |
| `Transparent`     | For integration flows that render a form and integrate mostly through js to maintain the UX of the checkout. |

### RedirectPayment, ModalPayment and CustomPayment

These flows don't provide any inputs. The difference is how they're rendered in the checkout.

### Transparent

There're three options availalbe in `Methods.Transparent`.

| Name           | Description                                                           |
| -------------- | --------------------------------------------------------------------- |
| `CreditPayment`  | For credit or debit card payments.                                    |
| `DebitPayment`   | For redirecting the consumer to the bank's system for debit payments. |
| `OfflinePayment` | For boleto or ticket type payments.                                   |

#### CreditPayment

These are the fields rendered and available through `Checkout.data.form`.

| Name                | Description                                     |
| ------------------- | ----------------------------------------------- |
| `brand`               | Type of card: `visa`, `mastercard`, etc.            |
| `cardNumber`          | Card number.                                     |
| `cardHolderName`      | Card holder's name.                              |
| `cardExpiration`      | Card's expiration date in `mm/yy` format.        |
| `cardCvv`             | Card's verification code.                        |
| `cardInstallments`    | Number of installments selected by the consumer. |
| `cardHolderIdType`    | **TODO How to use this?**                           |
| `cardHolderIdNumber`  | Card holder's CPF, DNI or equivalent.            |
| `cardHolderBirthDate` | Card holder's birthday in `dd/mm/yy` format.     |
| `cardHolderPhone`     | Card holder's phone number.                      |
| `bankId`              | Card's issuing bank.                             |
| `issuerId`            | **TODO What is this?**                              |

#### DebitPayment

These are the fields rendered and available through `Checkout.data.form`.

| Name           | Description                              |
| -------------- | ---------------------------------------- |
| `bank`           | Bank to debit from.                       |
| `holderName`     | Account holder's name.                    |
| `holderIdNumber` | Account holder's CPF, DNI, or equivalent. |

#### OfflinePayment

These are the fields rendered and available through `Checkout.data.form`.

| Name           | Description                        |
| -------------- | ---------------------------------- |
| `holderName`     | Consumer's name.                    |
| `holderIdNumber` | Consumer's CPF, DNI, or equivalent. |

### Arguments

All of the instances of `Methods` take can take the following arguments.

| Name         | Description                                                                                                             |
| ------------ | ----------------------------------------------------------------------------------------------------------------------- |
| `name`         | Method name.                                                                                                            |
| `scripts`      | List of external js files to be loaded before registering this method.                                                  |
| `onLoad`       | Function to be invoked after registering this method.                                                                   |
| `onDataChange` | Function to be invoked whenever there's a change in `Checkout.data`.                                                    |
| `onSubmit`     | Function to be invoked whenever the consumer clicks on "Finish checkout" and all mandatory fields are filled correctly. |

#### OnSubmit
The function passed to `onSubmit` received a parameter called `callback`, which must be invoked to return control to the checkout flow with information regarding the success of the payment attempt.

This `callback` function can be invoked with an object containing the following attributes.

| Name            | Description                                                                                                                                |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `success`         | If true, the checkout process continues and the order is complete. Otherwise, shown a customizable error message to the consumer.          |
| `message`         | If `success` is false, this message will be displayed to the consumer.                                                                     |
| `redirect`        | External url for the consumer to be redirected to in order to finish the purchase in an external checkout.                                 |
| `close`           | **TODO The Transactions endpoint already solves this.**                                                                                        |
| `confirmed`       | **TODO What does this mean? What's the difference between sending true and false? Sounds like the Transactions endpoint already solves this.** |
| `recovered`       | **TODO What is this? Sounds like it shouldn't be an argument.**                                                                                |
| `extraBoletoUrl`  | **TODO The Transactions endpoint already solves this.**                                                                                        |
| `extraAuthorized` | **TODO Don't know what this is but it sounds like the Transactions endpoint already solves this.**                                             |
| `skipRedirect`    | **TODO Isn't this the same as not providing `redirect`?**                                                                                      |

#### Sample Arguments

Here's an example summarizing all the definitions above.

```js
LoadPaymentMethod(function (Checkout, Methods) {

	var Custom =  new Methods.CustomPayment({
		name: 'custom',
		scripts: 'https://meuscriptonline.com/payment.js',
		onLoad: function() {
			// Do something after the script loads
			// Example: generate a token
		},
		onDataChange: Checkout.utils.throttle(function() {
			// Do something when data change
			// Data changed is already available on `Checkout.data`
			// Example: update credit card installments when order value change
		}, 700),
		onSubmit: function(callback) {
			// Do something when user submit payment
			callback({
				success: true/false,
				...
			})
		}
	});

	Checkout.addMethod(Custom);
	
});
```

## Installments

**TODO We'll want to revise this and replace it with a function `Checkout.setInstallments`.**

In order to offer installment options you must update the object `data.installments` by calling `setCheckoutData`. For example:

```js
Checkout.setCheckoutData({ installments: [] })
```

Each element of the list must be an object with the following fields.

| Name              | Description                     |
| ----------------- | ------------------------------- |
| `quantity`          | Number of installments.          |
| `installmentAmount` | Value of a single installment.   |
| `totalAmount`       | Total value of all installments. |

### Example

```js
[
	{
		quantity: 1,
		installmentAmount: 25,
		totalAmount: 25
	}, {
		quantity: 2,
		installmentAmount: 13,
		totalAmount: 26
	}, {
		quantity: 3,
		installmentAmount: 10,
		totalAmount: 30
	}
]
```
