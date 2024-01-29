# Apple Pay Developer Guide

This guide provides information for integrating Apple Pay acceptance with an iOS or web-based application. 

The [CardPointe Mobile iOS SDK](?path=docs/documentation/CardPointeMobileiOSSDKDeveloperGuide.md) supports the ability to generate a token from data retrieved by Apple Pay, simplifying the integration with your iOS app. 

## Requirements

Before you begin, consider the following:

- Currently, Apple Pay is only supported for **First Data Rapid Connect** merchants in the **US**. 
- You must obtain a Certificate Signing Request (CSR) file from our Integration Delivery team. You use this file to generate an Apple Pay Payment Processing Certificate, and you must send the public key file for this certificate back to your representative.

> Contact integrationdelivery@fiserv.com to request the required CSR file.
> 
> Note that this process can take up to 5 business days.

- You must must create and configure an Apple Pay Merchant ID, and you must create a provision an Apple Pay Merchant ID (with appropriate Identifier and Profile) and Payment Processing Certificate prior to submitting tokenization requests.
- Using either the CardPointe Mobile SDK or a combination of the [CardSecure API](?path=docs/APIs/CardSecureAPI.md) and [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md), your solution must integrate CardSecure to tokenize Apple Pay data and the CardPointe Gateway to use the tokens in authorization requests.

## Limitations

The CardPointe Gateway's Apple Pay integration includes some important limitations. Before you begin, review the following limitations to ensure that this solution meets your needs.

### Transaction Limitations

