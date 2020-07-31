# Checkout

## Payment Options (Javascript Interface)

Our Checkout flow offers different Payment Options that provides consumers with the means to pay for an order with the payment method of their choice. Payments App developers can create their own Payment Options.

Payment Options configuration params are set via our [Payment Provider API](./payment_provider.md) by adding [`checkout_options`](./payment_provider.md#Checkout-Options) to the created Payment Provider.

Our Checkout triggers a variety of events for which we provide a JavaScript API that allows you to handle these events freely to initiate the payment process. Hence, you can implement their _(most likely)_ already existing and widely tested Javascript SDKs.

The file with the handlers implemented for the different options should be hosted on a CDN that must be capable of handling high traffic loads. The URL to this file must be stated in the [Payment Provider REST API](./payment_provider.md) `checkout_js_url` property.

## Examples

### External Payment Option

Let's take a look at a simple script for a hypothetical integration with a Payment Provider that redirects the user to *'acmepayments.com'* to finish the purchase in their checkout. This is what we call a `redirect` checkout.

```javascript
// File: AcmeExternalPaymentOption.js

// Call 'LoadCheckoutPaymentContext' method and pass a function as parameter
// to get access to the Checkout context and the Payment Options object.
LoadCheckoutPaymentContext(function(Checkout, PaymentOptions) {

    // Create a new instance of external Payment Option and set its properties.
    var AcmeExternalPaymentOption = PaymentOptions.ExternalPayment({

        // Set the option's unique id as it is configured on the Payment Provider so Checkout can relate them.
        id: 'acme_redirect',

        // This function handles the order submission event.
        onSubmit: function(callback) {

            // Gather the minimum required information.
            let acmeRelevantData = {
                // You should include all the relevant data here.
                orderId: Checkout.data.order.cart.id,
                paymentProviderId: Checkout.data.payment_provider_id,
                currency: Checkout.data.order.cart.currency,
                total: Checkout.data.order.cart.prices.total
            }

            // Use the Checkout HTTP library to post a request to our server and fetch the redirect URL.
            Checkout.http
                .post('https://app.acme.com/generate-checkout-url', {
                    data: acmeRelevantData
                })
                .then(response => response.json())
                .then(function(responseBody) {

                    // Once you get the redirect URL, invoke the callback by passing it as argument.
                    callback({
                        success: true,
                        redirect: responseBody.redirect_url
                    });
                })
                .catch(function(error) {
                    // Handle a potential error in the HTTP request.
                    callback({
                        success: false,
                      	message: 'Some error description to show to the consumer'
                    });
                });
        }
    })

    // Finally, add the Payment Option to the Checkout object so it can be
    // render according to the configuration set on the Payment Provider.
    Checkout.addPaymentOption(AcmeExternalPaymentOption);
});
```

### Card Payment Option

This is a more complex example, since this is a richer interaction with more control over the user experience. The entire flow happens without leaving the Tiendanube checkout, in what we call a `transparent` checkout.

This type of payment renders a form where the consumer inputs their credit card information and with which you can interact.

In this example, whenever the consumer inputs or changes the credit card number we fetch the first 6 digits and populate the list of available installments.

```javascript
// AcmePaymentsCardMethod.js
LoadCheckoutPaymentContext(function(Checkout, PaymentOptions) {

    var currentTotalPrice = Checkout.data.order.cart.prices.total;
    var currencCardBin = null;

    // SOME HELPER FUNCTIONS

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

    // Check whether the BIN (first 6 digits of the credit card number) has changed.
    // If so, we intend to update the available installments.
    var mustRefreshInstallments = function() {
        var cardBin = getCardNumberBin();
        var hasCardBin = cardBin && cardBin.length >= 6;
        var hasPrice = Boolean(Checkout.data.totalPrice);
        var changedCardBin = cardBin !== currencCardBin;
        var changedPrice = Checkout.data.totalPrice !== currentTotalPrice;
        return (hasCardBin && hasPrice) && (changedCardBin || changedPrice);
    };

    // Update the list of installments available to the consumer.
    var refreshInstallments = function() {

        // Let's imagine the app provides this endpoint to obtain installments.
        Checkout.http.post('https://app.acmepayments.com/card/installments', {
            amount: Checkout.data.totalPrice,
            bin: getCardNumberBin()
        }).then(function(response) {
            Checkout.setCheckoutData(response.installments);
        });
    };

    // Create a new instance of card Payment Option and set its properties.
    var AcmeCardPaymentOption = PaymentOptions.Transparent.CardPayment({

        // Set the option's unique id as it is configured on the Payment Provider so Checkout can relate them.
        id: "acme_transparent_card",

        // Event handler for form field input.
        onDataChange: Checkout.utils.throttle(function() {
            if (mustRefreshInstallments()) {
                refreshInstallments()
            } else if (!getCardNumberBin()) {
                // Clear installments if customer remove credit card number.
                Checkout.setCheckoutData(null);
            }
        }),

        onSubmit: function(callback) {
            // Gather the minimum required information.
            var acmeCardRelevantData = {
                orderId: Checkout.data.order.cart.id,
                currency: Checkout.data.order.cart.currency,
                total: Checkout.data.order.cart.prices.total,
                card: {
                    number: Checkout.data.form.cardNumber,
                    name: Checkout.data.form.cardHolderName,
                    expiration: Checkout.data.form.cardExpiration,
                    cvv: Checkout.data.form.cardCvv,
                    installments: Checkout.data.form.cardInstallments
                }
            }
            // Let's imagine the app provides this endpoint to process credit card payments.
            Checkout.http.post('https://app.acmepayments.com/charge', acmeCardRelevantData)
                .then(function(response) {
                    if (response.success) {
                        // If the charge was successful, invoke the callback to indicate you want to close order.
                        callback({
                            success: true
                        });
                    } else {
                        callback({
                            success: false,
                            message: 'Some error description to show to the consumer'
                        });
                    }
                })
                .catch(function(error) {
                    // Handle a potential error in the HTTP request.
                    callback({
                        success: false,
                      	message: 'Some error description to show to the consumer'
                    });
                });
        }
    })

    // Finally, add the Payment Option to the Checkout object so it can be
    // render according to the configuration set on the Payment Provider.
    Checkout.addPaymentOption(AcmeCardMethod);
});
```

## Checkout Context

The `LoadPaymentMethod` function takes a callback with two arguments, `Checkout` and `Methods`. Here is what's available in each of them.

| Name               | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| `addPaymentOption` | Register the option so the checkout can inject the configuration params and render it. |
| `setInstallments`  | Update the attributes of the `data.installments` object. See [Installments](#Installments). |
| `data`             | Object containing the data of the shopping cart, the consumer and more. See [Data](#Data). |
| `http`             | Function to perform AJAX requests. See [HTTP](#HTTP).        |
| `utils`            | Collection of helper functions. See [Utils](#Utils).         |

### HTTP

This function allows you to easily make an HTTP request to your app. The response is a Promise.

```js
Checkout.http.post('https://acmepayments.com/charge', {
    cartId: cartId,
}).then(function(response) {
    // Do something with the response.
});
```

### Utils

- **Throttle**  
- **LoadScript**  
- **FlattenObject**

### Data

Here's an example of the data available in this object.

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
            lineItems: [{
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
            }],
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
            addresses: [{
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
            }],
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
            history: [{
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

This is the second argument of the callback in `LoadCheckoutPaymentContext`. It contains the different possible integration flows. The difference among each of them being the UI that is rendered and the data that's available to you as `Checkout.data.form`.

| Name              | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| `ExternalPayment` | For integration flows that redirect the consumer to a different website. |
| `ModalPayment`    | For integration flows that open a modal with an iframe.      |
| `Transparent`     | For integration flows that render a form and integrate mostly through JavaScript to maintain the UX of the checkout. |

### RedirectPayment, ModalPayment and CustomPayment

These flows don't provide any inputs. They differ in how they're rendered in the checkout.

### Transparent

There're three options availalbe in `PaymentOptions.Transparent`.

| Name            | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| `CardPayment`   | For credit or debit card payments.                           |
| `DebitPayment`  | For redirecting the consumer to the bank's system for bank debit payments. |
| `BoletoPayment` | For payments with Boleto.                                    |
| `TicketPayment` | For payments with Ticket.                                    |

#### CardPayment

These are the fields rendered and available through `Checkout.data.form`.

| Name                  | Description                                            |
| --------------------- | ------------------------------------------------------ |
| `brand`               | Card brand. E.g. `visa`, `master`, etc.                |
| `cardNumber`          | Card number.                                           |
| `cardHolderName`      | Card holder's name.                                    |
| `cardExpiration`      | Card's expiration date in `mm/yy` format.              |
| `cardCvv`             | Card's verification code.                              |
| `cardInstallments`    | Number of installments selected by the consumer.       |
| `cardHolderIdNumber`  | Card holder's identification (CPF, DNI or equivalent). |
| `cardHolderBirthDate` | Card holder's birthday in `dd/mm/yy` format.           |
| `cardHolderPhone`     | Card holder's phone number.                            |
| `bankId`              | Card's issuing bank.                                   |

#### DebitPayment

These are the fields rendered and available through `Checkout.data.form`.

| Name             | Description                                                |
| ---------------- | ---------------------------------------------------------- |
| `bank`           | Bank to debit from.                                        |
| `holderName`     | Account holder's name.                                     |
| `holderIdNumber` | Account holder's identification (CPF, DNI, or equivalent). |

#### BoletoPayment

These are the fields rendered and available through `Checkout.data.form`.

| Name             | Description                                          |
| ---------------- | ---------------------------------------------------- |
| `holderName`     | Consumer's name.                                     |
| `holderIdNumber` | Consumer's identification (CPF, CNPJ or equivalent). |

#### TicketPayment

These are the fields rendered and available through `Checkout.data.form`.

| Name             | Description                                          |
| ---------------- | ---------------------------------------------------- |
| `holderName`     | Consumer's name.                                     |
| `holderIdNumber` | Consumer's identification (DNI, CUIT or equivalent). |

### Arguments

All of the instances of `PaymentOptions` can take the following arguments.

| Name           | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `name`         | Method name.                                                 |
| `scripts`      | List of external JavaScript files to be loaded before registering this method. |
| `onLoad`       | Function to be invoked after registering this method.        |
| `onDataChange` | Function to be invoked whenever there's a change in `Checkout.data`. |
| `onSubmit`     | Function to be invoked whenever the consumer clicks on "Finish checkout" and all mandatory fields are filled correctly. |

#### onSubmit

The function passed to `onSubmit` received a parameter called `callback`, which must be invoked to return control to the checkout flow with information regarding the success of the payment attempt.

This `callback` function can be invoked with an object containing the following attributes.

| Name       | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| `success`  | If true, the checkout process continues and the order is completed. Otherwise, a customizable error message is shown to the consumer. |
| `message`  | If `success` is false, this message will be displayed to the consumer. |
| `redirect` | External URL for the consumer to be redirected to in order to finish the purchase in an external checkout. |

#### Sample Arguments

Here's an example summarizing all the definitions above.

```javascript
LoadCheckoutPaymentContext(function(Checkout, PaymentOptions) {

    var Custom = new PaymentOptions.CustomPayment({
        id: 'acme_custom',

        onLoad: function() {
            // Do something after the script loads.
            // Example: generate a token.
        },
        onDataChange: Checkout.utils.throttle(function() {
            // Do something when data change.
            // Data changed is already available on `Checkout.data`.
            // Example: update credit card installments when the order value changes.
        }, 700),
        onSubmit: function(callback) {
            // Do something when user submits the payment.
            callback({
                success: true, // Or false.
                ...
            })
        }
    });

    Checkout.addPaymentOption(Custom);
});
```

## Installments

In order to offer installment options you must update the object `data.installments` by calling `setInstallments`. For example:

```js
Checkout.setInstallments({
    installments: []
})
```

Each element of the list must be an object with the following fields.

| Name                | Description                      |
| ------------------- | -------------------------------- |
| `quantity`          | Number of installments.          |
| `installmentAmount` | Value of a single installment.   |
| `totalAmount`       | Total value of all installments. |

### Example

```js
[{
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
}]
```
