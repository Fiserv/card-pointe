# CardPointe Gateway API Changelog

The following entries describe changes to the [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) and documentation.

Visit status.cardconnect.com and click subscribe to updates to receive important release and status notifications.

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
