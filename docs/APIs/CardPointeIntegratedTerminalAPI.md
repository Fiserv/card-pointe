> The Bolt Terminal API is now the CardPointe Integrated Terminal API.

# CardPointe Integrated Terminal API Overview

The CardPointe Integrated Terminal API streamlines the integration of our P2PE-validated, card-present payment acceptance solution with your point-of-sale (POS) application. Integrating the Terminal API with your application, allows you to easily and securely accept card-present payments within your POS environment. 

The CardPointe Integrated Terminal solution consists of the following components:

- The Terminal API, which provides your software with access to the terminal service.
- CardPointe Integrated Terminals, pre-provisioned for your merchant account. 
- The [CardPointe Gateway](?path=docs/APIs/CardPointeGatewayAPI.md), which provides a complete solution for transaction processing and reporting. 
- [CardSecure](?path/docs/documentation/CardSecure.md), which tokenizes payment card information.
- Your point-of-sale (POS) software, integrated with the Terminal API.

> This API reference describes the complete Terminal API and all supported features and operations. Some features are specific to either Ingenico or Clover terminals, as noted throughout the API. See the [CardPointe Integrated Terminal Developer Guide for Clover Terminals](?path/docs/documentation/CardPointeIntegratedTerminalDeveloperGuideForCloverTerminals.md) for detailed information on key differences and additional information specific to integrating Clover terminals.

<!-- theme: warning -->
> The CardPointe Gateway API provides additional capabilities not offered by the Terminal API, including the ability to void or inquire on past transactions. To take full advantage of these features, you must integrate the CardPointe Gateway API into your payment acceptance solution. See the [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) for information on integrating these features and other advanced capabilities.

# What's New?

