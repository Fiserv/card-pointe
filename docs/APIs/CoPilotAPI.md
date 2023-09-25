# CoPilot API Overview

The CoPilot API uses JSON web service semantics and provides resources for boarding new merchant accounts and managing existing accounts. 

The API complements the CoPilot web application, offering a programmatic interface to some features available in the application; however, you must use the web application to complete some tasks (for example, creating application templates).

# What's New?

<!-- theme: warning -->
> Visit [Statuspage](http://status.cardconnect.com/) and click **subscribe to updates** to receive important release and status notifications.

## Date Updated: 1/19/2022 

This release contains the following updates:

### Third Party Provider Details 

The Merchant Definition now includes an additional `thirdPartyProviderDetails` object, to specify third party provider (TPP) details in a merchant create or update request. This object is also returned in a GET merchant details response. 

This change aligns with a previous update to the MPA and CoPilot to ask if the merchant uses any third party provider (TPP) to store, process, or transmit cardholder data, and if so, to provide the TPP Name and contact phone and email.

The thirdPartyProviderDetails object is optional, but requires the following fields when provided:

| Field	| Type | Description
| --- | --- | --- 
| thirdPartyProviderFlg	| Boolean | A true/false indicator the determines whether or not the merchant uses a third party provider to store, process, or transmit cardholder data. <br> <br> If **true**, `thirdPartyProviderCd`, `thirdPartyProviderEmail`, and `thirdPartyProviderPhone` are required. <br> <br> If **false**, all other fields can be `null`.
| thirdPartyProviderCd | String | A 2-character code representing the TPP. <br> <br> If `thirdPartyProviderFlg` is **true**, this field is required and must be one of the following values: <br> <br> 01 - Yahoo <br> 02 - Authorize.net <br> 03 - Cybersource <br> 04 - VeriFone <br> 05 - Merchant Link <br> 06 - Shift 4 <br> 07 - Apriva <br> 08 - FIS <br> 09 - Six Payment Services Corp <br> 10 - Verisign <br> 99 - Other <br> <br> If **99**, the `thirdPartyProviderOtherName` field is **required**.
| thirdPartyProviderOtherName	| String | The TPP name if `thirdPartyProviderCd` is **99**. <br> <br> Must be null or omitted if `thirdPartyProviderCd` is any other value. <br> <br> This field must not contain the a name from the `thirdPartyProviderCd` list (for example, "Yahoo").
| thirdPartyProviderEmail	| String | The contact email address for the TPP. <br> <br> Must be a less than or equal to 32 characters.
| thirdPartyProviderPhone |	String | The contact phone number for the TPP. <br> <br> Must be 7-12 characters.

<!-- type: row -->

<!-- type: card
title: CoPilot API Changelog
description: Visit the Changelog for more information on recent updates to the CoPilot API and documentation
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
