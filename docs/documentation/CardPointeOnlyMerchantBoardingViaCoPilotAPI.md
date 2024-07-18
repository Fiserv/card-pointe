# Overview

Merchants that process transactions through the CardPointe Gateway while still maintaining their relationship with their acquirer are known as CardPointe-Only accounts.

Boarding for these accounts can be done automatically through CoPilot allowing for minimal manual interaction with the account. 

The following guide illustrates how to allow for merchants to be automatically boarded to the CardPointe Gateway through the API while still maintaining a relationship with their acquirer. 

# Requirements

To allow for merchants to be automatically boarded to the CardPointe Gateway through the API the following requirements must be met.

If these requirements are not met, the account must be manually reviewed by an internal team. 

## API Endpoint Requirements

Account information provided must include the fields that are necessary to run the Token, Merchant, and Submit CoPilot API endpoints. For more information on the exact fields needed, refer to the [API Request Details](#api-request-details) section of this document. 

# Submitting a Merchant Application via CoPilot API

For general information on creating a merchant via the CoPilot API, refer to our documentation concerning Using the [CoPilot API to Submit a Merchant Application]().

# Running the API in Postman

To help you get started with your integration, you can use the sample API Integration Postman Collection, which includes a template of the API service endpoints documented below.

See our documentation on the [Postman Collection for the CoPilot API]() for more information. 

Click the button below to download the collection:

> [CardPointe Only Boarding Postman Collection](https://developer.cardpointe.com/assets/developer/assets/CardPointe-OnlyBoarding.postman_collection.json)

# Limitations

## Merchant Service and Account Details

Since these accounts are not acquired, a **Merchant Service** tab in CoPilot does not exist for CardPointe-Only accounts.

These accounts can be seen on the **Account Details** page in CoPilot. Use the **Advance Search** ability on the **Account Details** page and filter by **CardPointe-Only**. 

Changes to these accounts such as enabling/disabling **Amex**, **Visa**, **MC**, and **Discover** flags cannot be changed in CoPilot once the CardPointe-Only account is boarded. These fees will need to be handled wherever the merchant is boarded.

# API Request Details

The following API methods are used to create and submit a CardPointe-Only merchant.

## Token

To authenticate any request to the CoPilot API, you must first request an access token using the Token endpoint. The Token endpoint requires CoPilot user account credentials and returns the access token you will use to make API requests during a session.

The access token is valid for a limited timeframe, indicated by the <code>expires_in</code> value (in seconds). Once a token has expired, you must request a new token.

> - The provided <code>client_secret</code> is a secret and should never be exposed on the client side via JavaScript. Ensure that the client secret is stored securely in your application.
> - Do not expose the <code>access_token</code> or <code>refresh_token</code> in client-side applications.
> - Create a **dedicated** CoPilot user account for authenticating your CoPilot API requests, and do not use this account to access the CoPilot web application. Using this account to access the CoPilot web application will initiate the two-factor authentication (2FA) requirement for this account, preventing the authentication of your API requests until the 2FA requirement is satisfied

| **Method** | <code>POST</code> |
| -- | -- |
|**Host** | https://accountsuat.cardconnect.com |
| **Path** | /auth/realms/cardconnect/protocol/openid-connect/token | 
| **Headers** | Content-Type: application/x-www-form-urlencoded |
| **Consumes** | application/x-www-form-urlencoded | 
| **Produces** | application/json |

### Token Request

| Field | Required | Default Value | Comments| 
| -- | -- | -- | -- |
| username | Yes | N/A | The username for the CoPilot user account used to authenticate your API requests. |
| password | Yes | N/A | The password for the CoPilot user account used to authenticate your API requests. | 
| grant_type | Yes | password | |
| client_id | Yes | merchapi | | 
| client_secret | Yes | N/A | CoPilot API registration is required in order to receive your client_secret. |

### Token Response

The JSON-encoded request will return a signed OpenID Connect JSON Web Token (JWT) as the access_token value, as well as other properties pertaining to authentication.

<!--
type: tab
titles: Example: Token Example Response 
-->

```json
{    
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIzNDZZVkUwaGJXQlVvT1NoMm5wOHF4d2NuZ2tKUjY1Uzh3SVM2UmI5X3N3In0.eyJqdGkiOiJmYmZjMjY5My04YTliLTQyOTAtOGRiMS0zM2YxODc3M2IwMTkiLCJleHAiOjE0OTkxMTYyODUsIm5iZiI6MCwiaWF0IjoxNDk5MTE1OTg1LCJpc3MiOiJodHRwOi8vYWNjb3VudHMtbG9jYWwuY2FyZGNvbm5lY3QuY29tL2F1dGgvcmVhbG1zL2NhcmRjb25uZWN0IiwiYXVkIjoiY29waWxvdCIsInN1YiI6IjZlZjdkOTkyLWZhZTMtNDVhYy1iMDA5LTIzOGIzMzIwOTA1MSIsInR5cCI6IkJlYXJlciIsImF6cCI6ImNvcGlsb3QiLCJhdXRoX3RpbWUiOjAsInNlc3Npb25fc3RhdGUiOiJjYzExYWYyMi0xMjMxLTQ4M2ItYjA0ZC01ZmYyMTIwMTM0OGYiLCJhY3IiOiIxIiwiY2xpZW50X3Nlc3Npb24iOiJmNGQ1ZWQyOC05MzM0LTQwOGYtODlmZC0zNzY0MmU3OGViNWUiLCJhbGxvd2VkLW9yaWdpbnMiOltdLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50Iiwidmlldy1wcm9maWxlIl19fSwibmFtZSI6IiIsInByZWZlcnJlZF91c2VybmFtZSI6ImFwaW9uZXNhbGVzY29kZSIsImNjVU5hbWUiOiJhcGlvbmVzYWxlc2NvZGUifQ.a6W3BihEKH4vcbFDInWIF0KmiAU3g3jtdQmBLEwEdOhOlRtsAyYVngeUxJ7pjUqqM9rv9Y8pCTAyZCZegu1Qi6ZSd3vQ1A7TKaR61LvxFXgFwuNodl-PKtJOCnSk9C1tnOEW_hlz-amBq19ydx2PoPZPqjeyopbnXFMOXvXP3_Km5gDM8JqVXlN0Awg38WpMB1SWgw5f8ZItjAVj3_9EpMi1pImO7wyjKw7KXk_O_drUPJIO6XB1vwd_tYWdNf_pAuoUPQLl9R0xGHDkIOAioXYjWnVpZ8bNExwPdSGJkkzTuTOEoWGuMsXKKFkLZxzzzzzz",    
    "expires_in": 300,    
    "refresh_expires_in": 600,    
"refresh_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIzNDZZVkUwaGJXQlVvT1NoMm5wOHF4d2NuZ2tKUjY1Uzh3SVM2UmI5X3N3In0.eyJqdGkiOiI3MTUyMTkwYy1mMzdjLTQ5MGQtOGYxZC1kMDlhMzJlYTlhYzAiLCJleHAiOjE0OTkxMTc3ODUsIm5iZiI6MCwiaWF0IjoxNDk5MTE1OTg1LCJpc3MiOiJodHRwOi8vYWNjb3VudHMtbG9jYWwuY2FyZGNvbm5lY3QuY29tL2F1dGgvcmVhbG1zL2NhcmRjb25uZWN0IiwiYXVkIjoiY29waWxvdCIsInN1YiI6IjZlZjdkOTkyLWZhZTMtNDVhYy1iMDA5LTIzOGIzMzIwOTA1MSIsInR5cCI6IlJlZnJlc2giLCJhenAiOiJjb3BpbG90IiwiYXV0aF90aW1lIjowLCJzZXNzaW9uX3N0YXRlIjoiY2MxMWFmMjItMTIzMS00ODNiLWIwNGQtNWZmMjEyMDEzNDhmIiwiY2xpZW50X3Nlc3Npb24iOiJmNGQ1ZWQyOC05MzM0LTQwOGYtODlmZC0zNzY0MmU3OGViNWUiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50Iiwidmlldy1wcm9maWxlIl19fX0.Ifoi_ZGGA2dVg1PAxDzXqjyXZtH6B0ISiDNfm7jPUJUvaFhRDkxGPWoXOoIOabbNyL-rfqS2aChDrP8OhqtxmWFBqr0e7bWTitHsruXtcXoI7gw-0rnF31LAXhhyGosnZcOqD57-UZ4tuQ3wi9luvoXw-skc59iW6EgV9_jkd3n11sIO2-lOrCoSD4OhTqrSFbf4b-duOf9UsYm1sBq3vDLQMxd4Brp7SarStCYHIe0VOBGgKBrliwwzZi0g2ucKX2jA1eMpmWng5ddQ5uZGg5jnak2-bzrx8QFMyWj5zYkWGMb5SDa5N8Kizzzzz",    
    "token_type": "bearer",    
    "not-before-policy": 1498510947,    
    "session_state": "cc11af22-1231-483b-b04d-5ff21201348f",
    "scope": "isv"
}
```

<!-- type: tab-end -->

| Field | Type | Comment |
| -- | -- | -- |
| access_token | string	| The access token value to be used with the Bearer Authentication scheme for subsequent calls to the API. | 
| expires_in | number | The number of seconds until the access token is no longer valid. |
| refresh_expires_in | number | The number of seconds until the refresh token is no longer valid. |
| refresh_token | string | The refresh token used to request a new access token. |
| token_type | string | The type of token provided. A successful request returns a bearer token. |
| not-before-policy | number | When applicable, restricts authentication prior to the timestamp value. |
| session_state | string | The session state string. |
| scope | string | The scope of use for the token, based on the user that initiated the request. |

Supply the <code>access_token</code> value in the Authorization header of your subsequent API calls, using the Bearer Authentication scheme as shown below.

```json
Authorization: Bearer <access_token>
```
## Create a Merchant Account

The following fields are uniquely required for a successful API call to create a CardPointe-Only merchant. These fields must be fulfilled in addition to any required fields for the create Merchant API endpoint in general. Review the Create Merchant Endpoint documentation for more information.

| **Method** | <code>POST</code> |
| -- | -- |
|**Host** | https://api-uat.cardconnect.com |
| **Path** | /merchantt | 
| **Headers** | Authorization: Bearer
X-CoPilotAPI-Version: 1.0
Content-Type: application/json|
| **Consumes** | application/json | 
| **Produces** | application/json |
