<!-- type: row -->

<!-- type: card 
description: The following guides provide information on CardSecure, tokens, and the various tokenization methods available to fit your business needs.
-->

<!-- type: row-end -->

# CardSecure Overview

CardSecure® is the CardPointe platform's P2PE-validated encryption and tokenization solution, and is at the center of all integrated payment solutions. Whether you use a [CardPointe Integrated Payments solution](https://cardconnect.com/cardpointe) to securely accept card-present transactions with your integrated point-of-sale, or the [Hosted iFrame Tokenizer](?path=docs/documentation/HostediFrameTokenizer.md) or [CardSecure API](?path=docs/APIs/CardSecureAPI.md) to extend secure eCommerce payments to online customers, CardSecure allows you to accept sensitive payment data securely, with reduced PCI scope.

# Understanding CardSecure Tokens

CardSecure's patented P2PE-validated tokenization solution offers numerous secure and convenient methods for tokenizing sensitive data. A token is a hashed alphanumeric representation of sensitive data such as a credit card account number. These tokens can only be used with the [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) for transaction processing.

When you submit a [tokenization request](../api/?type=post&path=/cardsecure/api/v1/ccn/tokenize), CardSecure encrypts and stores the sensitive data in a secure vault, and generates a unique token that corresponds to and represents the stored data. For example, if you capture and tokenize payment card data, the payment card number and associated track and EMV data (for card-present transactions) is stored in the database. You then use the token to represent this payment card in a transaction.

> CardSecure stores and purges sensitive data in accordance with Payment Card Industry (PCI) compliance standards. However, even after a token's sensitive data has been purged, CardSecure retains a hash of the clear card number, allowing the same token to be generated any time a given card is tokenized.

Consider the following information when handling tokens:

- Tokens never expire. In the event that a card is re-swiped or manually keyed on a terminal a device, the same token is generated for that card.
- Tokens can be alpha-numeric. The format of the token is driven by the specific key format used.
- Credit card tokens are typically 16-digit numeric tokens, where: 
  - The 1st digit is always 9.
  - The 2nd and 3rd digits represent the 1st and 2nd digits of the card number. You can use these to determine the card type, as follows:
    - 3X= Amex
    - 4X= Visa
    - 5X= Mastercard
    - 6X= Discover
  - The last 4 digits of the token represent the last 4 digits of the card.
- Electronic check (ACH) tokens are typically 16-digit numeric tokens, where: 
  - The 1st digit is always 9.
  - The 2nd and 3rd digits represent the 1st and 2nd digits of the routing number.
  - The last 4 digits of the token represent the last 4 digits of the account number.

# Tokenizing Using CardPointe Mobile SDKs

The CardPointe Mobile SDKs seamlessly connect your iOS and Android applications to CardSecure for tokenization of customer card data. Tokens and other associated payment details are then retrieved by your server and securely transmitted to the CardPointe Gateway for authorization, using a server-side REST client.

Integrating the CardPointe mobile SDKs can help reduce your PCI scope. The SDKs provide direct tokenization methods that pass your customers' sensitive card data to CardSecure without ever sending the clear or unencrypted card data to your server.

The token returned from this process is not considered card data; therefore, as the token moves between your client application and your application server, the token does not bring any of those systems or data paths into scope for PCI security controls.

## More Information

See the following guides for more information and integration details:

<!-- type: row -->

<!-- type: card 
title: <center> CardPointe Mobile SDKs </center>
description: <center> Provides an overview of the CardPointe Mobile SDKs solution for integrating secure payments in your mobile app </center>
link: ?path=docs/documentation/CardPointeMobileSDKs.md
-->

<!-- type: row-end -->

<!-- type: row -->

<!-- type: card
title: <center> CardPointe Mobile Andriod SDK Developer Guide </center>
description: <center> Provides information for integrating the CardPointe Mobile SDK with your Android app </center>
link: ?path=docs/documentation/CardPointeMobileAndroidSDKDeveloperGuide.md
-->

<!-- type: card
title: <center> CardPointe Mobile iOS SDK Developer Guide </center>
description: <center> Provides information for integrating the CardPointe Mobile SDK with your iOS app </center>
link: ?path=docs/documentation/CardPointeMobileiOSSDKDeveloperGuide.md
-->

<!-- type: row-end -->

# Tokenizing Closed Loop Gift Cards

CardSecure allows merchants the flexibility to accept their own proprietary, closed loop gift cards for customer purchases. If you want to include closed loop gift cards in your accepted payment methods, review the following information to get started.

## Gift Card Requirements

Before you begin, check with your gift card printer to verify that your gift cards meet the following requirements:
- Cards must include a magnetic-stripe to provide track 2 data.
  - The track data must include a 3-digit service code. 
    - The service code must **not** begin with 2 or 6.
    - We recommend service code 702 (7: No interchange except under bilateral agreement (closed loop) | 0: Normal | 2: Goods and services only (no cash)).
- The track data must include a 4-digit expiration date in the format MMYY.

The following example illustrates track data that meets these requirements:

```json
6765109987992410=122070225?
```

