# CardPointe Gateway API Changelog

The following entries describe changes to the [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) and documentation.

Visit status.cardconnect.com and click subscribe to updates to receive important release and status notifications.

## Date Updated: 11/9/2023

An update to the CardPointe Gateway was released to the UAT environment on 10/23/2023 and has been deployment to the production environment on 11/9/2023.

This release includes the following updates in addition to internal fixes and enhancements:

### Original MID Requirement for Refund Requests 

Refund requests must include the Merchant ID (MID) used in the original authorization request. If the refund includes a different MID, or a null value, then the request returns an error as seen in the example below. See the [Refund Requirement Update](https://support.cardpointe.com/refund-requirements-update#whats-changing) for more information. 

```
{ 
    "respproc": "PPS", 
    "resptext": "Txn not found", 
    "retref": "198096236590", 
    "respcode": "29", 
    "respstat": "C" 
}
```

### Surcharge Endpoint 

The [surcharge endpoint](../api/?type=get&path=/cardconnect/rest/surcharge) has been added to determine whether or not a surcharge fee will be applied to a transaction, based on the cardholder's postal code and payment method. This allows your application to notify the cardholder of the potential credit card surcharge prior to completing a transaction.

This request does **not** effect whether or not a surcharge is applied to a transaction; the CardPointe Gateway automatically applies a surcharge depending on the merchant configuration and the eligibility of the payment account. This request is used only for informational purposes.

For more information on credit card surcharging, see the [Merchant Surcharge Program overview](https://support.cardpointe.com/compliance/surcharging). 

```
{
    "token": "CREDIT",
    "postal": "NOT_RESTRICTED",
    "messages": []
}
```

### Authorization Code in Refund Endpoint 

The `authcode` field is now included in the API response for the [/refund](../api/?type=post&path=/cardconnect/rest/refund) endpoint for both online and offline refunds. For online refunds the `authcode` returned by the processor will be included in the response. For auto-approved (offline) refunds, the `"authcode":"REFUND"` is returned in the response. The following example displays an online refund authorization response:

```
{
    "authcode": "PPS655",
    "respproc": "AMEX",
    "amount": "3.00",
    "resptext": "Approval",
    "retref": "270110240566",
    "respstat": "A",
    "respcode": "000",
    "merchid": "831831831901"
}
```

See the [Refund Authorization overview](https://support.cardpointe.com/compliance/refund-authorizations) for more information on online refunds.

### Profile Field Validation 

When making an authorization request with a stored profile, the profile field will no longer accept a token; instead, you must include the profile ID and account ID pair to authorize a transaction. 

## Date Updated: 8/26/2023 

An update to the CardPointe Gateway is tentatively scheduled for release to the UAT environment on 8/18/2023 and to the Production environment on 8/26/2023.

This release includes the following updates in addition to internal fixes and enhancements: 

### Original MID Requirement for Refund Requests 

Refund requests must include the Merchant ID (MID) used in the original authorization request. If the refund does not include the same MID, or a null value, then the request returns an error; for example:

```
{ 
    "respproc": "PPS", 
    "resptext": "Txn not found", 
    "retref": "198096236590", 
    "respcode": "29", 
    "respstat": "C" 
}
```

### Updated Incorrect URL Endpoint API Response 

When an incorrect endpoint URL is called a new error response is returned that displays the incorrect URL passed in the request. For example, the path below has a spelling error: 

```
{ 
    "timestamp": "2023-06-20T15:04:43.980+00:00", 
    "status": 404, 
    "error": "Not Found", 
    "path": "/cardconnect/rest/inquireByOderid" 
}
```

## Date Updated: 6/10/2023 

An update to the CardPointe Gateway was released to the UAT environment on 6/2/2023 and to the Production environment on 6/10/2023.

This update includes backend enhancements.

## Date Updated: 5/6/2023 

An update to the CardPointe Gateway was released to the UAT environment on 4/28/2023 and to the Production environment on 5/6/2023.

This release includes the following update, in addition to internal fixes and enhancements:

### voidByOrderId Now Ignores Declined Transactions 

The voidByOrderId request now ignores declined transactions.

Previously, when a transaction declined, and a merchant reused the order ID to retry and successfully complete the transaction, then a subsequent voidByOrderID request would attempt to void the first, declined, transaction causing the void to fail.

## Date Updated 3/18/2023 

An update to the CardPointe Gateway was deployed to the UAT environment on 2/22/2023 and to the production environment on 3/18/2023.

This release includes backend enhancements in addition to the following update:

### COF Update for First Data North Retail Merchants 

For Retail merchants who process Card on File (COF) transactions on the First Data North platform, subsequent COF transactions will no longer process as COF transactions. Instead these transactions will process as `Swiped` or `Keyed` transactions, unless the transaction includes `"ecomind" : "T"` (telephone) or `"ecomind:"E"` (e-commerce).

## Date Updated: 11/15/2022 

An update to the CardPointe Gateway was released to the UAT environment on 11/10/2022 and to the production environment on 11/15/2022.

This release includes the following updates, in addition to internal fixes and enhancements:

### UAT PIN Debit Testing (First Data North and Rapid Connect) 

For merchants accepting PIN Debit cards on the First Data North or First Data Rapid Connect processor, you can now simulate PIN Debit card transactions in the UAT environment.

To simulate a PIN Debit transaction, you can now make an authorization request using either of the following test cards and amount ranges:

| Test Card #	| Amount Range
| --- | ---
| `4222222222222204` | Minimum amount - `0` <br> Maximum amount - `4999` ($49.99)
| `6510000000000331` | Minimum amount - `0` <br> Maximum amount - `4999` ($49.99)

See [Testing your Integration](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md) for more details on testing in the UAT environment.

## Date Updated: 10/22/2022 

An update to the CardPointe Gateway was released to the UAT environment on 10/6/2022 and to the production environment on 10/22/2022.

This release includes the following updates, in addition to internal fixes and enhancements:

### Visa Decline Category Codes 

> This update applies to merchants using the following processors:
>
> - First Data North
> - First Data Rapid Connect
> - Chase Paymentech Salem
> - Chase Paymentech Tampa
> - TSYS
> - Vantiv

For declined Visa transactions, the CardPointe Gateway API now returns two new fields, `declineCategory` and `declineCategoryText`, in the response from the following endpoints:

- `auth`
- `refund`
- `inquire`
- `inquireByOrderId`

<!-- theme: warning -->
> See [Visa Decline Rules and Responses](https://support.cardpointe.com/compliance/visa-decline-rules) for detailed information on implementing and testing these new fields.

The following table describes these new fields and potential values in detail:

|   |   |   |   | 
| --- | --- | --- | --- |
| declineCategory	| Visa Decline Category Code	| 1 | A numeric decline category code, only returned for declined transactions using Visa accounts. <br> <br> One of the following values: <br> <br> **1** - Issuer Will Never Approve. <br> In this case, the merchant should never reattempt to authorize the transaction, and the cardholder must provide an alternate payment method. This code can be returned for accounts that are blocked, never existed, or are otherwise flagged as invalid by the issuer. <br> <br> **2** - Issuer Cannot Approve at this Time. <br> In this case, the merchant may reattempt the authorization at a later time. Reasons for this decline can include velocity controls, credit risk holds, or system errors. <br> <br> **3** - Issuer Cannot Approve Based on Details Provided. <br> In this case, the card has been declined due to incorrect data, such as the CVV, expiry, or PIN. The merchant may attempt the authorization, after obtaining the corrected data.
| declineCategoryText | Visa Decline Category Code Description | 25 | A text description to provide more information on the decline. <br> <br> One of the following: <br> <br> **Do not retry** - Returned for `declineCategory` 1. <br> **Wait to retry** - Returned for `declineCategory` 2. <br> **Fix the request and retry** - Returned for `declineCategory` 3.

> For First Data Rapid Connect UAT transactions only, "declineCategory":"4" and "declineCategoryText":"" may be returned.

### AMEX Processor Refund Authorizations 

This release includes Refund Authorization support for merchants who process American Express transactions directly through the AMEX processor. 

> This does **not** affect American Express card refunds on other processors (for example, First Data North).

Amex refund and forced credit responses will now return `"respproc":"AMEX"` instead of `PPS`, as well as the appropriate AMEX processor [response code](?path=docs/documentation/GatewayResponseCodes.md).

<!-- theme: warning -->
> See [Refund Authorizations](https://support.cardpointe.com/compliance/refund-authorizations) for more information.

### Vantiv Visa L3 Item Quantity 

For Vanitv Visa L3 orders, the `items` array's `quantity` field is now truncated to 10 characters.

## Date Updated: 7/26/2022 

A release of the CardPointe Gateway was deployed to the UAT environment on 7/12/2022 and to the production environment on 7/23/2022.

_**Note**: This update was previously released to the UAT environment on 7/7/2022 and rolled back on 7/8/2022._

This release includes the following updates, in addition to internal fixes and enhancements:

### Additional Validation for Creating Profiles 

The CardPointe Gateway no longer allows duplicate profiles. If you attempt to create a new profile that exactly matches an existing profile, the new profile will not be created; instead the response includes the `profileid` and `acctid` for the existing profile. 

The following fields are checked for matches, and if **all** of the following fields match an existing profile, the new profile is not created:

- merchid
- token
- accttype
- expiry
- name
- address
- city
- state
- country
- postal
- phone
- email
- auoptout

Additionally, If a profile create or authorization request with `"profile":"Y"` attempts to create a duplicate profile with `"defaultacct":"Y"`, the CardPointe Gateway will update the existing profile to set it as the default, and will return the existing `profileid` and `acctid` info in the response.

### First Data North Processor Updates 

This release includes an updated First Data North processor certification for the CardPointe Gateway, which includes the following features and updates for merchants processing on First Data North:

- **Support for Card on File (COF) transactions**

    See the Visa and Mastercard Stored Credential Transaction Framework Mandate support article for more information.

- **Support for refund authorizations for Discover and Mastercard refunds**

    Support for Visa refund authorizations was made available in a previous release.
    See the Refund Authorizations support article for more information.

- **Support for 3-D Secure 2.0**

    See the 3-D Secure 2.0 support article for more information.

### ProfitStars ACH Timeout Reversals 

This release includes improvements for handling ProfitStars ACH transactions that timeout during the transaction process.

Like other processors, the CardPointe Gateway now waits 32 seconds for a response from the ProfitStars host. If no response is received, the Gateway returns a PPS `"Timed Out"` response via the API, and attempts to look up and void the original transaction if it was successful.

### New First Data Omaha Response Code 

This release introduces a new First Data Omaha (FOMA) response code for an invalid CVV/CVV2 match:

| respproc | respcode	| respstat | resptext
| --- | --- | --- | ---
| FOMA | 14	| C	| "Invalid CVV2"

### First Data Rapid Connect Card on File (COF) Update

for First Data Rapid Connect, the `cofscheduled` parameter is no longer required for card on file (COF) transactions. if not specified, this field now defaults to `N` (not scheduled).

## Date Updated: 6/29/2022 

An update to the CardPointe Gateway was released to the UAT and Production environments on 6/29/2022.

> This update will roll out to First Data Rapid Connect merchants in a phased approach over the next several days.

The update includes a fix to address First Data Rapid Connect transaction declines for recurring payments that **do not** include the required `cof` fields.

As a result of this change, all recurring payments that do not include the `cof` fields are now converted to non-recurring transactions, depending on the merchant configuration (**manually-keyed** transactions for retail merchants, or **telephone** for non-retail merchants). As a result, Rapid Connect recurring transactions without `cof` parameters will no longer qualify for recurring rates.

<!-- theme: warning -->
> To comply with the card brand requirements for handling stored credentials, all recurring payments must include the required `cof` data. See our [Visa and Mastercard Stored Credentials Framework Mandate Support Article](https://support.cardpointe.com/compliance/visa-stored-credential-transaction-framework-mandate) for more information on these requirements.

## Date Updated: 5/11/2022 

An update to the CardPointe Gateway was released to the UAT environment on 4/22/2022, and to the production environment on 5/7/2022.

This release includes fixes and internal enhancements to the CardPointe Gateway. While no client impact is expected, we encourage you to perform regression testing of your standard workflows in the UAT environment. See our [Acceptance and Regression Testing](?path=docs/documentation/APIBasicsAndBestPractices.md) guide for more information.

Additionally, this release includes the following change to the CardPointe Gateway API:

### HTTP Response Status Changes 

This release includes the following changes to the HTTP status in the response:

- HTTP responses no longer include a status reason. For example, a successful response of `200 OK` is now returned as `200`.
- An `HTTP 404` response is now returned when the request includes an invalid URL path. Previously, the CardPointe Gateway returned an `HTTP 500` response in this case. 

### entrymode Response for Amex, TSYS, and Vantiv 

The `entrymode` field in the authorization response was previously only returned for the First Data North, Rapid Connect, and Chase Paymentech Tampa processors. This field is now returned for Amex, TSYS, and Vantiv, as well. 

Possible values are:

- Keyed
- Moto
- ECommerce
- Recurring
- Swipe (Non EMV)
- DigitalWallet
- EMVContact
- Contactless
- Fallback to Swipe
- Fallback to Keyed

## Date Updated: 3/31/2022 

A CardPointe Gateway release was deployed to the UAT environment on 3/28/2022. This release will remain in the UAT environment for an extended period for testing and validation. This release will be scheduled for release to the production environment at a later date.

This release includes fixes and internal enhancements to the CardPointe Gateway.

<!-- theme: warning -->
> While no client impact is expected, we encourage you to perform regression testing of your standard workflows in the UAT environment. See our [Acceptance and Regression Testing](?path=docs/documentation/APIBasicsAndBestPractices.md) guide for more information.

## Date Updated: 1/22/2022 

A release of the CardPointe Gateway was deployed to the UAT environment on 1/20/2022 and to the Production environment on 1/22/2022.

This release includes the following updates, in addition to internal fixes and enhancements:

### TSYS Support for Refund Authorizations and Stored Credential (COF) Transactions 

This release includes support for refund authorizations and stored credential (also known as card on file or COF) transactions for TSYS merchants.

For more information on these changes and any necessary updates to your application, see the following additional resources:

- Refund Authorizations support article
- Visa and Mastercard Stored Credential Transaction Framework Mandate support article

### Deprecated Support for Contactless MSD Card Entry 

To align with the card brands, who have phased out this card entry method, support for the Contactless MSD (magnetic stripe data) format has been deprecated on the CardPointe Gateway. 

If a cardholder taps a Contactless MSD card (excluding Amex), the CardPointe Gateway will now decline the attempt and return the following response, asking the cardholder to insert the card instead:

```
{
...
"respproc":"PPS",
"respcode":"106",
"resptext":"Invalid method. Please insert card",
...
}
```

## Date Updated: 11/13/2021 

A release of the CardPointe Gateway was deployed to the UAT environment on 11/10/2021 and to the Production environment on 11/13/2021.

This release includes the following updates, in addition to internal fixes and enhancements:

### Refund Authorizations 

This release includes support for refund authorizations on the **Chase Paymentech Salem (PMT)** processor for **Visa**, **Mastercard**, and **Discover** transactions.  

With refund authorizations enabled, refund transactions are subject to the same authorization process as sale transactions, and will no longer be automatically approved by the CardPointe Gateway.

See the Refund Authorizations support article for detailed information on this change and the expected changes to refund response data.

See Testing With Amount-Driven Response Codes in the [Testing Your Integration guide](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md) for information for testing refund authorizations in the UAT environment.

### Card Account Updater GET Linked Profile Updates 

The CardAccount Updater API GET request includes a new, optional parameter, `profiles=true`, to return information on updated stored profiles.

When `profiles=true` in the request, each update record in the response includes a profiles array that includes each profile (`profileid`/`acctid` pair) associated with the card update.

See the [Card Account Updater Integration Guide](?path=docs/documentation/CardAccountUpdaterIntegrationGuide.md) for more information.

### New inquireMerchant Response Fields 

The inquireMerchant response now includes the following fields:

- `avs` - Either `Y` or `N`. Determines whether or not AVS validation is enabled for the MID. If `Y`, the MID can perform $0 authorizations to validate the `postal` and `address` values in the auth request.

- `cvv` -  Either `Y` or `N`. Determines whether or not cvv validation is enabled for the MID. If `Y`, the MID can perform $0 authorizations to validate the `cvv2` value in the auth request.

- `echeck` - Either `Y` or `N`. Determines whether or not the MID is configured to process eCheck/ACH transactions. If `Y`, the MID can process ACH transactions.

### New settlestat Response Fields 

The settlestat response now includes the following fields:

- `authcode` - The authcode copied from the authorization response.

- `cardtype` - The card brand returned by the processor.

### New First Data Omaha (FOMA) Response Codes 

The following authorization response codes may now be returned for First Data Omaha merchants:

| respproc	| respcode	| respstat	| resptext
| --- | --- | --- | --- |
| FOMA	| 11 | B | Invalid batch
| FOMA	| 12 | C | Unmatched void
| FOMA	| 13 | C | Invalid tran type

### Improved ProfitStars (PSTR) Error Responses 

ProfitStars authorization response code 99 is used to return error responses. Previously, the `resptext` field truncated these responses at 40 characters. These responses are no longer truncated (up to a maximum of 128 characters), to display the full error message.

## Date Updated: 9/15/2021 

A release of the CardPointe Gateway was deployed to the UAT environment on 9/9/2021 and to the Production environment on 9/15/2021.

This release includes the following updates in addition to internal fixes and enhancements:

### Refund Authorizations 

Refund authorizations are now enabled for the following processors and card brands:

- **First Data North** - Visa only (support for Discover and Mastercard coming in a future update)

- **First Data Rapid Connect** - Visa, Discover, and Mastercard

- **First Data Omaha** - Visa, Discover, and Mastercard

With refund authorizations enabled, refund transactions are subject to the same authorization process as sale transactions, and will no longer be automatically approved by the CardPointe Gateway. 

See the Refund Authorizations support article for detailed information on this change and the expected changes to refund response data.

See Testing With Amount-Driven Response Codes in the [Testing Your Integration guide](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md) for information for testing refund authorizations in the UAT environment.

### 3DS 2.0 Fields in inquire and inquireByOrderid Responses 

For merchants using a 3DS provider to integrate 3DS 2.0 transaction authentication and passing 3DS fields in the authorization request, these fields are now returned in the `inquire` and `inquireByOrderid` responses.

The following fields are will be returned when present in the transaction record:  

- `securedstid`

- `secureexemption`

- `secureflag`

- `securevalue`

See the 3-D Secure 2.0 Support Article for more information on using 3DS.

### UAT Request Rate Limiting Changes 

In a recent update to the CardPointe Gateway, rate limiting was enabled for the `funding`, `inquire`, and `profile` endpoints in the UAT environment.

With this release, rate-limiting is also enabled for the `settlestat` endpoint. Additionally, requests to these endpoints are now limited to **20 transactions per minute** (TPM), by IP address.

Responses from these endpoints in the UAT environment include the following rate-limit header fields:

- `X-Rate-Limit-Retry-After-Seconds:` - Returned for unsuccessful `HTTP 429 Too Many Requests` responses when the limit has been reached. Specifies the seconds remaining before the limit resets.

- `X-Rate-Limit-Remaining:` - Returned for successful `HTTP 200 OK` responses. Specifies the number of requests available before the limit is reached.

## Date Updated: 7/17/2021 

A release of the CardPointe Gateway was deployed to the UAT environment on 7/15/2021 and to the Production environment on 7/17/2021.

This release includes the following updates in addition to internal fixes and enhancements:

### Refund Response Testing in the UAT Environment 

The CardPointe Gateway UAT environment now supports refund decline emulation for First Data North, First Data Rapid Connect, and Chase Paymentech (PMT) merchants. 

See the Refund Authorizations support article for detailed information on upcoming changes to support Refund Authorizations (also referred to as Online Refunds).

Similarly to testing specific authorization response scenarios using amount-driven responses, you can test individual refund response codes, by sending a partial refund request using an amount value that includes the desired response code.

_**Note**: Like Production, transactions in the UAT environment cannot be refunded until they have been settled, unless the MID is enabled to refund unsettled transactions._

To test a refund decline, do the following:

**1.** Run an authorization request including `"capture":"y"` and `"amount":"2000.00"` or greater.

**2.** Run a refund request including the `retref` from the authorization response and `"amount":"1nnn.00"`, where `nnn` is the 2 (including leading 0) or 3-digit decline response code you want to receive.

For example, to return a RPCT 500 "Decline" response, include `"amount":"1500.00"` in the refund request.

See [Processing Host Response Codes](?path=docs/documentation/GatewayResponseCodes.md) for a list of possible response codes by processor.

See Testing With Amount-Driven Response Codes in the [Testing Your Integration guide](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md) for additional information and examples.

### Request Rate Limiting For UAT API Testing 

Requests to the `funding`, `inquire`, and `profile` endpoints in the UAT environment are now rate-limited by to **100 transactions per minute** (TPM), by IP address.

Responses from these endpoints in the UAT environment include the following new header fields:

- `X-Rate-Limit-Retry-After-Seconds:` - Returned for unsuccessful `HTTP 429 Too Many Requests` responses when the limit has been reached. Specifies the seconds remaining before the limit resets.

- `X-Rate-Limit-Remaining:` - Returned for successful `HTTP 200 OK` responses. Specifies the number of requests available before the limit is reached.

### Card Account Updater API Enhancements 

This release includes enhancements to the Card Account Updater API. See the [Card Account Updater Integration Guide](?path=docs/documentation/CardAccountUpdaterIntegrationGuide.md) for detailed information on the API.

## Date Updated: 6/12/2021 

A CardPointe Gateway release was deployed to the UAT environment on 6/10/2021, and to the production environment on 6/12/2021.

This release includes fixes and internal enhancements to the CardPointe Gateway.

## Date Updated: 5/15/2021 

A release of the CardPointe Gateway was deployed to the UAT environment on 5/11/2021 and to the Production environment on 5/15/2021.

This release includes the following update in addition to numerous internal fixes and enhancements:

### cofpermission Field for Profiles 

A new, optional field, `cofpermission`, has been added to allow you to store a record of having obtained the cardholder's permission to create a stored profile using their payment data.

You can include the `cofpermission` field in profile create or update requests, as well as auth requests when `"profile":"y"`. Optionally specify `"cofpermission":"y"` to indicate that you obtained the cardholder's permission. The default value if not specified is `"n"`.

`cofpermission` is returned in the get profile response, and returns `"n"` unless manually set to `"y"` for the profile.

## Date Updated: 4/5/2021 

A CardPointe Gateway maintenance release was deployed on 4/5/2021.

This release includes internal enhancements to the CardPointe Gateway.

## Date Updated: 3/13/2021 

A release of the CardPointe Gateway was deployed to the UAT environment on 3/10/2021 and to the Production environment on 3/13/2021.

This release includes bug fixes and other internal enhancements to the CardPointe Gateway.

## Date Updated: 2/20/2021 

A release of the CardPointe Gateway was deployed to the UAT environment on 2/12/2021 and to the Production environment on 2/20/2021.

This release includes bug fixes and other internal enhancements to the CardPointe Gateway.

## Date Updated: 1/23/2021 

A release of the CardPointe Gateway was deployed to the UAT environment on 1/13/2021 and to the Production environment on 1/23/2021.

This release includes the following enhancements:

### COF and 3DS 2.0 Support for Rapid Connect 

This release includes changes to support the COF (also known as Card on File or Visa and Mastercard Stored Credential Transaction Framework Mandate) and 3DS 2.0 features for First Data Rapid Connect Merchants. 

See the Visa and Mastercard Stored Credential Transaction Framework Mandate Support Center article for requirements and detailed information on the API changes to support COF transactions.

See the 3-D Secure 2.0 Support Center article for requirements and detailed information on the API changes to support 3DS 2.0.

### entrymode Added to the Authorization Response for Rapid Connect 

The `entrymode` field is now returned in the authorization response for First Data Rapid Connect merchants.

Possible values are:

- Keyed
- Moto
- ECommerce
- Recurring
- Swipe (Non EMV)
- DigitalWallet
- EMVContact
- Contactless
- Fallback to Swipe
- Fallback to Keyed

### New JSON Receipt Data Format in the Authorization Response 

The `receipt` parameter in the authorization request, now supports a new value, `json`, which returns a new JSON-formatted `receiptObj` field in the authorization response. When the request includes `"receipt":"json"`, the response includes the new `receiptObj` field in the response (instead of the `receiptData` field returned when `"receipt":"y"`).

`receiptObj` includes the following fields:

> The following fields are only returned when a value is available.
> These field names are returned as documented.

| Field | Description
| --- | ---
| address1	| Line 1 of the merchant's address.
| address2	| Line 2 of the merchant's address.
| Application Label	| Duplicated from the `emvTagData` object. Mnemonic associated with the AID according to ISO/IEC 7816-5. 
| dateTime	| The date and time of the transaction (YYYYMMDDHHMMSS).
| dba | The merchant's Doing Business As (DBA) name.
| emv | An object that returns the following fields, duplicated from the `emvTagData` object: <br> <br> AID- Identifies the application as described in ISO/IEC 7816-5. <br> ARC - Indicates the transaction disposition of the transaction received from the issuer for online authorizations. <br> IAD - Contains proprietary application data for transmission to the issuer in an online transaction. <br> TSI - Indicates the functions performed in a transaction. <br> TVR - Status of the different functions as seen from the terminal.
| Entry Method | Duplicated from the `emvTagData` object. <br> <br> Indicator identifying how the card information was obtained. <br> <br> One of the following values: <br> <br> Manual <br> Chip Read <br> Contactless Chip <br> Fallback Keyed
| footer | A customizable field to display an alphanumeric message. For example, a specific terms, disclosure, or cardholder agreement statement.
| header | A customizable field to display an alphanumeric message. For example, a specific terms, disclosure, or cardholder agreement statement.
| items	| A custom item descriptor, if included in the userFields in the authorization request.
| Mode | Duplicated from the `emvTagData` object. Identifies the mode used to authorize (or decline) the transaction. Always `Issuer`.
| nameOnCard | The Cardholder's name, if included in the authorization request.
| orderNote	| A custom order note, if included in the userFields in the authorization request.
| phone | The merchant's phone number.
| PIN | Duplicated from the `emvTagData` object. Indicates the results of the last CVM performed. If PIN was entered, then "Verified by PIN"
| routeIndicator | An indicator that describes how the transaction was routed by the processor. <br> <br> Possible values are: <br> <br> Credit <br> Debit <br> Star
| Signature	| Duplicated from the `emvTagData` object. Indicates the results of the last CVM performed. If "true" then CVM supports signature and signature line may be applicable. However, card brands have moved away from requiring signature. <br> <br> Returns "unknown" if the signature requirement could not be determined. |

#### Example Authorization Response with receiptObj

```
{
   "amount": "56.50",
   "resptext": "Approval",
   "cvvresp": "",
   "respcode": "000",
   "batchid": "106",
   "avsresp": "",
   "merchid": "RCTST1000084487",
   "token": "9371752927801009",
   "emv": "8A023030910A468D746C894958073030",
   "authcode": "404592",
   "respproc": "RPCT",
   "emvTagData": "{\"TVR\":\"0200008000\",\"PIN\":\"Verified by PIN\",\"Signature\":\"false\",\"Mode\":\"Issuer\",\"ARC\":\"00\",\"Network Label\":\"AMEX\",\"TSI\":\"E800\",\"AID\":\"A000000025010802\",\"IAD\":\"06020103A40000\",\"Entry method\":\"Chip Read\",\"Application Label\":\"AMEX 2\"}",
   "receiptObj":    {
      "dateTime": "20200102130732",
      "dba": "RapidConnect Retail",
      "PIN": "Verified by PIN",
      "address2": "Princeton, NJ",
      "phone": "888-555-2222",
      "address1": "Rapid Connect Rd",
      "Signature": "false",
      "Mode": "Issuer",
      "Network Label": "AMEX",
      "Entry method": "Chip Read",
      "Application Label": "AMEX 2",
      "emv":       {
         "TVR": "0200008000",
         "TSI": "E800",
         "AID": "A000000025010802",
         "IAD": "06020103A40000"
      }
   },
   "expiry": "1224",
   "retref": "002003147252",
   "respstat": "A",
   "account": "9371752927801009"
}
```

## Date Updated: 11/21/2020 

A release of the CardPointe Gateway was deployed to the UAT environment on 11/19/2020 and to the production environment on 11/21/2020.

This release includes the following enhancements:

### cof Field Added to the Authorization Response 

The authorization response now includes a `cof` field, which echoes the `cof` value passed in the request (if present) for a card on file (stored credential) e-commerce or recurring transaction.

Possible values are:

- `C` - customer-initiated transaction
- `M` - merchant-initiated transaction

_**Note**: The_ `cof` _field is not returned in the response if it contains an invalid value._

> This field has been added to help integrators test updates to their application for compliance with the Visa and Mastercard Stored Credential Transaction Framework Mandate. This feature is currently in development and will be available in a future release of the CardPointe Gateway.
> 
> See the Visa and Mastercard Stored Credential Transaction Framework Mandate Support Center article for detailed information on this future enhancement.

### UAT AVS Response Enhancements 

This release includes enhanced AVS response logic for the following UAT emulators:

- First Data North (FNOR)
- First Data Rapid Connect (RPCT)
- First Data Nashville (NASH)
- Chase Paymentech (PMT)
- Paymentech Tampa (PTAM)
- American Express (AMEX)
- Vantiv (VANT)

See Testing AVS Response Codes in the [CardPointe Gateway Developer Guides](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md) for detailed information.

## Date Updated: 10/24/2020 

A release of the CardPointe Gateway was deployed to the UAT environment on 10/21/2020 and to the production environment on 10/24/2020.

This release includes the following enhancements:

### Support for orderid Added to the Refund Endpoint 

The refund request now supports the optional `orderid` parameter to allow you to specify a unique identifier for the refund transaction instead of retaining the order ID from the original authorization (if present).

If an `orderid` is present in the refund request, it will be stored and associated with the refund transaction record.

If an `orderid` is **not** present in the refund request, the refund will be associated with the `orderid` from the original authorization, if one was provided.

_**Note**: To return the_ `orderid` _in the response, you must also include_ `"receipt":"y"` _in the refund request._

### Details Added to the inquireMerchant Endpoint 

The inquireMerchant endpoint has been enhanced to return additional merchant configuration details, including whether or not the MID is enrolled in the Card Account Updater service and the surcharge fee amount and type, if configured for the the MID.

The `inquireMerchant` response includes the following new fields:

| Field	| Max Length | Type	| Description
| --- | --- | --- | ---
| acctupdater | 1 | AN | Specifies whether or not the MID is enrolled in the Card Account Updater service. <br> <br> Possible values are: <br> <br> `Y` <br> `N`
| fee_format | 7 | AN | Specifies the surcharge fee format. <br> <br> Possible values are: <br> <br> `flat` <br> `percent`
| fee_merchid | 16 | N | Specifies the linked MID used for processing transactions with surcharge enabled. <br> <br> if not applicable, this field is returned as an empty string.
| fee_type | 1 | AN | Specifies whether the merchant is has convenience or surcharge fees enabled. <br> <br> Possible values are: <br> <br> `C` - convenience fee <br> `F` - service fee <br> `S` - surcharge fee <br> `N` - none
| fee_value	| -	| N	| Specifies the fee value configured for the MID, where the value is either a percentage or flat amount, used to calculate the fee for each transaction.

## Date Updated: 10/2/2020 

A release of the CardPointe Gateway was deployed to the UAT environment on 10/2/2020. This release is planned for deployment to the production environment at a later date.

This release includes bug fixes and other enhancements to the CardPointe Gateway.

## Date Updated: 8/8/2020 

A release of the CardPointe Gateway was deployed to the UAT environment on 8/6/2020 and to the production environment on 8/8/2020.

This release includes the following enhancement: 

### Retrieve Funding Data for Previous Years 

The funding API request now supports the ability to specify a date in a previous year, using the `date` format `YYYYMMDD` (previously only `MMDD` was supported).

`date` is optional, and can be specified in the format `MMDD` to specify a day within the current calendar year, or `YYYYMMDD` to specify a date in a previous calendar year. 

If no `date` is specified, the service checks for any funding data that has not already been retrieved.

## Date Updated: 5/30/2020 

A release of the CardPointe Gateway was deployed to the UAT environment on 5/27/2020 and to the production environment on 5/30/2020.

This release included bug fixes and other enhancements to the CardPointe Gateway.

## Date Updated: 4/25/2020 

The following updates were deployed to the UAT environment on 4/15/2020 and the production environment on 4/25/2020.

### entrymode for Chase Tampa merchants 

The `auth`, `inquire`, and `inquireByOrderid` responses will now include the `entrymode` field for Chase Tampa merchants. 

_**Note**: This field is currently only returned for First Data North merchants._

| Field	| Content | Max Length | Comments
| --- | --- | --- | ---
| entrymode	| POS Entry Mode | 25 | **Possible Values:** <br> <br> Keyed <br> Moto <br> ECommerce <br> Recurring <br> Swipe(Non EMV) <br> DigitalWallet <br> EMVContact <br> Contactless <br> Fallback to Swipe <br> Fallback to Keyed

### Partial Authorization Responses for UAT Emulators 

The following UAT emulators now support simulating a partial authorization response:

- First Data North (FNOR)
- First Data Rapid Connect (RPCT)
- Paymentech (Paymentech)
- Paymentech Tampa (PTAM)
- Vantiv (VANT)
- Worldpay (VPS)

To simulate a partial authorization in the UAT environment, submit an authorization request using `"account":"4387750101010101"` and `"amount":"6.00"` or greater.

The following responses are returned:

| respproc | respcode | respstat | resptext                   | amount |
|----------|----------|----------|----------------------------|--------|
| FNOR     | 10       | A        | Partial Approval           | 5.00   |
| PMT      | 100      | A        | Approval                   | 5.00   |
| PTAM     | 10       | A        | Partial Approval           | 5.00   |
| RPCT     | 002      | A        | Approve for Partial Amount | 5.00   |
| VANT     | 10       | A        | Partial Approval           | 5.00   |
| VPS      | 10       | A        | Partial Approval           | 5.00   |

## UAT-Only Changes 

The following enhancements were deployed to the UAT environment on 3/10/2020.

### Amount-Driven Response Codes 

The amount-driven response codes feature will be enabled for the following UAT emulators:

- First Data North (FNOR)
- Chase Paymentech (PMT)
- Paymentech Tampa (PTAM)
- TSYS (VPS)

> This feature was previously enabled for the First Data Rapid Connect (RPCT) UAT emulator.

When testing your integration in the UAT environment, you can use amount-driven response codes to emulate processor-specific authorization responses that you might encounter in the production environment.

See [Testing with Amount-Driven Response Codes](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md#test-cases) for detailed information on this feature.

### JSON Response Randomization 

CardConnect has developed an enhancement to the UAT (non-production) environment that randomizes the response data returned to your client application. Response fields are now returned in a random order, and responses include additional randomly-generated fields, objects, and arrays containing dummy data.

The following example illustrates the modified UAT authorization response:

```
{
  "avsresp":"Y",
  "expiry":"1220",
  "respstat":"A",
  "resptext":"Partial Approval",
  "nNBcv":{},
  "lyUy":"GKmUM",
  "authcode":"-",
  "account":"9415809626691111",
  "commcard":"A ",
  "yrpkQmS":{"EMuB":"hrLGci"},
  "cvvresp":"P",
  "batchid":"270",
  "respcode":"10",
  "kePFkt":["RnaRUuCz","EVPFm","ddXcoXnc","JIVtH"],
  "BQVhs":"KZyqBI",
  "merchid":"871555880001",
  "respproc":"VANT",
  "retref":"350001238933",
  "amount":"4.00",
  "token":"9415809626691111"
 }
```

This change will require your application to parse and accept JSON response properties dynamically, rather than coding for the static position of specific properties within the response data; otherwise, your test cases in the UAT environment may encounter errors or other failures.

This change is intended to help clients identify potential failure points and to ensure that your applications are designed for backwards compatibility. CardConnect intends all changes to the CardPointe Gateway API to be backwards-compatible, assuming that client applications are designed to dynamically handle response data. 

For more information on backwards compatibility, see the [API Basics and Best Practices Guide](?path=docs/documentation/APIBasicsAndBestPractices.md).

## Date Updated: 3/3/2020 

The following updates were deployed to the UAT environment on 3/04/2020 and the production environment on 3/07/2020.

### expiry Field Added to inquire and inquireByOrderid Responses 

The `inquire` and `inquireByOrderid` responses now include the `expiry` field in the format `MMYY`.

### Changed commcard Field Values 

To simplify the use of the `commcard` field returned in the auth response, the supported values have been changed to `Y` and `N` to simply indicate whether or not the card is a Corporate or Purchase Card. 

If you previously used the `commcard` value to try to determine the card type, you can instead include `"bin":"y"` in the auth request to retrieve BIN lookup data in the response. The `bin` object in the auth response provides information that you can use to determine the type of card and other relevant details. 

## Date Updated: 1/19/2020 

The following updates were deployed to the Production environment on 1/19/2020.

_**Note**: Visit status.cardconnect.com and **click subscribe to updates** to receive important release and status notifications._

### Updated Cardholder Signature Requirements 

The CardPointe Gateway now uses the amount value to determine whether or not a cardholder signature is required, based on the guidelines for each card brand.

The EMV tag data returned in the auth response includes a Cardholder Verification Method (CVM) indicator that determines if a PIN or signature is required.

_**Note**: The CVM value is not used by Bolt terminals. In a Bolt transaction, the cardholder signature prompt is instead driven by the_ `includeSignature` _parameter in the request or a separate request to the_ `readSignature` _endpoint_.

When the CVM response returned to the CardPointe Gateway includes `signature:"true"`, the Gateway checks the amount and adjusts the signature value as follows:

- For Visa transactions
    - if the amount is **less** than **$25**, then `signature:"false"`.
    - if the amount is **greater** than or equal to **$25**, then `signature:"true"`.
- For Mastercard, Discover, and Amex transactions
    - if the amount is **less** than **$50**, then `signature:"false"`.
    - if the amount is **greater** than or equal to **$50**, then `signature:"true"`.

### bin Field Added to the Authorization and Bin Response 

The BIN service now returns the exact bank identification number (BIN) in the `bin` field in the following responses:

- The authorization response, in the `binInfo` object when `"bin":"y"` in the request.
- The BIN response.

### DPS Response Code Changes 

The following DPS Payment Express (DPS) response codes have been added:

| respproc | respcode | respstat | resptext                       |
|----------|----------|----------|--------------------------------|
| DPS      | AQ       | C        | "Amex not enabled"             |
| DPS      | D4       | C        | "User blocked"                 |
| DPS      | D9       | C        | "Invalid merchant or password" |

The following DPS response code has been removed:

| respproc | respcode | respstat | resptext           |
|----------|----------|----------|--------------------|
| DPS      | D5       | C        | "Invalid password" |

See [Gateway Response Codes](?path=docs/documentation/GatewayResponseCodes.md) for a comprehensive list of response codes.

### UAT Emulator Updates 

The following updates have been made to better align the UAT emulators with the processor host responses received in the production environment:

- The First Data Rapid Connect emulator now returns the `emvTagData` array in the auth response when an EMV test card is used.
- The Paymentech emulator has been updated to support voids for Amex authorizations.

## Date Updated: 1/9/2020 

The following update was deployed to the UAT environment on 1/9/2020.

_**Note**: This update, as well as the updates released on 12/3/19, were deployed to the Production environment on 1/19/2020. Visit status.cardconnect.com and click **subscribe to updates** to receive important release and status notifications._

### Updated Cardholder Signature Requirements 

The CardPointe Gateway now uses the amount value to determine whether or not a cardholder signature is required, based on the guidelines for each card brand.

The EMV tag data returned in the auth response includes a Cardholder Verification Method (CVM) indicator that determines if a PIN or signature is required. 

_**Note**: The CVM value is not used by Bolt terminals. In a Bolt transaction, the cardholder signature prompt is instead driven by the_ `includeSignature` _parameter in the request or a separate request to the_ `readSignature` _endpoint_. 

When the CVM response returned to the CardPointe Gateway includes `signature:"true"`, the Gateway checks the amount and adjusts the signature value as follows:

- For Visa transactions
    - if the amount is **less** than **$25**, then `signature:"false"`.
    - if the amount is **greater** than or equal to **$25**, then `signature:"true"`.
- For Mastercard, Discover, and Amex transactions
    - if the amount is **less** than **$50**, then `signature:"false"`.
    - if the amount is **greater** than or equal to **$50**, then `signature:"true"`.

## Date Updated: 12/3/2019 

The following updates were deployed to the UAT environment on 12/3/2019.

**Note**: These updates will **not** be deployed to the production environment until after January 1st 2020.

Visit status.cardconnect.com and click **subscribe to updates** to receive important release and status notifications.

### bin Field Added to the Authorization and Bin Response 

The BIN service now returns the exact bank identification number (BIN) in the `bin` field in the following responses:

- The authorization response, in the `binInfo` object when `"bin":"y"` in the request.
- The BIN response.

### DPS Response Code Changes 

The following DPS Payment Express (DPS) response codes have been added:

| respproc | respcode | respstat | resptext                       |
|----------|----------|----------|--------------------------------|
| DPS      | AQ       | C        | "Amex not enabled"             |
| DPS      | D4       | C        | "User blocked"                 |
| DPS      | D9       | C        | "Invalid merchant or password" |

The following DPS response code has been removed:

| respproc | respcode | respstat | resptext           |
|----------|----------|----------|--------------------|
| DPS      | D5       | C        | "Invalid password" |

See [Gateway Response Codes](?path=docs/documentation/GatewayResponseCodes.md) for a comprehensive list of response codes.

### UAT Emulator Updates 

The following updates have been made to better align the UAT emulators with the processor host responses received in the production environment:

- The First Data Rapid Connect emulator now returns the `emvTagData` array in the auth response when an EMV test card is used.
- The Paymentech emulator has been updated to support voids for Amex authorizations.

## Date Updated: 10/23/2019 

This release includes the following updates:

### Auth Response Changes 

The authorization response includes the following changes:

#### New Response Fields

- The response now includes the `expiry` value (MMYY) provided in the request.
- Declined authorizations can return `avsresp` and `cvvresp`.

_**Note**: This feature must be enabled for the merchant account. Declined authorizations do not return the_ `avsresp` _and_ `cvvresp` _values by default._

#### Changed Response Fields

- Declined authorizations no longer return `profileid` or `acctid`.
- For merchants with CVV verification enabled (but AVS verification **not** enabled), $0 authorizations are declined by the CardPointe Gateway, when `cvv` is not included in the auth request.

    The auth response returns `"respproc" : "PPS"` and `"resptext" : "CVV is required"`.

### orderId Included in inquire, inquirebyOrderid, void, and voidbyOrderId Response

The inquire, inquireByOrderid, void, and voidByOrderId responses now include the `orderId` included in the original auth request. 

**Note:**

- `"orderId" : ""`, If no order ID was provided in the original auth request and `"receipt" : "y"`.
- `orderId` is not returned in the response if no order ID was provided in the original auth request and `"receipt" : "n"`.

### BIN Response cardusestring Value Change

The `cardusestring` value for credit hybrid cards now returns `"Credit Hybrid (PIN and Signature)"` in the BIN response. This is also returned in the auth response when the request includes `"bin" : "y"`.

### API Basics and Best Practices Developer Guide 

To make the most of your integration, and to ensure that you develop an application that maintains backwards compatibility with the CardPointe Gateway, see our new [API Basics and Best Practices Developer Guide](?path=docs/documentation/APIBasicsAndBestPractices.md). 

You can find additional helpful information for integrating the CardPointe Gateway API in the [CardPointe Gateway Developer Guides](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md).

## Date Updated: 9/16/2019 

This release includes the following updates:

### New API URLs 

To better facilitate our partners' API testing and release validation efforts, we are enhancing the CardPointe Gateway release process. To take advantage of these enhancements, your application should no longer target port 6443 when sending requests to the UAT environment. Instead, ensure that your application targets the UAT and PROD environments using the following URLs:

- **UAT**: https://<site>-uat.cardconnect.com
- **PROD**: https://<site>.cardconnect.com

See the CardPointe Gateway Release Update notification for more information on this change.

See the [API Connectivity Guide](?path=docs/documentation/APIConnectivityGuide.md) for general information on connecting to the CardPointe Gateway and other CardConnect services using our APIs.

### Auth Request BIN Data Retrieval 

The auth request supports a new `bin` parameter to retrieve BIN data for the card used in an authorization attempt. The BIN service allows you to use a CardConnect token to determine what type of payment card is being used.

Include `"bin" : "y"` in the authorization request to retrieve BIN data. 

The authorization response includes a `binInfo` array that includes the BIN response data.

### Gateway Timeout Testing 

Merchants boarded to the First Data North and Rapid Connect platforms can now test CardPointe Gateway timeout responses in the UAT environment. 

See Testing CardPointe Gateway Timeouts in the new Testing your Integration topic in the [CardPointe Gateway Developer Guide](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md).

## Date Updated: 7/10/2019 

This release includes the following updates:

### New Voidable and Refundable Transaction Indicators 

The inquire and inquireByOrderid responses include two new fields, `voidable` and `refundable`. You can use these new fields to more easily determine whether or not a transaction can be voided or refunded in its current state.

New response values include:

| Value            | Description                         |
|------------------|-------------------------------------|
| `"voidable":"Y"`   | The transaction can be voided.      |
| `"voidable":"N"`   | The transaction cannot be voided.   |
| `"refundable":"Y"` | The transaction can be refunded.    |
| `"refundable":"N"` | The transaction cannot be refunded. |

The `voidable` value is determined by the status of the batch at the time of inquiry.

The `refundable` value is determined by the status of the batch at the time of the inquiry, as well as the MID configuration. If the MID is enabled to refund for unsettled authorizations, then `refundable` always returns **Y**.

### entrymode Field Now Included in Decline Response 

For **First Data North (FNOR)** merchants, the authorization response now includes the `entrymode` field for **declined** authorizations. Previously, `entrymode` was only returned for approvals.

## Date Updated: 5/30/2019 
This release includes the following updates:

### RapidConnect Auth Response Updates 

Authorization responses from the RapidConnect front end processor now include the `entrymode` and `bintype` fields. See the authorization response description for more information.

### Additional emvTagData Field

The emvTagData included in the authorization response data includes a new `Network Label` field, which identifies the Authorizing Network Name, which should be printed on the receipt.

## Date Updated: 5/13/2019 

This release includes the following updates:

### Additional Receipt Data Fields 

The authorization request now includes a receipt parameter, used to return additional receipt data fields in the authorization response. When you specify `"receipt" : "y"` in the authorization request, the receipt array is included in the authorization response data as well as any subsequent inquire or inquireByOrderid requests for that authorization record.

See Printing Receipts Using Authorization Data for detailed information on customizing the receipt data fields and printing receipts.

## Date Updated: 4/29/2019 

This release includes the following updates:

### Support for POST Requests 

The CardPointe Gateway API now supports HTTP POST requests for all service endpoints that support PUT requests. You can now use either PUT or POST operations to create and update resources.

### New inquireMerchant Endpoint 

The new inquireMerchant endpoint allows you to validate CardConnect merchant account configurations. This is helpful for partners who need to validate their CardConnect merchant IDs or for businesses with merchants operating in multiple locations, to ensure that the merchant ID is boarded to the correct site.

## Date Updated: 2/22/2019 

This release includes the following updates:

### Developer Documentation Improvements 

The CardPointe Gateway API developer documentation has been updated with numerous improvements, including:

- A new [Postman Collection](?path=docs/APIs/CardPointeGatewayAPI.md), to help you test and experiment with the CardPointe Gateway API.
- Revised content throughout.

### New inquireByOrderid Endpoint 

The new inquireByOrderid endpoint allows you to inquire on a transaction using the order ID included in the original request. This is helpful when the request timed out or did not return a retref, and you want to check on the status of the transaction.

### New voidByOrderId Endpoint 

The new voidByOrderId endpoint allows you to void a transaction using the order ID included in the original request. This is helpful when the original request did not return a response, and a subsequent inquireByOrderid call does not return a response. In this case, it is recommended that you call voidByOrderId three times (3x) to ensure that the transaction is voided.

### Updated CardPointe Gateway Response Codes 

The [Gateway Response Codes](?path=docs/documentation/GatewayResponseCodes.md) page now includes a downloadable text file that provides a comprehensive list of all possible response codes that can be returned from the Gateway and from all supported processing hosts. This page also includes a new topic, [Testing With Amount-Driven Response Codes](?path=docs/documentation/GatewayResponseCodes.md), that provides information for emulating response codes for testing purposes. Note that this feature is currently limited to merchants using the RapidConnect processor.

### Deprecated openBatch and CloseBatch Endpoints 

The openBatch and closeBatch endpoints have been removed from the CardPointe Gateway API documentation, but are still available for use if required for your business. The CardPointe Gateway automatically manages batches; however, if you need to integrate these manual batch management features, see Manually Managing Gateway Batches in the [CardPointe Gateway Developer Guides](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md). 
