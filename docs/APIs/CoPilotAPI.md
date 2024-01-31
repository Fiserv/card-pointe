# CoPilot API Overview

The CoPilot API uses JSON web service semantics and provides resources for boarding new merchant accounts and managing existing accounts. 

The API complements the CoPilot web application, offering a programmatic interface to some features available in the application; however, you must use the web application to complete some tasks (for example, creating application templates).

# What's New?

<!-- theme: warning -->
> Visit [Statuspage](http://status.cardconnect.com/) and click **subscribe to updates** to receive important release and status notifications.

## Date Updated: 11/8/2023 

An update to the CoPilot API has been released to the Production environment on 11/8/2023. This release includes the following updates in addition to internal fixes and enhancements:

### Enhanced Field Validation 

Additional validations have been incorporated for the `legalBusinessName`, `akaBusinessName`, `taxFilingName`, and `dbaName` fields to check for string sizes that exceed the expected maximum length. The following example illustrates two possible errors:

```
{
    "errors": [
        {
            "code": "1105",
            "message": "Legal Business Name has invalid length.",
            "errorfield": "merchant.legalBusinessName",
            "status": "BAD_REQUEST"
        },
        {
            "code": "1178",
            "message": "AKA Business Name has invalid length.",
            "errorfield": "merchant.akaBusinessName",
            "status": "BAD_REQUEST"
        }
    ]
}
```

### Shipping Bill To Field 

Previously, the Shipping Bill To field was not pulled from the template. However, it is now automatically provided from the active template. There is no need to utilize the `shippingBillToCd` field in the API, and if it is used, it will be disregarded.

### Non-Swiped Auth Fee 

`cloverNonSwipedAuthFee` has been added to the fee class. This fee is only applicable for North Retail ISO merchants. 

<!-- type: row -->

<!-- type: card
title: <center> CoPilot API Changelog </center>
description: <center> Visit the Changelog for more information on recent updates to the CoPilot API and documentation </center>
link: ?path=docs/changelog/CoPilotAPI.md
-->

<!-- type: row-end -->

# Getting Started

## Authentication

The CoPilot API uses the Bearer Authentication HTTP authentication scheme in order to authenticate requests to the API. 

To obtain a bearer token to authenticate your API requests, you must create a CoPilot user in the CoPilot web application then provide that user's username and password in a request to the CoPilot API's Token endpoint. A successful request returns a JSON Web Token (JWT) which you use as the Bearer token in subsequent calls to the CoPilot API.

<!-- theme: danger -->
> We **strongly recommend** that you create a dedicated CoPilot user to authenticate your CoPilot API requests, and that you do not use this account to access the CoPilot web application.
>
> The CoPilot web application uses two-factor authentication (2FA); attempting to log into the web application with your CoPilot API user will initiate the 2FA requirement for this user, preventing it from authenticating API requests until the 2FA requirement is satisfied in the web application.

## Versioning

Whenever possible, newer versions of the API will be backwards compatible with older versions. To ensure your integration is not disrupted in the event that backwards compatibility cannot be maintained, we **strongly recommend** specifying the version in the header of each API request as part of your integration. This is accomplished by supplying the `X-CopilotAPI-Version` HTTP header with the value of the version your application uses.

### Example Header 

```
Authorization: Bearer accessTokenValue
Content-type: application/json
X-CopilotAPI-Version: 1.0
```

> - This header should be omitted from requests to the Token endpoint, as authentication via the Token endpoint occurs independently of the CoPilot API.
> - The current version of the CoPilot API is 1.0.

## Additional Resources 

In addition to this API documentation, the following resources are available to help you get started:

### Developer Guides 

The [CoPilot API Developer Guides](?path=docs/documentation/CoPilotDeveloperGuides.md) provide additional information for using the API to complete specific workflows, for example, [Using the CoPilot API to Submit a Merchant Application](?path=docs/documentation/CoPilotDeveloperGuides.md).

### Postman Collection 

To help you get started with your integration, we have created a Postman Collection that includes templates for all of the requests documented on this page.

Click the Run in Postman button to download the CoPilot API collection.

> [Run in Postman](https://app.getpostman.com/run-collection/123cd242a1b83c941022?action=collection%2Fimport#?env%5BCoPilot%20API%20(UAT)%5D=W3sia2V5IjoidG9rZW4tdXJsIiwidmFsdWUiOiJodHRwczovL2FjY291bnRzdWF0LmNhcmRjb25uZWN0LmNvbS9hdXRoL3JlYWxtcy9jYXJkY29ubmVjdC9wcm90b2NvbC9vcGVuaWQtY29ubmVjdC90b2tlbiIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoidXNlcm5hbWUiLCJ2YWx1ZSI6IklOU0VSVF9VU0VSTkFNRSIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoicGFzc3dvcmQiLCJ2YWx1ZSI6IklOU0VSVF9QQVNTV09SRCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiY2xpZW50LWlkIiwidmFsdWUiOiJJTlNFUlRfQ0xJRU5UX0lEIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJjbGllbnQtc2VjcmV0IiwidmFsdWUiOiJJTlNFUlRfQ0xJRU5UX1NFQ1JFVCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiYWNjZXNzLXRva2VuIiwidmFsdWUiOiJTRVRfQllfVEVTVFNfVEFCX09GX1RPS0VOX1JFUVVFU1QiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImFwaS11cmwiLCJ2YWx1ZSI6Imh0dHBzOi8vYXBpLXVhdC5jYXJkY29ubmVjdC5jb20iLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6ImFwaS12ZXJzaW9uIiwidmFsdWUiOiIxLjAiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Im1lcmNoYW50LWlkIiwidmFsdWUiOiJJTlNFUlRfQ09QSUxPVF9NRVJDSEFOVF9JRCIsImVuYWJsZWQiOnRydWV9LHsia2V5Ijoic2FsZXMtY29kZSIsInZhbHVlIjoiSU5TRVJUX1NBTEVTX0NPREUiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Im9yZGVyLWlkIiwidmFsdWUiOiJJTlNFUlRfT1JERVJfSUQiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Im1pZCIsInZhbHVlIjoiSU5TRVJUX0ZST05URU5EX01JRCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiYmlsbGluZy1wbGFuLWlkIiwidmFsdWUiOiJJTlNFUlRfQklMTElOR19QTEFOX0lEIiwiZW5hYmxlZCI6dHJ1ZX0seyJrZXkiOiJhcHBsaWNhdGlvbi10ZW1wbGF0ZS1pZCIsInZhbHVlIjoiSU5TRVJUX1RFTVBMQVRFX0lEIiwiZW5hYmxlZCI6dHJ1ZX1d)

See [Running the API in Postman](?path=docs/documentation/CoPilotDeveloperGuides.md#running-the-api-in-postman) in the [CoPilot Developer Guides](?path=docs/documentation/CoPilotDeveloperGuides.md) for more information on using the Postman Collection.

# Errors

When there is an error in the request, there will be a non-null `errors` array in the response body. If there are multiple field validation errors, there will be multiple error objects in the errors array. See the example below.

## Errors Definition 

| Field	| Type | Comments
| --- | --- | --- |
| code	| String	| The error code
| message	| String	| The error message
| errorField	| String	| The field that caused the error
| status	| String	| The HTTP Status Code

#### Example Error Array

```json
{
    "errors": [
        {
            "code": "1113",
            "message": "Owner date of birth is invalid.  Please use MM/DD/YYYY format.",
            "errorField": "merchant.ownership.owner.ownerDob",
            "status": "BAD_REQUEST"
        },
        {
            "code": "1115",
            "message": "Phone is invalid.  Please use XXX-XXX-XXXX format.",
            "errorField": "merchant.demographic.businessPhone",
            "status": "BAD_REQUEST"
        }
    ]
}
```

<!-- type: row -->

<!-- type: card
title: <center> API Explorer </center>
description: <center> Click Learn More to see each API service endpoint included in the CoPilot API explorer. </center>
link: 
-->

<!-- type: row-end -->