- Card numbers must pass Luhn algorithm verification.
- Card numbers must not start with 9.
- Card numbers must not be greater than 19 digits.
- You must provide a unique Bank Identification Number (BIN) range for your gift cards. This range must not overlap any other BIN ranges or conflict with any card brand BINs.

If you currently use gift cards that do not meet all of these requirements, or if they are in a different BIN range, you will not be able to accept those gift cards.

## Establishing a BIN Range

Before you can begin to accept gift card payments, you must provide the BIN range used to identify your gift cards. We use this information to whitelist the specific BIN range, allowing CardSecure to return unencrypted clear text data to your point-of-sale software.

Contact Support to get started.

<!-- theme: danger -->
> The BIN range that you provide must be unique, and must not overlap any other BIN range or conflict with any card brand BINs.

## Loading a Gift Card

When a customer purchases a gift card, the merchant initiates an authorization request using the customer's payment card. The authorization response data can then be used to update the merchant's ledger to include the transaction amount, the total amount added to the gift card, the quantity of gift cards, issued gift card numbers, and any additional identifying information for the transaction.

## Accepting a Payment

When a customer presents a gift card for payment, the payment request process is similar to accepting a credit or debit card payment. 

Depending on your business needs, you can use a [CardPointe Integrated Terminal](?path=docs/APIs/CardPointeIntegratedTerminalAPI.md) to accept card-present gift card payments in person, or you can integrate CardSecure to allow customers to use gift cards to make payments on your website or mobile application.

### Using a CardPointe Integrated Terminal

If you are using a [CardPointe Integrated Terminal](?path=docs/APIs/CardPointeIntegratedTerminalAPI.md) to process payments, you can use a [readCard](../api/?type=post&path=/api/v2/readCard) or [readManual](../api/?type=post&path=/api/v2/readManual) request to initiate a gift card transaction. When the gift card is swiped or manually entered at a terminal, the terminal matches the BIN with the range of BINs for your gift cards and returns an unencrypted clear text card number, which your point-of-sale software uses to complete the transaction.

### Using CardSecure

If your software directly integrates CardSecure, using the [CardSecure API](?path=docs/APIs/CardSecureAPI.md), [Hosted iFrame Tokenizer](?path=docs/documentation/HostediFrameTokenizer.md), or [CardPointe Mobile SDKs](?path=docs/documentation/CardPointeMobileSDKs.md), your software passes the gift card number to CardSecure, which matches the BIN with the whitelisted BIN range and returns the unencrypted card number, which your software uses to complete the transaction.

> Notes:
> - Unlike a credit or debit card transaction, CardSecure does not encrypt or tokenize the gift card account number.
> - In the event that the gift card balance is insufficient to cover the full amount of the transaction, your software will need to initiate a second request to complete the transaction using another payment method. 

## Managing Gift Card Transactions

Because these gift card transactions take place within a closed loop, no settlement or funding data is passed to an issuing bank. Instead, it is the merchant or partner's responsibility to manage gift card transaction and funding data. By integrating the [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) and [CardPointe Integrated Terminal API](?path=docs/APIs/CardPointeIntegratedTerminalAPI.md), your software can retrieve the data necessary for you to manage your general ledger, and to ensure that funds are transferred from the gift card to the merchant.

# Tokenizing Interactive Voice Response Input

This guide provides information for tokenizing data gathered using an interactive voice response (IVR) telephone system.

## Overview

Interactive voice response (IVR) systems are used to capture payment and personal information from customers using a telephone keypad or voice recognition. If your business uses an IVR system to capture sensitive data, you can integrate the [CardSecure API](?path=docs/APIs/CardSecureAPI.md) with your solution to securely encrypt and tokenize the data and the [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) to use the token to process a payment.

## Requirements

To tokenize data captured from an IVR system, your integration must meet the following requirements:

- You must obtain an RSA public key from CardPointe support.
  
  CardPointe support generates a unique RSA key pair, provides the public key (in X.509 format) to you and retains the private key. You use the public key to encrypt the IVR input   data, and CardSecure uses the private key to decrypt the data prior to tokenization. For more information, see [Encrypting and Tokenizing Payment Account Data](?path=docs/APIs/CardSecureAPI.md#encrypting-and-tokenizing-payment-account-data).
- Your authorization requests to the CardPointe Gateway must include the key value pair "ecomind" : "T" to specify that the payment source is "telephone."

## How it Works

The following workflow describes a typical process for capturing cardholder data with an IVR system, tokenizing the data, and processing a payment:

**1.** A customer calls in to your service to make a payment.
   
**2.** When prompted, the customer enters a credit card number, CVV code, and zip code using either Touch-Tone or voice recognition input.
   
**3.** Your IVR software captures the customer's input.
   
**4.** Your application uses an RSA encryption key to encrypt the data.
   
**5.** Your application makes a tokenization request, using the [CardSecure API](?path=docs/APIs/CardSecureAPI.md).
   
**6.** CardSecure generates a token that represents the customer's payment card data, and returns the encrypted token to your application.
   
**7.** Your application makes an authorization request, using the [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) and the token.
   
**8.** The CardPointe Gateway authorizes the payment and returns the authorization response to your application.
