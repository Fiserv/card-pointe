# CardPointe Integrated Terminal Surcharge Guide

## Overview

Surcharging allows merchants to add a "% checkout fee" to a credit card transaction, paid by the cardholder, to help cover the merchant’s credit card processing fees.

The [Merchant Surcharge Program](https://support.cardpointe.com/compliance/surcharging) allows eligible merchants to add a surcharge on applicable credit card transactions. This guide provides an overview of the integration requirements for surcharging card-present credit card transactions using a CardPointe Integrated Terminal.

> Merchants must be enrolled in the Merchant Surcharge Program to take advantage of these features. See the [Merchant Surcharge Program Overview](https://support.cardpointe.com/compliance/surcharging) for detailed information and important requirements.

## How it Works

For merchants enrolled in the Merchant Surcharge Program, the CardPointe Gateway automatically assesses each transaction to determine its eligibility for applying a credit card surcharge, based on the following factors:

- The type of card presented
- The cardholder's postal code (card-not-present only)

If the Gateway determines that the card used is not a credit card (for example, a debit card), or that the postal code associated with the account is a surcharge-restricted location, then the surcharge is waived. Otherwise, a 3% surcharge is automatically added to the subtotal amount.

The CardPointe Integrated Terminal API has been updated with new request endpoints, to enable the terminal to display surcharging disclosures and optional itemized amount details to the cardholder. To enable these display features for your merchants enrolled in the Merchant Surcharge Program, you must update your integration to use the new payment endpoints.

> Surcharging is enabled at the merchant ID level; When a merchant ID is enrolled in the Merchant Surcharge Program, all terminals and transactions associated with that merchant ID will be subject to credit card surcharging.

##Requirements

Before you begin, ensure that you read and understand the following requirements.

###Disclaimers and Signage

You and your merchants are responsible for compliance with all state, local, and card brand requirements, including displaying disclaimers and signage to notify your customers that a credit card surcharge will be applied to applicable transactions. To make this easier, the new /v4 Terminal API payment endpoints include a surcharging disclosure and an optional itemized amount display.

###Receipts

If you use a Clover Flex or Mini CardPointe Integrated Terminal, the receipt printed by the terminal now includes surcharge line items to display the subtotal, surcharge, and total amounts, as well as the following disclaimer:

*To cover the cost of accepting credit cards, we collected a 3% credit card surcharge.*

If you generate your own receipts (for example if using an Ingenico terminal, then you must include line items to display the subtotal, surcharge, and total amounts. You must also include a surcharge disclaimer similar to the example above.

###Refunds

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

###Terminal Devicea
