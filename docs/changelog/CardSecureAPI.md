# CardSecure API Changelog

The following entries describe changes to the [CardSecure API](?path=docs/APIs/CardSecureAPI.md) and documentation.

## Date Updated: 9/23/2023 

An update to CardSecure was released to the UAT environment on 9/19/2023, and is planned for release to the Production environment on 9/23/2023.

This update includes backend improvements and enhancements. 

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

See the [tokenize](../api/?type=post&path=/v1/cnn/tokenize) description for a complete example, and see the [Apple Pay Developer Guide](?path=docs/documentation/ApplePayDeveloperGuide.md) for detailed information on integrating support for Apple Pay, and formatting the payment token string for CardSecure. 

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

## Date Updated: 8/12/2019 

This release includes the following updates:

### Support for CVV and Expiry 

CardSecure now supports tokenizing the CVV and Expiry values for payment cards.

The CVV and Expiry values are stored in the token until the token is used in an authorization attempt, after which this information is cleared from the token.

The tokenize request now supports the following optional parameters:

| Field	| Content	| Max Length | Description
| --- | --- | --- | --- |
| cvv	| Card Verification Value	| 4 | Optional, the card verification value (CVV). <br> <br> Must be a 3 or 4 character numeric string. <br> <br> Alphanumeric and special characters are not supported. <br> <br> Strings greater than 4 characters or fewer than 3 characters are not supported.
| expiry | Card Expiration | 8 | Optional, the card expiration date. <br> <br> Must be a numeric string. <br> <br> Must be formatted as: <br> <br> MMYY <br> YYYYMM <br> YYYYMMDD

### Support for Google Pay 

CardSecure now supports tokenizing Google Pay wallet account data. 

To tokenize a Google Pay account, use the `devicedata` parameter to pass the Google Pay data, and use the `encryptionhandler` parameter to pass the value `"EC_GOOGLE_PAY"` to allow CardSecure to decrypt and handle the encrypted payload.

See the [tokenize request description](../api/?type=post&path=/v1/cnn/tokenize) for more information and an example. 

See the [Google Pay Developer Guide](?path=docs/documentation/GooglePayDeveloperGuide.md) for detailed information on integrating Google Pay acceptance with a mobile app.
