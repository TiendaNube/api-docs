# Checkout SDK

This library allows you to customize the checkout, through the methods below

## Payments customization

Renders in the console the list of ids of active gateways in the store.
These ids can be used in the methods below to customize them

```javascript
window.SDKCheckout.getPaymentIds()
```

Change the title of the payment option

```javascript
window.SDKCheckout.changePaymentTitle({ id: 'gateway_redirect', value: 'New Title' })
```

Hide payment options

```javascript
window.SDKCheckout.hidePaymentOptions(['gateway_redirect', 'gateway_credit_card'])
```

Adds or changes discount and installment information for a gateway

```javascript
window.SDKCheckout.changePaymentBenefit({ id: 'gateway_credit_card', value: '12x sem juros' })
```

Adds extra information to the content of the external payment method

```javascript
window.SDKCheckout.addPaymentContentText({ id: 'gateway_redirect', value: 'lorem ipsum dolor sit amet' })
```

Hide installments from the user's picklist
Only works with transparent credit or debit card gateways

```javascript
window.SDKCheckout.hideInstallments({ id: 'gateway_credit_card', value: [3, 6] })
```

To reset customizations, just call the method one more time with empty string or empty array