The following Apple Pay transaction types, as described [here](https://developer.apple.com/apple-pay/planning), are currently not supported by the CardPointe Gateway:

- Partial shipment
- Fixed subscription
- Flexible frequency subscription
- Managed subscription

### Token Limitations

As described [here](#integrating-apple-pay-using-the-cardsecure-api), CardSecure decrypts the Apple Pay payment data and generates a token that your application uses to make an authorization request to the CardPointe Gateway. Note the following limitations for Apple Pay tokens:

- A unique Apple Pay Device Personal Account Number (DPAN) is generated for each transaction; therefore, tokens generated for the unique DPAN should not be stored for reuse or saved to customer profiles.
- You must tokenize the Apple Pay payload within 2 minutes of retrieval. Attempting to tokenize an expired Apple Pay payload results in an `"decryption failure"` error response from CardSecure.
- You must retrieve a new Apple Pay payload for each tokenization attempt. Tokens generated for Apple Pay payloads are valid for a single authorization.

# Configuring an Apple Pay Merchant Account

Perform the following procedures to create and configure an Apple Pay merchant account.

## Creating an Apple Merchant ID

**1.** Log in to https://developer.apple.com/account.

**2.** Select the **Certificates, Identifiers & Profiles** link.

**3.** Select the **Merchant IDs** link under **Identifiers**.

**4.** Click the + icon to create a new Merchant ID.

**5.** Enter a description and identifier and click **Continue**.

**6.** Click **Register**.

## Creating an Apple Pay Payment Processing Certificate and Uploading the CSR

**1.** Log in to https://developer.apple.com/account.

**2.** Select the **Certificates, Identifiers & Profiles** link.

**3.** Select the **All** link under **Certificates**.

**4.** Click the + icon to create a new certificate.

**5.** Select the **Apple Pay Certificate** radio button and click **Continue**.

**6.** Select the Merchant ID that you created in the previous procedure.

**7.** Click **Create Certificate** in the **Payment Processing Certificate** section.

**8.** On the **Generate Certificate** page, click **Choose File**..., select the CSR file that you received from us (apple-pay-.csr), and click **Continue**.

**9.** Click **Download** to download the signed public key file (apple_pay.cer) and click **Done**.

**10.** Send the apple_pay.cer file that you downloaded in the previous step to your Integration Delivery representative.

> To test your solution using test (non-production) Apple Pay data, you must use the Apple Pay Sandbox environment. Attempting to tokenize test data generated in the Xcode simulator will fail. See the [Apple Pay Sandbox Testing](https://developer.apple.com/apple-pay/sandbox-testing/) documentation for more information.
 
# Integrating Apple Pay using the CardPointe Mobile iOS SDK

This topic provides information for adding Apple Pay to an iOS application using the [CardPointe Mobile iOS SDK](?path=docs/documentation/CardPointeMobileiOSSDKDeveloperGuide.md).

## Configuring the Application

Do the following to add the Apple Pay capability to your merchant ID and provision your application:

**1.** Within your Apple Developer account, enable Apple Pay in your provisioning profile with the merchant ID you configured for use with the CardPointe Gateway.

**2.** Enable Apple Pay under **Capabilities** by selecting your merchant ID from the list. If it doesn’t appear, find the `{app_name}.entitlements` file that was generated and add a string with your merchant ID in the merchant IDs array key.

**3.** Ensure your application is set up to run with your provisioning profile and run the application or use the simulator.

## Creating a Custom Flow

The CardPointe Mobile iOS SDK includes a function called `generateTokenForApplePay:completion:`. This function takes the Apple Pay PKPayment object and generates a CardSecure token. For information on creating a custom workflow, see [Apple's Apple Pay Programming Guide](https://developer.apple.com/library/archive/ApplePay_Guide/).

## Creating an Integrated Flow

The CardPointe Mobile iOS SDK's integrated UI supports a basic Apple Pay workflow. It handles display of the Apple Pay UI, token generation, and response of the result. Your application needs to make the authentication request using the API bridge class and forward the response.

The following procedure provides general guidance for creating an integrated Apple Pay workflow.

**1.** Follow the initial setup described in the [Apple Pay Programming Guide](https://developer.apple.com/library/archive/ApplePay_Guide/) to enable Apple Pay in your application.

**2.** Create a `BMSPaymentRequest` object and set your merchant ID and amount to applePayMerchantID and Total respectively. An `additionalData` parameter is provided that can be used to store a reference, typically a transaction or order number.

**3.** Before displaying your `BMSPaymentController`, set your `BMSPaymentRequest` to its `paymentRequest` parameter.

**4.** In your class that conforms to the `BMSAPIBridgeProtocol`, include `BMS_authApplePayTransactionWithToken:`completion to use the token provided to make your authentication request and return the result in the completion block:

**5.** You can add the optional function `paymentController:finishedApplePayWithResult:` to your `BMSPaymentController` delegate to receive notifications for Apple Pay transactions.

**6.** You can also customize how the Apple Pay UI is displayed using your `BMSTheme` class and the parameters `applePayButtonDescription`, `applePayButtonStyle`, and `applePayButtonType`.

## Troubleshooting

If the Apple Pay button doesn’t appear on the initial payment controller screen, enable debug logging on `BMSAPI`. When the payment controller screen displays, it will include a debug log to help troubleshoot.

Some related log messages include:

- “applePayMerchantID not set.”
- “total not set or <= $0.”
- “Device doesn’t support payments.”

# Integrating Apple Pay using the CardSecure API

This topic provides information for adding Apple Pay to your website or application using the CardSecure API. Apple Pay tokenization makes use of [CardSecure's RSA encryption](?path=docs/APIs/CardSecureAPI.md#tokenizing-and-encrypting-pan-data) capabilities. A similar decryption process handles the encrypted data returned from Apple.

Before you begin, review the Apple Pay [Payment Token Format Reference](https://developer.apple.com/documentation/passkit/apple_pay/payment_token_format_reference).

<!-- theme: danger -->
> A unique Apple Pay Device Personal Account Number (DPAN) is generated for each transaction. Tokens generated for the unique DPAN must not be stored for reuse or saved to customer profiles.

## Making a CardSecure Tokenization Request

To pass the Apple Pay payment token to CardSecure, your application must first parse the data to retrieve and the required properties and reformat them in an encoded string. Your application includes this string in the devicedata field in the CardSecure tokenize request.

### CardSecure Request URL

https://<site>.cardconnect.com/cardsecure/api/v1/ccn/tokenize

### CardSecure Request Method

POST

### CardSecure Request Parameters

The following parameters are required in the tokenize request:

| Field | Description |
|-------| ------------|
| devicedata | A string including the Apple Pay payment token data. |
| encryptionhandler | The decryption method for CardSecure to use to handle the encrypted data. This **must** be `EC_APPLE_PAY`. |

<!-- theme: danger -->
> Do not URL encode the devicedata string. See the example later in this topic to ensure that you format the string properly.

## Formatting the Apple Pay Tokenization Request

To use CardSecure to tokenize an Apple Pay payment token, you must pass specific properties from the payment token object to CardSecure in the `devicedata` field in the tokenization request as follows:

```json
"devicedata":"<data>&ectype=apple&ecsig=<signature>&eckey=<ephemeralPublicKey>&ectid=<transactionId>&echash=<applicationDataHash>&ecpublickeyhash=<publicKeyHash>"
```

The following table describes the correlation of each Apple Pay payment token property and its corresponding property in the `devicedata` string:

| CardSecure Request Property | Apple Pay Payment Token Property | Description |
| --------------------------- | -------------------------------- | ----------- |
| Not labeled | data | The `data` string in the Apple Pay response, which includes the encrypted payment data. This field **must** be the first value in the CardSecure `devicedata` string and is not labeled. | 
| ectype | N/A | The encryption type, which must be `apple`. This field is not present in the Apple Pay payment token. |
| ecsig | Signature | The signature of the payment and header data. The signature includes the signing certificate, its intermediate CA certificate, and information about the signing algorithm. |
| eckey | ephemeralPublicKey | The ephemeral public key bytes. |
| ectid | transactionId | The transaction identifier, generated on the device. |
| echash | applicationData | This field is optional, but must be included if the `applicationData` is present in the Apple Pay payment token. `applicationData` is the hash of the `applicationData` property of the original `PKPaymentRequest` object. If the value of that property is null, this value is omitted. |
| ecpublickeyhash | publicKeyHash | This field is optional, but must be provided included in the Apple Pay payment token. `publicKeyHash` is the hash of the X.509 encoded public key bytes of the merchant’s certificate. |

> Version 1.0 of the Apple Pay API does not return the `echash` and `ecpublickeyhash` values. If you are using Version 1 of the Apple Pay API, do not include these values in your CardSecure tokenization request.
> 
> If you have an `ecpublickeyhash`, you **must** provide the `echash` property in your request, even if it is empty (for example, `"...&echash=&ecpublickeyhash=<publicKeyHash>"`).

<!-- theme: danger -->
> You must tokenize the Apple Pay payload within 2 minutes of retrieval. Attempting to tokenize an expired Apple Pay payload results in an `"decryption failure"` error response from CardSecure.
>
> Additionally, you must retrieve a new Apple Pay payload for each tokenization attempt. Tokens generated for Apple Pay payloads are valid for a single authorization.

### Sample Apple Pay Tokenization Request

```json
POST /cardsecure/api/v1/ccn/tokenize HTTP/1.1
Content-Type: application/json

{
"encryptionhandler" : "EC_APPLE_PAY",    
"devicedata":"VDlOajZ3MzVzT3R5TFBJUmxJc2tES2RE\/VdZY2pwdmZ1b1JLMWlFYllZajFoRVg5N0YxSzBDR21pSjRjTHc1NEg5OThsTFBsRVZQb2wzaW9sVXY3eGdWbWN1eWhHTGI1WnJKamZGMXBqdUNnYlBRaHNHQm0yVG9o\/kZxWUwxblJvU2hidGFOMnFjczM2SnV4UE44T3hxY3FSSXNnMGtKS2tXenk4akYxeHBnUXJGZEI0c3RTaDhHM0VnVXdub3lFOUhWM0ZITkx2RnoxTWFPcDR6Y3owbklteF\/KMm9pT3ZUang5dHNFOWl3cktTMFNMakR6VENxU2hZWWxXYnBob0pYRWRqcVJtSG9kNEdncnBqWVBwT0tPczlKWHhrU0hNUTFJMzJGTHRhSkdTV1pPUVVaSlNEUmJFbFd4OUJBQmV3Nk9wT&ectype=apple&ecsig=dGlzOWkxUnRvRGlJbDc5ZllQ\/UpQZGFzQW51OGt6OXd2b0d6S2V0TU5UNU5ZeGhtUkN4MnE5d0NSb0xMbGlhbEVLNVJJUXYzQ0o4VGdKa1JWTm9rc3hYYjRZM1Fsam5mNlhnY2puRGNITzJlc2kwem1RelBmeWFFUUNLNmFxczU1QkJzQ2p6RVFNdE8zYnh0bm1kTjh5bHpDd1l4MWs5azdSOEU4RmgwRUZSVHRmYUtqRzJiMzd4WmFuWDA5ZW5paVcxRVNMdzVCRlkxcTN5d0J0WUpYUlVXRzBEOG9rQzhQdlkxQTJGS09FMHhHVUwyMnVjbWlDakJOZmdGb0U4aFFLOUJqSHgzUnRCU0tLeWFRaGxzeE\/zOGNHa1RVTVNuTVpoZ2hiUmhOazMxRTRoMjREU2NWbWdibWI3TXdUNjMyUm80OGpRbzBOMlA5WXZ5OUVJTllPOHN5bGYyY0s0VjhuUG96YzRSYTlQWVcwVXdxOUo0VW5lWUs4RzFNNjJ4VUFtZ3lpT0ZGYWRzWmJqcWJnNlBpdDVXQk5keXlZTkhsWlhqaU5Od3QyZFNRQ2FpbzJPQVoxc09UdFFldzA3OTJocW0xMXM5TVR5dWxXZXB0bEZjQmx0cm56SUM0cG52NWFtcXE2SW40cWlJMjk3a2JVRk1SSGpOM3hjSHdwR1RuVXVubUo3Q28xeGpwenVoRURmd0ZjUFBGRzhvSHBMVnJpSnZxVXRQdHp1N0dvdExhSXBDU2dwdzBkRlVrUWJEQ2JwWmY5WjNyWlo1ZnVqQjVFZjhKMjE3aHZNcEtaVm9NZXZvNGZZMVVrTHFzUlJaa3FxZWU0dFRNbjFJZDk3UGl5R2VQMElNVVBxVGdkY0h4VVJhRFlxT2hmdU9adkdXSzA0Q3c1RVFteEYwZk8zRVQ0TjJhbUFjalF3VUl6UmpnT1VQeWRVMERacVFwbXpLR0FZZlF1ano0QmozeXp6VE54YzkzMk53YTUzVVo4QUQ4RUl1Vk51MFBjUFc3Y1BYd3J4QzRtV2NlbERwNG9ablczd3hDMTdRNERmdjBKOVp6NWE0ZmJtcEJHUnNLVmJTenVPOEt6NW1aTldjcmRWTXhYbWNI\/1FUSEFvOGtyUmJwOWl3ZEFJZVQ5S0FmQTV1YUxIZmFUZ2RZbjMwNUhuRnZXQTRuOG9saVBxbVNUeWVlV1h1V0t3ZHQ4ZjBSdnlBSEVpRUUwMXNEeHNsVndxRnN3OXNkSWFxOU45cTFBQldJQVBrcEFYQTJZenRBUHpRT05YZGlLVUhzM21NbGxvTU1BdFFGNkNlMmlvUmVyMkN2SXN4N1VCV0puRW1qU0lnR0VteUxaY1dQTmxTTGVxSFZxZHRVM1ZuaTlhc2RLUFpDRGRVN2RqaU1taG5wQTJrcUxhNmJmamo4ZlJlMENIVlB5S2plWmZnRzlXRGs0bHE0aFJDQ3A2amtFc2I3SDlCeWN3alJLV3F1azVIazIzTHNpbWRkRWtidmxIbWV2S2dCZzlEeVNpTmVaRzRaRWtHeXdGWE54Z1kydTFzZENnaXZyTjdhTjNQcHl5YWJoblp6cUVVSVlyT1N4ZEZETWhiaFp4SmJhV041TTVYSGJmN0J5WWZHNUhaQ1FYNmFvdkFXdVZCVjE3c2JWM2dzSWNmQVBaZ3dIODQwSkJQWlIyN\/hvaW9GUVk3OHZmVjVuWFFZbnMxS2RuMmF0ZkxIV2NJNzRmb29RQXQ2NHJtaXpDeHJqQW95YkYySnJNdDNFaTJYWkJlZlBLS0lIZTBSUW5HRVY5WW9xMDhhUE1pRVFCeWFBNndoeXBiNENxWFZoYW9EUnRSWENYNExoZGFnR1VLQ2NZNFpnSHU3NXBHYnI2bWpMMHlXVFo3YnVoSkVWZVkwQ1p3QUhQT0tGMHMwWGQzV2g3ektjeTBRbXZOekJ0TWdmclNnNExvQkdvSENGUFNOOFkwNzdBZVNNMDQzcnE2V2h4TXhKTkE0eDNZZzIzRkhQeHp3Ynh4djc0N004UTFxU2g0SHQxODFMQ052NFZMMFZNZnJnM0FBWWM1YnI4VW5scU5iNlc3d2FsZG5kNjlGNEZjaDhiTDAwcTlSNUVqbUQ3WkVSVElLeXlqWTczWTNBTmFvN05BY2FBSWFqNnhkWDkyT2dmdjU0ekhhSjRyOXgyUkEzYUg4NHh2ZzFrQTAxWGI3WFY4TTE4R3g5WldtejdGSUNqY0RJVnlaaWhKQWhsZWg3ZjFuYTI3WE00QzhUWWgwYzdtTXZ0TmNHVG5sblFZYXVrUVpYSWhieDEzWHVwSEhFUkFabjB1VHptUVlFWnNJSzBXTk5nbXk0YnMybEpsbHZnaHlhd29SbkZnbkNGY\/NiODZweUJ5c0dxeVUwMEFjQUFJMHk2dTB3SWlnazNWbE5XVGhSclZLdHo5aGlNa3JXekxBdm1KN2F3ZHc3bjN2UzIxQkhOTnh2YklBdHJvcXB1ZFBUaWlPb2I2c1ZodnVnRlpvVjgwWW8wa1lpRXo0WHF4Z1lXd3pLdklPcGVJTXpPdzBIUDU5RUJZcktDVzNMUXRLN3BPdzNTVWVoNU80Q2NWOVJSOUFlanBkNU9hT1hlZ1BCQVVkNEI3OUVFbk51RW5uYWFuVzNvZVlNOEl6WUtYZ3RtQVdqb3FQOTY3emQ2OFQxUGVSZUlJaDJPcDU0bFFGdFJTR25lQ1VPbGJhVG5hQmNpc0tBdFRQYXVwdEZ3b2RYd290NXZocDFXTTRoTnVWcGl3RzFIUHdzSjZjYU1qR1BFZjdIVWY2UVhTVm1Mb3FoRGpiWjNyUmd4akl4V1hyQzR5d3BFdFlDU0xuTGpGWEkxSTFkNFZKMmY5RTJYcndRZE9aY3ZTYUtWTUtzbklDZVJGWTZsZlFCckhlVkx1bmh0MEhqQXhv\/nBMTV\/acWxDcnB4ZnBkbW9OcFY0U1JJWjlBR3ZOaFo2NEV&eckey=VWZOUlkySmh1TEVaT0IyM2NlS3k1S3Bvd1B4aWx6MUs4VTRVa01hd1lpOGQ0dkZwZXFWbHliUXplNGNLQUww4UmczTmptcFB2ZUh2cmo4YTQ1dmF6WFNKaGwzcAo=&ectid=386c441cc42971d6f59b54e2b64c0b88fcb1108b41e61be8d3875b2328ad79a1&echash=&ecpublickeyhash=3Ahi2ZJsqPUsQY7FY5nl2o\/rM3NwxitS7jFHUt1JNRk="
}
```

### Sample Apple Pay Tokenization Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
  "message" : "No Error",
  "errorcode" : 0,
  "token" : "9417119164771111"
}
```

## Making a CardPointe Gateway Authorization Request

Once you obtain a token, you can pass that token in an authorization request to the CardPointe Gateway. See the [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) for more information.
