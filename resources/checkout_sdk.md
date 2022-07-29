# Checkout SDK

This library allows you to customize the checkout, through the methods below

## Payments customization

### Renders in the console the list of ids of active gateways in the store.
> These ids can be used in the methods below to customize them

```javascript
window.SDKCheckout.getPaymentIds()
```

Example:
```javascript
['mercadopago_transparent_card', 'pagseguro_transparent_debit', 'mercadopago_transparent_offline', 'mercadopago_transparent_pix', 'custom', 'pagseguro_redirect', 'cielo_redirect', 'mercadopago_redirect', 'ame_digital']
```

### Change the title of the payment option

```javascript
window.SDKCheckout.changePaymentTitle({ id: '{{gateway_id}}', value: 'New Title' })
```

### Hide payment options
> An array with the ids of the gateways you want to hide

```javascript
window.SDKCheckout.hidePaymentOptions(['{{gateway_id}}', '{{gateway_id}}'])
```

### Adds or changes discount and installment information for a gateway

```javascript
window.SDKCheckout.changePaymentBenefit({ id: '{{gateway_id}}', value: '12x sem juros' })
```

### Adds extra information to the content of the external payment method
> Only works with external gateways

```javascript
window.SDKCheckout.addPaymentContentText({ id: '{{gateway_id}}', value: 'lorem ipsum dolor sit amet' })
```

### Hide installments from the user's picklist
> Only works with transparent credit or debit card gateways

```javascript
window.SDKCheckout.hideInstallments({ id: '{{gateway_id}}', value: [3, 6] })
```

> To reset customizations, just call the method one more time with empty string or empty array