<!-- theme: warning -->
> Visit [Statuspage](http://status.cardconnect.com/) and click **subscribe to updates** to receive important release and status notifications.

## Date Updated: 8/16/2022 

Our Bolt family of integrated solutions is becoming **CardPointe Integrated Payments**.

Over the coming weeks, your terminals will receive an update with minor changes, including an updated default theme and branding for terminals that are not configured with custom branding.

Terminals will update automatically during the nightly reboot window; therefore, we ask that you leave your terminals powered on and connected to your network overnight. Once your terminal is updated, you may notice the following changes:

- If you are using the default device wallpaper and theme, the Bolt wallpaper and logo will change to **CardPointe Integrated Terminal**.
- The **Bolted** and **Unbolted** statuses will change to **Connected** and **Disconnected**, respectively.

<!-- type: row -->

<!-- type: card
title: <center> CardPointe Integrated Terminal API Changelog </center>
description: <center> Visit the Changelog for more information on recent updates to the CardPointe Integrated Terminal API </center>
link: ?path=docs/changelog/CardPointeIntegratedTerminalAPI.md
-->

<!-- type: row-end -->

# Getting Started

See the CardPointe Integrated Terminal Developer Guides for information to help you get started, including guides for [understanding the Terminal API application workflow](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md#understanding-the-terminal-api-application-workflow) and [understanding the Terminal API service endpoints](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md#understanding-the-terminal-api-service-endpoints).

Additionally, see the [API Connectivity Guide](?path=docs/documentation/APIConnectivityGuide.md) for information on API connectivity.

## RESTful Implementation

The Terminal API is a RESTful web service, which uses the JSON (JavaScript Object Notation) method of encoding data for transmission via the HTTP network protocol. Most programming languages have libraries to convert an arbitrary object to and from a JSON data transfer encoding. See the [API Basics and Best Practices Guide](?path=docs/documentation/APIBasicsAndBestPractices.md) for more information on these concepts.

The Terminal API provides a library of POST operations used to submit HTTP requests to the web service. The Terminal service expects that ALL properties are encoded as US ASCII strings. Responses are returned in JSON objects. See [Service Endpoints](#service-endpoints) for detailed information on the supported request types and corresponding responses.

<!-- theme: danger -->
> Changes to the Terminal API are designed to retain backwards-compatibility. The JSON standard does not assign any significance to the ordering of key/value pairs, and this API does not guarantee the order of key/value pairs in response messages. See the [API Basics and Best Practices Guide](?path=docs/documentation/APIBasicsAndBestPractices.md) for recommendations for retaining backwards compatibility in your application.

## Requirements

Integrating the Terminal API requires: 

- Your application, which calls the Terminal service endpoints via HTTPS. 
- Pre-provisioned terminal devices.

Additionally, integrating EMV acceptance requires your merchant account to be on a supported processing platform. 

For more information on specific requirements, contact integrationdelivery@fiserv.com.

## Additional Resources

In addition to this API documentation, the following additional resources are available to help you get started.

### Developer Guides 

Browse the [Developer Guides](?path=docs/getting-started.md) library for general information on integrating our products and services.

The following guides provide helpful information for developing your integration:

<!-- type: row -->

<!-- type: card
title: <center> CardPointe Integrated Terminal Developer Guides </center>
description: <center> Provides best practices and supplemental information for integrating the Terminal API with your point-of-sale application </center>
link: ?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md
-->

<!-- type: card
title: <center> CardPointe Integrated Terminal Developer Guide for Clover Terminals </center>
description: <center> Provides specific details for integrating Clover terminals with a new or existing CardPointe Integrated Terminal solution </center>
link: ?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuideForCloverTerminals.md
-->

<!-- type: row-end -->

Additionally, review the [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) documentation for more information on the additional transaction processing and reporting capabilities offered by the CardPointe Gateway, for example the ability to void and inquire on transactions.

### Run in Postman

To help you get started with your integration, we created a Terminal API Postman collection that includes a template of the Terminal API service endpoints. Click the following button to download the collection.

> [Run in Postman](https://app.getpostman.com/run-collection/78bf7730d5cf55a3080f?action=collection%2Fimport#?env%5BBolt%20Terminal%20API%20UAT%20Environment%5D=W3sia2V5Ijoic2l0ZSIsInZhbHVlIjoie1VBVCBvciBwcm9kdWN0aW9uIHNpdGV9IiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJ1cmwiLCJ2YWx1ZSI6Imh0dHBzOi8ve3tzaXRlfX0uY2FyZHBvaW50ZS5jb20vYXBpIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJBdXRob3JpemF0aW9uIiwidmFsdWUiOiJ7eW91ciBhdXRoIGtleX0iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Im1lcmNoYW50SWQiLCJ2YWx1ZSI6Int5b3VyIG1lcmNoYW50SWR9IiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJoc24iLCJ2YWx1ZSI6Int5b3VyIGhzbn0iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6IlgtQ2FyZENvbm5lY3QtU2Vzc2lvbktleSIsInZhbHVlIjoiIiwiZW5hYmxlZCI6dHJ1ZX1d)

See [Running the API in Postman](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md#running-the-api-in-postman) in the [CardPointe Integrated Terminal Developer Guides](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md) for information on using the Postman collection.

# Service Endpoints

The following table details each service of the Terminal API, its associated resource name, and which version it is supported in. The Terminal API version and endpoint name are appended to the base URL.

Example URL:

`https://<site>.cardpointe.com/api/v2/display`

Details about the REST implementation for each endpoint follow this section.

<!-- theme: warning -->
> See [Understanding the Terminal API Service Endpoints](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md#understanding-the-terminal-api-application-workflow) for additional information to help you make the most of your terminal integration.

## Complete List of Terminal API Service Endpoints

| Service          | Resource Name    | Supported Version(s) | Requires Session Key |
|------------------|------------------|----------------------|----------------------|
| [listTerminals](../api/?type=post&path=/api/v2/listTerminals) | listTerminals | v1 + v2 | false |
| [terminalDetails](../api/?type=post&path=/api/v3/terminalDetails) | terminalDetails  | v3 | false |
| [dateTime](../api/?type=post&path=/api/v2/dateTime) | dateTime | v1 + v2 | v1: false <br> v2: true |
| [getPanPadVersion](../api/?type=post&path=/api/v2/getPanPadVersion) | getPanPadVersion | v2 | true |
| [ping](../api/?type=post&path=/api/v2/ping) | ping | v1 + v2 | v1: false <br> v2: true |
| [preconnect](../api/?type=post&path=/api/v2/preconnect) | preconnect | v2 | false |
| [connect](../api/?type=post&path=/api/v2/connect) | connect | v2 | false |
| [disconnect](../api/?type=post&path=/api/v2/disconnect) | disconnect | v2 | true |
| [display](../api/?type=post&path=/api/v2/display) | display | v1 + v2 | v1: false <br> v2: true |
| [clearDisplay](../api/?type=post&path=/api/v3/clearDisplay) | clearDisplay     | v3                   | true                 |
| [readConfirmation](../api/?type=post&path=/api/v3/readConfirmation) | readConfirmation | v2                   | true                 |
| [readInput](../api/?type=post&path=/api/v2/readInput) | readInput        | v2                   | true                 |
| [readSignature](../api/?type=post&path=/api/v2/readSignature) | readSignature    | v2                   | true                 |
| [cancel](../api/?type=post&path=/api/v2/cancel) | cancel           | v1 + v2              | v1: false <br> v2: true |
| [readCard]         | readCard         | v2                   | true                 |
| [readManual]       | readManual       | v2                   | true                 |
| [authCard]         | authCard         | v3                   | true                 |
| [authManual]       | authManual       | v3                   | true                 |
| [tip]              | tip              | v3                   | true                 |
| [printReceipt]     | printReceipt     | v3                   | true                 |

# HTTP Response Codes

All requests return one of the following HTTP response codes:

| Response | Description
| --- | ---
| 200	| Success
| 400	| Bad Request
| 401	| Unauthorized
| 403	| Invalid HSN for MerchantID
| 500	| Client or Server Error

## 200: Success

A 200 response indicates that the request was completed successfully and includes the response body.

## 400: Bad Request

A 400 response indicates that the request could not be completed due to invalid or missing information in the header or body elements. 

a 400 response can include one of the following error codes:

| Error Code | Description
| --- | ---
| 3	| Generic Error
| 4	| Missing Required Parameter(s)

Error Code 3 is a generic code, which includes an error message that provides specific contextual information for the error, based on the request. 

For example, if a the prompt parameter in a call to the readSignature endpoint is greater than 16 characters, the following response is returned:

`{ "errorCode" : 3, "errorMessage" : "Prompt text must be less than or equal to 16 characters" }`

The following table describes Error Code 3 conditions that can be returned for calls to each endpoint.

| Endpoint	| Error Message
| --- | ---
| connect	| Incorrect token
| dateTime	| Text could not be parsed
| disconnect	| Session key for hsn was not valid
| preconnect	| Merchant is not configured for two-factor authentication
| printReceipt	| printDelay value must be between 0 and 60000 (miliseconds)
| readInput	| Invalid format
| readSignature	| Prompt text must be less than or equal to 16 characters

## 401: Unauthorized

An HTTP 401 response indicates an authentication error. Authentication errors can occur when the API key is invalid or missing from the Authorization header, the merchant ID is missing from the body or does not match the API key. Authorization errors can also occur due to a terminal configuration error in TMS.

## 403: Invalid HSN for MerchantID

a 403 response indicates that the request included an HSN value that does not match the specified merchant ID.

## 500: Client or Server Error

A 500 response indicates a client or server error. The response includes an error code, which corresponds to one or more specific error messages that describe the error.

<!-- theme: warning -->
> In the event that an authorization attempt was successfully initiated before the command sequence timed out or was canceled, the response body returned for errorCode 1 and errorCode 8 include the authorization response data returned from the CardPointe Gateway in an `authResponse` object.

For example, error code 1 denotes a general client or server error, which can be one of the following:

- Terminal request timed out

`{ "errorCode" : 1, "errorMessage" : "Terminal request timed out" }`

- Request method not supported

`{ "errorCode" : 1, "errorMessage" : "Request method 'GET' not supported" }`

- Invalid content type

`{ "errorCode" : 1, "errorMessage" : "Content type 'application/json' not supported" }`

- Attempting to connect to a terminal with an active session, and the force property is set to false.

`{ "errorCode" : 1, "errorMessage" : "Session exists for hsn" }`

The following table describes the various error codes that can be returned in an HTTP 500 response:

> Error codes are fixed; however the error messages associated with each error code are subject to change. 

| Error Code | Description | Source
| --- | --- | ---
| 1	| Generic catch all error - Server failed	| Client/Server <br> See [500: Client or Server Error](#500-client-or-server-error) for more information.
| 6	| Terminal not connected to the terminal service (no request queue registered) | Client/Server
| 7	| Terminal in use	| Client/Server
| 8	| The in-flight request (for example an authCard command sequence) was canceled. The error includes one of the following messages, which describe the specific cancellation scenario. <br> <br> `Command Cancelled` - The client application sent a cancel request. <br> <br> `Operation Cancelled` - The cardholder or merchant canceled a readCard sequence at the terminal. <br> <br> `Authorization Cancelled` - The client application, merchant, or cardholder attempted to cancel an authCard or authManual request; however, the terminal service already submitted the authorization request to the CardPointe Gateway. In this case, the authorization was processed, and the error includes an `authResponse` object that contains the authorization response from the CardPointe Gateway. | Client/Server
| 100	| Internal Server Error if no terminals found for merchant | listTerminals response
| 400	| PIN Debit not supported for merchantID <MID> | authCard response
| 500	| Generic	| authManual response
| 624	| Decryption failure | CardSecure
| 643	| Server failed	| CardSecure
| 700	| Signature capture not supported by device	| readSignature or authCard, authManual, readCard, or readManual request including a signature
| 800	| Printing not supported | authCard or authManual request including  `"printReceipt:"true"`, or printReceipt request, when the Clover Mini is running in low-power mode (not connected to AC power). <br> <br> _**Note**: This error response also includes the receiptData array for the associated transaction._
