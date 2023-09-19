# CardSecure API Overview

CardSecure is the CardPointe Gateway's sensitive data encryption and tokenization service. CardSecure allows you to securely accept and tokenize payment card, ACH (eCheck), and mobile wallet data to ensure the safety of your customers' sensitive payment data.

This guide provides information for developers looking to integrate CardSecure's tokenization and encryption methods using the CardSecure API. For information on integrating CardSecure with your iOS or Android mobile application, see [Tokenizing Using the CardPointe Mobile SDKs](?path=docs/documentation/CardSecure.md#tokenizing-using-cardpointe-mobile-sdks).

# What's New?

<!-- theme: warning -->
> Visit [Statuspage](https://status.cardconnect.com/) and click **subscribe to updates** to receive important release and status notifications.

## Date Updated: 7/24/2021

An update to CardSecure was deployed to the UAT environment on 7/19/2021 and to the production environment on 7/24/2021.

This release includes internal fixes and updates to CardSecure, including the following enhancement:

### Enhanced Apple Pay Support

When tokenizing an Apple Pay payload, CardSecure no longer requires a specific order for the Apple Pay parameters in the `devicedata` string, with the exception of the `data` value, which must be the first parameter in the string. Previously, CardSecure required a specific sequence for all Apple Pay parameters in the `devicedata` string  to correctly parse the string.

See the [Apple Pay Developer Guide](?path=docs/documentation/ApplePayDeveloperGuide.md) for detailed information on integrating Apple Pay support.

## Date Updated: 2/6/2020 

This release includes the following updates:

### Apple Pay Support

The CardSecure API now supports tokenizing Apple Pay payment tokens. Previously, this was only supported using the legacy API HTTP methods. 

To tokenize Apple Pay payment tokens, make a request to the `tokenize` endpoint using the Apple Pay payment token parameters in the `devicedata` field, and specify `EC_APPLE_PAY` in the `encryptionhandler` field, as follows:

```
POST /cardsecure/api/v1/ccn/tokenize HTTP/1.1
Content-Type: application/json

{
  "devicedata" : "<Apple Pay payload>",
  "encryptionhandler" : "EC_APPLE_PAY"
}
```

See the [tokenize](#tokenize) description for a complete example, and see the [Apple Pay Developer Guide](?path=docs/documentation/ApplePayDeveloperGuide.md) for detailed information on integrating support for Apple Pay, and formatting the payment token string for CardSecure. 

### Update a Token with CVV and Expiry

The CardSecure API now supports the ability to update a payment card token to include the CVV and expiration date associated with the card. 

_**Note**: If you use a card reader or terminal to capture track data, you should **not** use this method. Updating the token may delete the track data, making the token unusable._

To update a token, make a subsequent request to the `tokenize` endpoint using the token in the `account` parameter, and include the `cvv` and `expiry` parameters, as follows:

```
POST /cardsecure/api/v1/ccn/tokenize HTTP/1.1
Content-Type: application/json

{
  "account" : "9417119164771111",
  "expiry" : "1122",
  "cvv" : "123"
}
```

<!-- type: row -->

<!-- type: card
title: CardSecure API Changelog
description: Visit the Changelog for more information on recent updates to the CardSecure API and documentation
-->

<!--type: row-end -->

# RESTful Implementation

The CardSecure RESTful web service uses the JSON (JavaScript Object Notation) method of encoding data for transmission via the HTTP network protocol. Most programming languages have libraries to convert an arbitrary object to and from a JSON data transfer encoding.

The CardSecure API provides a library of POST operations used to submit HTTP requests to the web service. Responses are returned in JSON objects. See [CardSecure API Service Endpoints](#cardsecure-api-service-endpoints) for detailed information on the supported request types and corresponding responses.

# Tokenizing and Encrypting PAN Data

<!-- theme: danger -->
> You should only use CardSecure to programmatically handle and tokenize clear payment account numbers (PANs) if your business is a registered PCI Level 1 or Level 2 certified merchant. If you are not already certified for compliance with the Payment Card Industry's standards and guidelines for handling sensitive account data, see https://www.pcisecuritystandards.org/ for more information.
> 
> Additionally, you can integrate the [Hosted iFrame Tokenizer](?path=docs/documentation/HostediFrameTokenizer.md) with your solution to safely handle and tokenize clear PANs without increasing your business' scope of PCI compliance.

CardSecure provides encryption and tokenization services with a secure data vault typically used to store payment card Primary Account Numbers (aka PANs) in a PCI-DSS compliant manner. CardSecure generates a unique and random token value to be used by your application instead of a clear or encrypted PAN, potentially reducing PCI scope considerably.

If your application is using a database with sufficient encryption, CardSecure can tokenize data without additional encryption.

See [Understanding CardSecure Tokens](?path=docs/documentation/CardSecure.md#understanding-cardsecure-tokens) for detailed information for working with tokens.

## Tokenizing Card-Present Transactions

The CardSecure API supports tokenization of card data captured using an encrypted credit card reader or terminal (for example a [CardPointe Desktop](?path=docs/documentation/CardPointeDesktopSDKDeveloperGuide.md) or [CardPointe Integrated Terminal](?path=docs/APIs/CardPointeIntegratedTerminalAPI.md) device). 

When a credit card is swiped or inserted at a supported terminal, the extracted encrypted data can be submitted to CardSecure for direct tokenization.

> CardSecure does not support passing unencrypted track data. The encrypted data extracted from the device must be produced from a device pre-injected with the required Security Key. If testing is required using non-encrypted track, the data can be tokenized via the CardPointe Gateway (as part of an authorization), or the Personal Account Number (PAN) must be parsed and sent directly with a tokenization request.

CardSecure decrypts the encrypted swipe data and generates a token for the card number. CardSecure also stores the card’s encrypted track data for a short period of time, after which it is discarded.

If the token is used in an [authorization request](?path=docs/APIs/CardPointeGatewayAPI.md#authorization) to the CardPointe Gateway during this retention period, the stored track data associated with the token is sent to the processor; this potentially allows the transaction to run as a "card present" transaction. The track data is discarded after the authorization request, and can only be used in a single request.

## Encrypting and Tokenizing Payment Account Data 

CardSecure allows clients to encrypt the account data (PAN or ACH) sent in a [tokenization request](#tokenize) using a client-specific RSA public key. CardPointe support can generate a unique RSA key pair, provide the public key (in X.509 format) to you, and retain the private key.

Data must be encrypted using RSA encryption in ECB mode with PKCS1 padding. The encrypted data must be Base64-encoded when sent to CardSecure in a [tokenization request](#tokenize).

CardSecure uses the corresponding private key to decrypt the data. The decrypted data is temporarily stored in CardSecure's vault, and the token (generated from the encrypted data) is returned to your application for use in an [authorization request](?path=docs/APIs/CardPointeGatewayAPI.md#authorization) to the CardPointe Gateway. Note that the token itself is not encrypted.

> Note the following restrictions:
> 
> - CardSecure only supports encrypted Personal Account Number (PAN) or ACH (eCheck) data. Track data is not supported in an encryption request.
> - CardSecure encryption is not supported for use with CardPointe Retail Terminal or CardPointe Integrated Terminal devices due to the encryption key already configured within the terminals.

# Developer Resources

The following resources are available to simplify your integration.

## Developer Guides 

Our [Developer Guides](?path=docs/getting-started.md) provide additional information for integrating CardPointe products and services.

You can browse the whole library at the [Developer Guides home page](?path=docs/getting-started.md), or jump right to the [CardSecure Developer Guides](?path=docs/documentation/CardSecure.md) for more information on integrating CardSecure with your solution.

## Sample Postman Collection 

To help you get started with your integration, we created a sample Postman Collection that includes a template of the CardSecure API service endpoints.

The CardSecure API Integration collection includes an API reference document as well as a sample Environment, to simplify your integration.

Click the button below to download the CardSecure API Integration collection:

> [Run in Postman](https://app.getpostman.com/run-collection/7fcd43bb5e78941439f7#?env%5BCardSecure%20JSON%20API%20Integration%20Environment%5D=W3sidHlwZSI6InRleHQiLCJlbmFibGVkIjp0cnVlLCJrZXkiOiJzaXRlIiwidmFsdWUiOiJ7VUFUIG9yIHByb2R1Y3Rpb24gc2l0ZX0ifV0=)

### Configuring Your Postman Environment 

Environment variables allow you to autofill select fields with pre-configured values. Set the `{{site}}` variable to your specific site value provided during your account creation.

# CardSecure API Service Endpoints 

The following table lists the CardSecure API service endpoints and their functions. These endpoints are described in greater detail in the following sections.

#### Complete List of CardSecure API Service Endpoints

| Service	| API Version	| Description |
| --- | --- | --- |
| tokenize | v1	| Tokenizes sensitive data provided in the request, and returns a CardSecure token.
| echo | v1	| Sends a ping command to the CardSecure server to verify the connection.

## tokenize

A request to the tokenize endpoint returns a CardSecure token generated from the data provided in the request.

A tokenize request includes payment account data in either the `account` or `devicedata` field. 

Use the `account` field to provide a clear or encrypted payment account number (PAN) or ACH account number (in the format <routing>/<account>). 

If the `account` data is encrypted, include the `encryptionhandler` parameter to instruct CardSecure to decrypt the data using the private key for your site.

Use the `devicedata` field to provide track data received from a terminal device for an MSR (swipe), EMV (chip), or NFC (contactless) transaction, or a digital wallet payload (for example for a Google Pay transaction).

The tokenize response includes a token generated from the account data. You can then use this token to submit an [authorization request](?path=docs/APIs/CardPointeGatewayAPI.md#authorization) to the CardPointe Gateway.

#### Endpoint URL

The URL for the tokenize endpoint is:

`https://<site>.cardconnect.com/cardsecure/api/v1/ccn/tokenize`

#### Request Header

The following key/value pair is required in the request header:

| Key	| Value
| --- | ---
| Content-Type | application/json

#### Request Parameters

Fields in bold are required.

> Either `account` or `devicedata` are required. Use `account` when manually entering encrypted or clear PANs. Use `devicedata` when capturing card data from a card reader or terminal device.

| Field	| Description
| --- | ---
| **account**	| One of the following: <br> <br> A clear or encrypted payment account number (PAN) <br> An ACH routing and account number string in the format <routing>/<account> for eCheck transactions <br> <br> If `encryptionhandler` is provided, the account is treated as an encrypted PAN.
| **devicedata** | One of the following: <br> <br> Encrypted track data retrieved using a card reader or terminal device (MSR/EMV/NFC) <br> Apple Pay payment token data (see the [Apple Pay Developer Guide](?path=docs/documentation/ApplePayDeveloperGuide.md) for detailed information) <br> Google Pay wallet payload (see the [Google Pay Developer Guide](?path=docs/documentation/GooglePayDeveloperGuide.md) for detailed information)
| cvv	| Optional, the 3 or 4 digits card verification value (CVV). <br> <br> Must be a 3 or 4 character numeric string; strings greater than 4 characters or fewer than 3 characters are not supported. <br> <br> Alphanumeric and special characters are not supported.
| expiry | Optional, the card expiration date. <br> <br> Must be a numeric string, in one of the following formats: <br> <br> MMYY <br> YYYYM (for single-digit months) <br> YYYYMM <br> YYYYMMDD
| signature	| Signature data, if captured. Data must be a Base64-encoded, Gzipped bitmap (BMP).
| encryptionhandler	| One of the following : <br> <br> For an encrypted PAN or ACH, specify **RSA** <br> For an Apple Pay payment token, specify **EC_APPLE_PAY** <br> For a Google Pay wallet payload, specify **EC_GOOGLE_PAY**
| unique | Specifies whether a unique token should be generated. <br> <br> Specify **true** to generate a unique token. <br> <br> Defaults to **false**.

#### Response Parameters

| Field	| Description
| --- | ---
| message	| The status of the request. <br> <br> Returns "No Error" if the request was successful. <br> <br> Returns a descriptive error message if the request failed. <br> <br> See [Error Codes and Messages](#error-codes-and-messages) for a complete list of possible error codes and messages.
| errorcode	| An error code, if the request encountered an error.  <br> <br> Returns "0" if the request was successful. <br> <br> See [Error Codes and Messages](#error-codes-and-messages) for a complete list of possible error codes and messages.
| token	| The token generated from the request data.

## echo

A call to the echo service endpoint sends a ping command to the CardSecure server to verify the application's connection. You can include a message in the request, which is returned in the response if the request is successful. If the request fails, an [error message](#error-codes-and-messages) is returned with a corresponding [error code](#error-codes-and-messages).

#### Endpoint URL

The URL for the tokenize endpoint is:

`https://<site>.cardconnect.com:<port>/cardsecure/api/v1/echo`

#### Request Header

The following key/value pair is required in the request header:

| Key	| Value
| --- | ---
| Content-Type | application/json

#### Request Parameters

Fields in bold are required.

| Field	| Description
| --- | ---
| **message**	| A message to send in the ping request and receive in the response. The value can be blank, however the message field must be included in the request.

#### Response Parameters

| Field	| Description
| --- | ---
| message	| The `message` value included in the request. <br> <br> Returns a descriptive error message if the request failed. <br> <br> See [Error Codes and Messages](#error-codes-and-messages) for a complete list of possible error codes and messages.
| errorcode	| An error code, if the request encountered an error. <br> <br> Returns "0" if the request was successful. <br> <br> See [Error Codes and Messages](#error-codes-and-messages) for a complete list of possible error codes and messages.

# Error Codes and Messages 

The CardSecure server generates error messages to its log file for various conditions. If a request encounters an error, an error code and corresponding message are returned in a response to the calling application.

The following table lists all errors generated by CardSecure.

> All error codes contain four digits. Two zeros precede the codes listed below.

#### CardSecure Error Codes and Messages

| Error Code | Reason                                   |
|------------|------------------------------------------|
| 00         | No error.                                |
| 03         | Wrong version number.                    |
| 07         | Invalid action.                          |
| 08         | Data is not decimal digits.              |
| 12         | Key ID is not found.                     |
| 13         | Data is too long.                        |
| 14         | Data is empty.                           |
| 16         | Unknown algorithm.                       |
| 17         | Encryption failure.                      |
| 19         | Base 64 decode failure.                  |
| 20         | Crypto data is the wrong length.         |
| 21         | Data is too short.                       |
| 22         | CCN wrong length on decryption.          |
| 24         | Decryption failure.                      |
| 25         | Bad PKCS 5 padding.                      |
| 26         | Bad decrypted length field.              |
| 27         | Keystore not initialized.                |
| 33         | Application not initialized.             |
| 35         | Buffer too short.                        |
| 38         | Illegal characters.                      |
| 39         | Wrong message length.                    |
| 43         | Server failed.                           |
| 44         | Config item is bad.                      |
| 46         | Token database problem.                  |
| 47         | Unauthorized host.                       |
| 48         | Token purged.                            |
| 49         | Data not found.                          |
| 50         | Error hashing the value.                 |
| 53         | Token or data duplicate.                 |
| 54         | Card number check (Luhn formula) failed. |
| 55         | Missing or unknown site name.            |
| 56         | Token format is bad.                     |
| 57         | Request signature is invalid.            |
| 58         | Request signature timestamp expired.     |
| 59         | Request signature mismatch.              |
| 60         | Request signature failure.               |
| 61         | PIN translation failure.                 |
| 63         | Keystore import failure.                 |
| 64         | Keystore userstore failure.              |
| 65         | Keystore invalid processor.              |
| 66         | Unauthorized terminal.                   |
| 68         | Keystore rollback failure.               |
| 71         | Unauthorized decrypt.                    |
| 72         | Invalid EMV tag data.                    |
| 73         | EMV tag data empty.                      |
| 74         | Missing required parameter.              |
| 75         | Invalid elliptic curve type parameter.   |
| 76         | JSON serialization error.                |
| 77         | JSON deserialization error.              |
| 78         | Invalid swipe data.                      |
