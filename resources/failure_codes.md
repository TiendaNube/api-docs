Transaction Failure Codes
================

The following lists contains all the possible failures of a Transaction associated with a Payment Provider.

### Consumer

| Code                           |
| ------------------------------ |
| consumer_city_invalid          |
| consumer_country_invalid       |
| consumer_district_invalid      |
| consumer_email_invalid         |
| consumer_firstname_invalid     |
| consumer_floor_invalid         |
| consumer_id_invalid            |
| consumer_id_type_invalid       |
| consumer_id_value_invalid      |
| consumer_lastname_invalid      |
| consumer_phone_invalid         |
| consumer_province_invalid      |
| consumer_region_invalid        |
| consumer_state_invalid         |
| consumer_street_invalid        |
| consumer_street_number_invalid |
| consumer_zip_invalid           |

### Payment Method

#### General

| Code                      |
| ------------------------- |
| payment_method_id_invalid |

#### Bank Debit

| Code                              |
| --------------------------------- |
| bank_debit_bank_invalid           |
| bank_debit_method_unavailable     |
| bank_debit_payer_id_type_invalid  |
| bank_debit_payer_id_value_invalid |
| bank_debit_payer_name_invalid     |

#### Boleto

| Code                          |
| ----------------------------- |
| boleto_method_unavailable     |
| boleto_payer_id_type_invalid  |
| boleto_payer_id_value_invalid |
| boleto_payer_name_invalid     |

#### Card

| Code                               |
| ---------------------------------- |
| card_cvv_invalid                   |
| card_expiration_date_invalid       |
| card_holder_birthdate_invalid      |
| card_holder_id_type_invalid        |
| card_holder_id_value_invalid       |
| card_holder_name_invalid           |
| card_holder_phone_invalid          |
| card_info_invalid                  |
| card_issuer_invalid                |
| card_method_unavailable            |
| card_number_invalid                |
| card_rejected                      |
| card_rejected_deny_list            |
| card_rejected_disabled             |
| card_rejected_duplicated_payment   |
| card_rejected_fraud_high_risk      |
| card_rejected_insufficient_founds  |
| card_rejected_invalid_installments |
| card_rejected_max_attemps          |

#### Ticket

| Code                      |
| ------------------------- |
| ticket_method_unavailable |
| ticket_operator_invalid   |

### Shipping

| Code                           |
| ------------------------------ |
| shipping_city_invalid          |
| shipping_district_invalid      |
| shipping_email_invalid         |
| shipping_firstname_invalid     |
| shipping_floor_invalid         |
| shipping_lastname_invalid      |
| shipping_method_invalid        |
| shipping_method_unavailable    |
| shipping_phone_invalid         |
| shipping_province_invalid      |
| shipping_region_invalid        |
| shipping_state_invalid         |
| shipping_street_invalid        |
| shipping_street_number_invalid |
| shipping_total_curreny_invalid |
| shipping_total_value_invalid   |
| shipping_zip_invalid           |

### Order

| Code                           |
| ------------------------------ |
| line_items_currency_invalid    |
| line_items_description_invalid |
| line_items_quantity_invalid    |
| line_items_value_invalid       |
| order_total_currency_invalid   |
| order_total_value_invalid      |
| order_total_value_too_small    |

### Credentials

| Code                         |
| ---------------------------- |
| consumer_invalid_credentials |
| merchant_invalid_credentials |
