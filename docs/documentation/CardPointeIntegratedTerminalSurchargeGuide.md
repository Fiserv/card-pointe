# CardPointe Integrated Terminal Surcharge Guide (Beta)
<!-- theme: warning -->
> This solution is currently in a closed beta, and not available for general use. Contact your account manager for more information

## Overview

Surcharging allows merchants to add a "% checkout fee" to a credit card transaction, paid by the cardholder, to help cover the merchant’s credit card processing fees.

The [Merchant Surcharge Program](https://support.cardpointe.com/compliance/surcharging) allows eligible merchants to add a surcharge on applicable credit card transactions. This guide provides an overview of the integration requirements for surcharging card-present credit card transactions using a CardPointe Integrated Terminal.

> Merchants must be enrolled in the Merchant Surcharge Program to take advantage of these features. See the [Merchant Surcharge Program Overview](https://support.cardpointe.com/compliance/surcharging) for detailed information and important requirements.

## How it Works

For merchants enrolled in the Merchant Surcharge Program, the CardPointe Gateway automatically assesses each transaction to determine its eligibility for applying a credit card surcharge, based on the following factors:

- The type of card presented
- The cardholder's postal code (card-not-present only)
- The Credit Card Surcharge Rate configured for the merchant account

If the Gateway determines that the card used is not a credit card (for example, a debit card), or that the postal code associated with the account is a surcharge-restricted location, then the surcharge is waived. Otherwise, the Credit Card Surcharge Rateconfigured for the merchant account is automatically added to the subtotal amount.

The CardPointe Integrated Terminal API has been updated with new request endpoints, to enable the terminal to display surcharging disclosures and optional itemized amount details to the cardholder. To enable these display features for your merchants enrolled in the Merchant Surcharge Program, you must update your integration to use the new payment endpoints.

> Surcharging is enabled at the merchant ID level; When a merchant ID is enrolled in the Merchant Surcharge Program, all terminals and transactions associated with that merchant ID will be subject to credit card surcharging.

## Requirements

Before you begin, ensure that you read and understand the following requirements.

### Disclaimers and Signage

You and your merchants are responsible for compliance with all state, local, and card brand requirements, including displaying disclaimers and signage to notify your customers that a credit card surcharge will be applied to applicable transactions. To make this easier, the new /v4 Terminal API payment endpoints include a surcharging disclosure and an optional itemized amount display.

### Receipts

If you use a Clover Flex or Mini CardPointe Integrated Terminal, the receipt printed by the terminal now includes surcharge line items to display the subtotal, surcharge, and total amounts, as well as a disclaimer. For example:

*To cover the cost of accepting credit cards, we collected a 3% credit card surcharge.*

If you generate your own receipts (for example if using an Ingenico terminal, then you must include line items to display the subtotal, surcharge, and total amounts. You must also include a surcharge disclaimer similar to the example above.

### Refunds

To ensure that you refund the correct amount to a cardholder, you **must** use the CardPointe Gateway API refund request, using the retrieval reference number (`retref`) from the original transaction. Using a negative amount in an authorization request (forced credit) can result in a mismatch if a surcharge is applied in either the authorization or refund, but waived in the other, due to different cards used in both transactions.

For example:

1. A customer makes a $100 purchase with a credit card, and a surcharge is applied, totaling $103.
2. The customer returns the purchase for a refund, and presents a debit card.
3. You process a negative authorization to refund the amount, and the surcharge is waived, returning $100 to the cardholder.

Using a refund with reference to perform a card-not-present refund to the original payment method ensures that they correct amount is refunded, even in the case of a partial authorization.

### Tips and Gratuity

If you accept tips and gratuity, you **must** select one of the following methods:

- **Tip at time of sale using the tip endpoint**

  When using the `/tip` endpoint, to accept a tip at the time of sale, the tip is included in the subtotal amount prior to the surcharge calculation, and this calculation is displayed to the cardholder on the terminal.

  <!-- theme: warning -->
  > To ensure a consistent and compliant cardholder experience, we strongly encourage that merchants **only** accept tips at the time of sale, when enrolled in the Merchant Surcharge Program.

- **Tip on receipt using a tip adjustment**

  When performing tip adjustments after an initial authorization, you must update your receipt template to include a disclaimer that a surcharge will be applied to credit card transactions.

  Additionally you must include the `fee_excluded_amount` field in your capture request to the CardPointe Gateway, specifying the tip amount, to exclude the tip amount from the surcharge calculation, to prevent an additional surcharge on the adjusted amount. 

  <!-- theme: warning -->
  > For example, include the following statement on your receipts:
  > *To cover the cost of accepting credit cards, we collected a 3% credit card surcharge. Tips will not be surcharged.*

### Terminal Devicea

Surcharging is supported for all Ingenico (/Lane and /Link) and Clover CardPointe Integrated Terminal Devices.

## API Changes

To leverage the terminal API updates to display surcharge disclaimers to your customers, you must update your integration to use the /v4 version of the following endpoints, as applicable:

- `/v4/authCard` - For card-present transactions, verify that the card is eligible for a surcharge and display a notification to the cardholder. 
- `/v4/readCard` - For card-present tokenizations, verify that the card is eligible for a surcharge and display a notification to the cardholder.
- `/v4/authManual` - For card-not-present (manually keyed) transactions, verify that the card is eligible for a surcharge and display a notification to the merchant.
- `/v4/readManual` - For card-not-present (manually keyed) tokenizations, verify that the card is eligible for a surcharge and display a notification to the merchant.
- `/v3/printReceipt` - For Clover devices, the `printReceipt` response and the printed receipt now include surcharge details, when applicable.

Prior to authorizing or capturing a payment, these v4 requests make additional calls to the CardPointe Gateway's `/inquireMerchant` and `/surcharge` endpoints to determine whether or not the merchant has surcharging enabled, and whether or not the card presented is eligible for surcharging. If both are true, then the terminal displays the subtotal, surcharge, and total amount. Additionally, these updated requests return additional fields in the response to your application.

The following topics describe these changes in greater detail.

### v4 Requests

Each v4 request maintains the same format as the corresponding v2 and v3 requests; all of the additional calls to the CardPointe Gateway to retrieve and calculate surcharge details are handled by the Terminal service.

The terminal displays the potential surcharge amount to the cardholder and, if `"confirmAmount":"true"`  is provided in the request, prompts the payer to accept the final sale amount after they have tapped, inserted, or swiped a card.

> In v2/v3 terminal interactions, the confirm amount prompt appears before the cardholder presents their card. For v4 interactions, the confirmation appears after the card is presented, to allow the terminal to display the final sale amount including a surcharge, if the card is eligible.

To update your integration, you must update your payment or tokenization endpoint request urls to:

`https://<site>.cardpointe.com/api/v4/<endpoint>`

Additionally, you might need to make the following changes, depending on your integration:
- `confirmAmount` - This parameter is set to `false` by default.

  For Ingenico terminals, specify true to prompt the cardholder to accept the final sale amount, including any applicable surcharge fee. This parameter is optional and intended for integrations that choose to provide an itemized summary of the surcharged transaction.
- `includeAmountDisplay` - This parameter is always set to `false`. A value of `true` in the request will be ignored.
- `amount` - This parameter is **required** for all v/4 endpoints.

### v4 Responses

Each v4 response includes additional fields to indicate whether a surcharge was applied to the transaction, and if so, the amount and other details.

The following table describes these new response fields:

| Field	| Max Length | Type | Destination |
| --- | --- | --- | --- |
| `fee_format`	| 7	| AN | The surcharge format configured for the merchant account. Always `"percent"` when a surcharge is successfully applied to the transaction. Empty (`""`) if the surcharge was waived or bypassed. |
| `fee_value`	| 3	| N	| The surcharge rate applied to the transaction, for example, `"300"` for a 3.0% surcharge, or `"0"` if the surcharge was waived or bypassed. |
| `fee_type`	| 13	| AN | The type of fee applied to the transaction. Always `"SURCHRG"` when a surcharge is successfully applied. `"SURCHRG_WAIVED"` if the surcharge was waived or bypassed. |
| `fee_amount`| 14	| N | The surcharge amount applied to the transaction, in dollars and cents. `"0.00"` if the surcharge was waived or bypassed. |
| `fee_authcode` | 6 | N | Duplicate of the `authcode` field. |
| `fee_merchid`	| -	| -	| 	Not applicable for surcharge transactions; returns an empty value (`""`). |
| `fee_retref`	| -	| -	| Not applicable for surcharge transactions; returns an empty value (`""`). |

#### Sample v4 authCard Response (Successful Surcharge)

```json
{
  "token": "9656535197710133",
  "expiry": "1223",
  "name": "",
  "batchid": "107",
  "retref": "306017241368",
  "avsresp": "Y",
  "respproc": "RPCT",
  "amount": "1.03",
  "resptext": "Approval",
  "authcode": "PPS558",
  "respcode": "000",
  "merchid": "496590513883",
  "cvvresp": "P",
  "respstat": "A",
  "emvTagData": "{\"TVR\":\"8000008000\",\"PIN\":\"None\",\"Signature\":\"false\",\"Mode\":\"Issuer\",\"Network Label\":\"DISCOVER\",\"TSI\":\"0800\",\"Application Preferred Name\":\"Discover CL\",\"AID\":\"A0000001523010\",\"IAD\":\"0115209000000000B000\",\"Entry method\":\"Contactless Chip\",\"Application Label\":\"DISCOVER CL\"}",
  "tokenExpired": true,
  "orderid": "NCC1701D",
  "entrymode": "Contactless",
  "bintype": "",
  "fee_format": "percent",
  "fee_value": "300",
  "fee_type": "SURCHRG",
  "fee_amount": "0.03",
  "fee_authcode": "PPS558",
  "fee_retref": "",
  "fee_merchid": "",
  "receiptData": {
        "dba": "MM-RPCT with Surcharge",
        "address1": "232 Forsyth Street Southwest",
        "address2": "Atlanta, GA",
        "phone": "6101234567",
        "header": "",
        "orderNote": "",
        "dateTime": "20231102112928",
        "items": "",
        "nameOnCard": "",
        "footer": ""
}
```
