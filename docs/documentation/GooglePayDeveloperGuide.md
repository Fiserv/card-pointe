# Overview

This guide provides information to help you quickly integrate Google Pay<sup>tm</sup> acceptance into your Androidtm or web-based application. Adding Google Pay to your payment methods allows customers to use their stored Google Pay data to quickly and securely pay for goods and services.

# How it Works

By integrating Google Pay acceptance and CardSecure tokenization into your app, you enable your customers to pay quickly and securely using the payment data stored in a Google Pay wallet. CardSecure handles the decryption and tokenization of the customer's payment data (the Google Pay response object). Integrating Google Pay enables you to provide a quick, seamless checkout experience to millions of Google Pay users.

All transactions are available in CardPointe for reporting. Additionally, integrating the CardPointe Gateway API allows you to take advantage of the complete set of transaction and reporting features of the CardPointe Gateway.

## Developing your App

First, you build or upgrade an application, using the Google Pay API to integrate Google Pay acceptance. During that process, you configure some key parameters to integrate CardSecure's tokenization method. See [Integrating the Google Pay API](#Integrating-The-Google-Pay-API) later in this guide for more information. When your development is complete, you certify the app with Google to make it available to your customers.

## Payment Process

**1.** When a customer selects Google Pay as their payment method, your application requests that customer's encrypted payment card or wallet information.
  
**2.** Your app then passes that encrypted data to CardSecure, which decrypts and tokenizes the data.
  
**3.** Your app receives the tokenized data, and then passes it to the CardPointe Gateway in an authorization request.
  
**4.** Your app then receives a response from the CardPointe Gateway, completing the transaction.

## Testing your Integration

To test your Google Pay integration, you must enroll at least one valid payment card in your Google Pay wallet. When you use your wallet to generate a payload in the Google Pay test environment, Google returns dummy data in place of the actual card data. You then send this dummy data to CardSecure to test your integration.

# Requirements

To integrate Google Pay acceptance, you need:

- An Android or web application that integrates the following APIs:
  - [Google Pay API](https://developers.google.com/pay/api/android/overview) - to request and retrieve customer payment information.
  - CardSecure API - to decrypt and tokenize Google Pay data.
  - CardPointe Gateway API - to process payments using tokens.
- For an Android application, an Android device with Google Play services version 16.0.0 or greater installed.
- A CardPointe merchant account configured to accept Google Pay payments.

**Notes:**
Google Pay acceptance is only supported for merchants boarded to the First Data Rapid Connect platform.
Supported card networks and authorization methods are dependent on your CardPointe merchant account configuration.

> By integrating the Google Pay API, you accept the [Google Pay API Terms of Service](https://payments.developers.google.com/terms/sellertos).

## Integrating Google Pay for Android Applications

To integrate Google Pay acceptance into your application, perform the steps provided in the [Google Pay Android API documentation](https://developers.google.com/pay/api/android/overview), using the following supplemental information to integrate the Google Pay API and CardSecure with your app:

**1.** When you [choose a payment tokenization method](https://developers.google.com/pay/api/android/guides/tutorial#tokenization), do the following to connect to CardSecure:

- For the **Gateway** parameter replace `example` with `cardconnect`.
- For the **GatewayMerchantID** parameter, replace `exampleGatewayMerchantId` with your CardPointe merchant ID (MID).
      
**2.** When you [define supported payment card networks](https://developers.google.com/pay/api/android/guides/tutorial#supported-card-networks), do the following:
    
-  For **getAllowedCardNetworks**, specify the card types that your merchant account is configured to accept.
- For **getAllowedAuthCardMethods**, specify both `PAN_ONLY` and `CRYPTOGRAM_3DS`.
      
**3.** When you [describe your allowed payment methods](https://developers.google.com/pay/api/android/guides/tutorial#allowed-payment-methods), do the following:
    
- For **getBaseCardPaymentMethod**, specify
  `cardPaymentMethod.put("tokenizationSpecification", getTokenizationSpecification());`.
    
**4.** Configure your app to [handle the response object](https://developers.google.com/pay/api/android/guides/tutorial#paymentdata). See [Tokenizing Google Pay Data](#Tokenizing-Google-Pay-Data) for more information.

## Integrating Google Pay with a Web Application

To integrate Google Pay acceptance into your application, perform the steps provided in the [Google Pay web API documentation](https://developers.google.com/pay/api/web/guides/tutorial), using the following supplemental information to integrate the Google Pay API and CardSecure with your app:

To integrate Google Pay acceptance into your application, perform the steps provided in the [Google Pay Android API documentation](https://developers.google.com/pay/api/android/overview), using the following supplemental information to integrate the Google Pay API and CardSecure with your app:

**1.** When you [choose a payment tokenization method](https://developers.google.com/pay/api/android/guides/tutorial#tokenization), do the following to connect to CardSecure:
   
- For the **gateway** parameter replace `example` with `cardconnect`.
- For the **gatewayMerchantID** parameter, replace `exampleGatewayMerchantId` with your CardPointe merchant ID (MID).
  
  > The Google Pay Web API includes a `MerchantInfo` object with an additional `MerchantID` parameter used to specify a Google Pay merchant identifier generated by Google when you create your Google Pay merchant account. See the [Google Pay Web API documentation](https://developers.google.com/pay/api/web/reference/request-objects#MerchantInfo) for more information.
    
**2.** When you [define supported payment card networks](https://developers.google.com/pay/api/android/guides/tutorial#supported-card-networks), do the following:

- For **getAllowedCardNetworks**, specify the card types that your merchant account is configured to accept.
- For **getAllowedAuthCardMethods**, specify both `PAN_ONLY` and `CRYPTOGRAM_3DS`.
    
**3.** When you [describe your allowed payment methods](https://developers.google.com/pay/api/web/guides/tutorial#allowed-payment-methods), do the following:

- For **cardPaymentMethod**, specify the tokenizationSpecification as follows:
```json
const cardPaymentMethod = Object.assign(
  {tokenizationSpecification: tokenizationSpecification},
  baseCardPaymentMethod
);
```

**4.** Configure your app to [handle the paymentData response object](https://developers.google.com/pay/api/web/guides/tutorial#event-handler). See Tokenizing Google Pay Data for more information.

### Sample Google Pay Tokenization Response
```json
HTTP/1.1 200 OK
Content-Type: application/json

{
  "message" : "No Error",
  "errorcode" : 0,
  "token" : "9417119164771111"
}
```

## Google Developer Resources

Google Pay provides APIs for both Android and web applications, and comprehensive developer documentation for each version of the API. The specific Google Pay API integration details depend on the type of application you are developing. 

See the following resources for detailed Google Pay API integration details:

- [Google Pay API for Android Overview](https://developers.google.com/pay/api/android/overview)
- [Google Pay API for Web Overview](https://developers.google.com/pay/api/web/overview)

# Processing a Payment

Once you receive a token from CardSecure, you can pass the token to the CardPointe Gateway in an authorization request.

You use the CardPointe Gateway API to make a request to the `auth` endpoint. To simplify your integration, you can download and test our sample mobile applications and server side host scripts. See the [CardPointe Mobile Mobile SDKs Developer Guide](?path=docs/documentation/CardPointeMobileSDKs.md) for more information and examples.

See the CardPointe Gateway API Developer documentation for detailed information on the authorization request and response data.

# Support

For assistance with your CarPointe merchant account, or integrating CardSecure and the CardPointe Gateway, contact isvintegrations@fiserv.com.

For assistance with integrating the Google Pay API, see the [Google Pay API Troubleshooting page](https://developers.google.com/pay/api/android/support/troubleshooting) or complete the [Google Pay API Integration Support Request Form](https://services.google.com/fb/forms/googlepayAPIsupport/).

<!-- theme: warning -->
> This guide includes integration examples using the Google Pay API for Android, as described in the [Google Pay API for Android Integration Tutorial](https://developers.google.com/pay/api/android/guides/tutorial).

> See [Test with Sample Tokens](https://developers.google.com/pay/api/android/guides/resources/sample-tokens) in the Google Pay API documentation for more information.

# Integrating the Google Pay API

The following topics describes integrating Google Pay with either an Android application, or a website or web application.

<!-- theme: warning -->
> The following procedure uses examples from the [Google Pay API for Android Integration Tutorial](https://developers.google.com/pay/api/android/guides/tutorial). If you are developing a web application, see the [Google Pay API for Web Integration Tutorial](https://developers.google.com/pay/api/web/guides/tutorial).

# Tokenizing Google Pay Data

When your application receives the encrypted Google Pay payload, you pass the data to CardSecure, which decrypts and tokenizes the data.

<!-- theme: danger -->
> You must retrieve a new Google Pay payload for each tokenization attempt. Tokens generated for Google Pay payloads are valid for a single authorization.

Your application passes the Google Pay wallet data to CardSecure in a request to the tokenize endpoint.

## CardSecure Request URL

https://<site>.cardconnect.com/cardsecure/api/v1/ccn/tokenize

## CardSecure Request Method

POST

## CardSecure Request Parameters

The following parameters are required in the tokenize request:

| Field | Description |
| ----- | ----------- |
| device data | A string including the Google Pay payload data. |
| encryptionhandler | The decryption method for CardSecure to use to handle the encrypted data. This **must** be `EC_GOOGLE_PAY`. |

<!-- theme: danger -->
> Do not URL encode the `devicedata` string. See the following example to ensure that you format the string properly.

### Sample Google Pay Tokenize Request

```json
POST /cardsecure/api/v1/ccn/tokenize HTTP/1.1
Content-Type: application/json

{
"encryptionhandler" : "EC_GOOGLE_PAY",
"devicedata" : "{\"signature\":\"MEYCIQCwmJRWgG8cT1et/SgjLXr8+dmZ2BZpiLEg/T474g2NZAIhAKVmDiozWuQoPED7qaGNDyoYslL2YzHSFM724Md89+33\",\"intermediateSigningKey\":{\"signedKey\":\"{\\\"keyValue\\\":\\\"MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEgY3czp0xq5QW3NTQgYvmDJ2i+Oj3YtFwfXHed6ZjtDIju/FkfPIT66AOAAEIe2UqS8dTL/AZkM98KAp4LdekAQ\\\\u003d\\\\u003d\\\",\\\"keyExpiration\\\":\\\"1585761306143\\\"}\",\"signatures\":[\"MEYCIQDb+LBzB21jEBRr0r/RqH6QDoYWqpcY5nJFdFKIpNmB5QIhAN3RdiHK0bl6kBigXnIe8qUEnrGqdC6q5NQWJHwEhF12\"]},\"protocolVersion\":\"ECv2\",\"signedMessage\":\"{\\\"encryptedMessage\\\":\\\"mJVt1VLA/CJMosu8s/C3ixVgNHW3ZuJSBx4mSU8HbQtB1Ll9jV0jgeSZ9CVnmCr9w9RiPKvdo1mJGz69aNky4oYMKt/2gUWsRDMKf0LOktjYQ9kLUpyJvkX5YGrwkeL12qUceIYcMX84L+tlV+FVVfhCcxsDNWKnKSxqzP5/KAN3is6YQ5YnTxfz7xEVXTFoAHv78XBowQq2GSioK7uV2MubHO+o5+G5+i/OJBNMsZevM27nE8gO5OQUOugkX7/cLbFHYlvJEpy7rWHj7yUV9r7eeji2uC0cKorOGdgoFjY6Hax8gtwiBJM56TlkChOA6JI8e3pO5a3r+ZkSMB95c/lAOSbesush02KNvIAKan5A6435mQ7VnQK3FJcX3s7cGO0yP2FHnbki+Oewzfoix1tNg1WuNiPXk2Cn1IM4cvk+GErEqDG1Uqh1KGb/P4F/bBDtwiqKR8FP/1dIVtgj8gi/sRG55Nm+SfRIprXv3g\\\\u003d\\\\u003d\\\",\\\"ephemeralPublicKey\\\":\\\"BK9KRSzyuwWyy9LUh2S2ue7M02xheyVtn42plZb6bp0EhZUyu0iL0QsvDsczs2fPGtJ3h0GsC9NE1Oa0BbMoIHs\\\\u003d\\\",\\\"tag\\\":\\\"KVHidXy9urg15Sjw/DeibMgxuqw73VajbEN/NZ7YEik\\\\u003d\\\"}\"}"
}
```

# Trademark Information

Android<sup>TM</sup> and Google Pay<sup>TM</sup> are trademarks of Google LLC.

Google Play<sup>TM</sup> and the Google Play logo are trademarks of Google LLC.
