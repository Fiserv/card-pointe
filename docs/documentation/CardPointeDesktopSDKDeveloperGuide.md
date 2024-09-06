# CardPointe Desktop SDK Developer Guide

The CardPointe Desktop SDK enables you to quickly and easily integrate secure payment card tokenization and card-present payments using the CardPointe Desktop (ID TECH Augusta) card reader device.

This guide provides information to help you integrate the CardPointe Desktop solution with your application.

# Overview

The CardPointe Desktop SDK integration consists of the following components:

- The CardPointe Desktop EMV/MSR card reader (ID TECH Augusta)
- Your point-of-sale application, which integrates one or more of the following services to tokenize payment cards and authorize payments:
    - [CardSecure API](../docs/APIs/CardSecureAPI.md)
    - [Hosted iFrame Tokenizer](?path=docs/documentation/HostediFrameTokenizer.md)
    - [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md)
      
See the [CardPointe Desktop Integration Guide](#cardPointe-desktop-integration-guide), below, for more information.

## CardPointe Desktop Card Reader

The CardPointe Desktop card reader device (ID TECH Augusta) is an EMV (chip) and MSR (magnetic stripe) card reader device that offers a simple upgrade path to EMV from traditional countertop magnetic stripe readers. The device encrypts card data at the point of interaction, ensuring that sensitive cardholder data is never exposed to your environment. Additionally, the device supports EMV Quick Chip, allowing you to accept EMV payments much more quickly than standard EMV.

<!-- theme: warning -->
> See the [CardPointe Desktop Device User's Guide](https://support.cardpointe.com/integrated/desktop/cardpointe-sdk-desktop-device) for information on using and troubleshooting the device.

The device uses keyboard emulation to output the encrypted card data string from the device into a field, as though you manually typed the string into the field using your keyboard.

The CardPointe Desktop card reader is preconfigured with the following required settings:

- **USB Keyboard (KB) Mode** - This setting enables the device to act as a keyboard, returning data to your application as though it was typed on a keyboard.
- **Quick Chip and M/Chip Fast Transaction Mode** - This setting enables the the device to read EMV cards in Quick Chip mode and automatically output the data to your application.
- **Auto Mode** - This setting enables the device to automatically output track data for MSR card reads to your application.

<!-- theme: danger -->
> These settings are **required** for the CardPointe Desktop solution to function as designed, and must **not** be modified.

# CardPointe Desktop Integration Guide

The CardPointe Desktop solution supports the following integration methods, providing flexible tokenization and payment functions to your application.

- **CardSecure API** - Integrate the CardPointe Desktop device and [CardSecure API](?path=docs/APIs/CardSecureAPI.md) with your application to capture and tokenize encrypted MSR track or EMV tag data.
- **Hosted iFrame Tokenizer** - Integrate the [Hosted iFrame Tokenizer](?path=docs/documentation/HostediFrameTokenizer.md) and CardPointe Desktop device with your web application, to easily capture and tokenize card data using the secure iFrame.
- **CardPointe Gateway API** - Use the [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) to authorize payments using the tokens you generate from the CardSecure API or Hosted iFrame Tokenizer. Alternatively, streamline your integration by using the CardPointe Desktop device to capture track data to pass in an authorization request. The response returns a token to your application, as well as the transaction details.

## Requirements

To integrate the CardPointe Desktop solution with your application, you must meet the following requirements:

- You must have a CardPointe merchant account.
- Your merchant account must be boarded to the First Data Rapid Connect processor.
- You must have a CardPointe Desktop (ID TECH Augusta) card reader device, pre-provisioned and provided for use with your merchant account.

## Run in Postman

To help you get started with your integration, we created a sample Postman collection that includes templates of the CardPointe Gateway API and CardSecure API requests that you will use in your integration.

Click the Run in Postman button to download the sample collection:

[Run in Postman](https://app.getpostman.com/run-collection/12525c04e7132dc5077b)

## Printing Receipts

If you use the CardPointe Desktop solution to accept payments, you will need to provide your customers with either print or digital receipts. To do so, you can retrieve certain data from the authorization response to print on the receipt. See [Printing Receipts Using Authorization Data](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md#printing-receipts-using-authorization-data) for detailed information.

## Sample Tokenization Requests

You can use the [CardSecure API](?path=docs/APIs/CardSecureAPI.md) to [tokenize](../api/?type=post&path=/cardsecure/api/v1/ccn/tokenize) track data captured using the CardPointe Desktop device. The following examples illustrate sample tokenization requests using track data captured by the device.

| Method | URL | Request Header | 
| --- | --- | --- |
| POST | https://{site}.cardconnect.com/cardsecure/api/v1/ccn/tokenize | Content-Type: application/json |

<!-- theme: warning -->
> The CardPointe Desktop device uses keyboard emulation to output the track data to your application.
> 
> To capture the track data output, put your cursor in the `devicedata` field in the request body, then insert or swipe the card at the device. The device retrieves and encrypts the track data and inserts it at the cursor.

#### Non-EMV (MSR) Discover Test Card

```json
//Request Body:

{
"devicedata":"02A001801F372300839B%*6011********6668^DISCOVER TEST CARD^***************?*;6011********6668=***************?*10C1A6E1036EBEEAFCEE4C8A0899B2F6360B2A040D22DA853C91FC26AAE99A685B2550B263EE0C9DFECA54E364DD02E64A8286D10A72A95F591AB47592BA0D6D0A30D09A5AD82434AF2676299E5F531A96A314B48C34AA5E1AEA90E59509B1BD0000000000000000000000000000000000000000000000000000000000000000000000000000000038313554353935353138CDCDCD94030006C00106632303"
}

//Response:

200 OK 
{
"message": "No error",
"errorcode": 0,
"token": "9603982807976668"
}
```

#### EMV Discover Test Card

```json
//Request Body:

{
"devicedata":"DFEE25020203DFEE26022000DFEE120ACDCDCD94030006C001044F07A00000015230105008444953434F564552DFEF5D136510CCCCCCCC0133D2312201CCCCCCCCCCCCCC5718952CE912AC462F1D5261A811DE05633C65FBE2DF0BB56F54DFEF5B086510CCCCCCCC01335A100CFCF9EF17F00098F4C64D688AC3C3BE820218008407A00000015230108A025A33950580000080009A031905089B0268009C01005F24032312315F2A0208405F300202015F3401019F02060000000010009F03060000000000009F0607A00000015230109F0702FF009F080200019F090200019F1101019F0D053040E428009F0E0500100000409F0F053068C4F8009F1208446973636F7665729F1A0208409F1E085465726D696E616C9F21032230319F33036028C89F34035E03009F3501219F360200049F3704FDD4FBCC9F3901059F4005F000F0A0019F4104000002069F5301525F201A554154205553412F5465737420436172642031352020202020205F280208405F2D02656E9F2608A1B845A6577BFF189F2701809F10080106A00003800000DFEF4C06002700000000DFEF4D28218939A59B65F7A23A5CD3A39778E6B227E8D432BA99429DFD0FC63C9B5AF66FAB46BB817586ECD8DFEF57244944205445434820417567757374612053205553422D4B422056312E30322E3032312E53"
}

//Response: 

200 OK 
{
"message": "No error",
"errorcode": 0,
"token": "9656535197710133"
}
```

## Sample Authorization Requests

You can use the [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) to authorize a payment either using a token returned by CardSecure or the Hosted iFrame Tokenizer, or by directly sending the track data captured by the device in the track field in the request.

The following examples illustrate sample authorization requests using the tokenized and raw track data from the CardPointe Desktop device.

| Method | URL | Request Header |
| --- | --- | --- |
| POST or PUT | https://{site}.cardconnect.com/cardconnect/rest/auth | Content-Type: application/json <br> Authorization: Basic |

#### Authorization Request using Encrypted Track Data (EMV)

```json
{
"merchid":"123456789012",
"amount":"5.00",
"track":"DFEE25020203DFEE26022000DFEE120ACDCDCD94030006C001044F07A00000015230105008444953434F564552DFEF5D136510CCCCCCCC0133D2312201CCCCCCCCCCCCCC5718952CE912AC462F1D5261A811DE05633C65FBE2DF0BB56F54DFEF5B086510CCCCCCCC01335A100CFCF9EF17F00098F4C64D688AC3C3BE820218008407A00000015230108A025A33950580000080009A031905089B0268009C01005F24032312315F2A0208405F300202015F3401019F02060000000010009F03060000000000009F0607A00000015230109F0702FF009F080200019F090200019F1101019F0D053040E428009F0E0500100000409F0F053068C4F8009F1208446973636F7665729F1A0208409F1E085465726D696E616C9F21032230319F33036028C89F34035E03009F3501219F360200049F3704FDD4FBCC9F3901059F4005F000F0A0019F4104000002069F5301525F201A554154205553412F5465737420436172642031352020202020205F280208405F2D02656E9F2608A1B845A6577BFF189F2701809F10080106A00003800000DFEF4C06002700000000DFEF4D28218939A59B65F7A23A5CD3A39778E6B227E8D432BA99429DFD0FC63C9B5AF66FAB46BB817586ECD8DFEF57244944205445434820417567757374612053205553422D4B422056312E30322E3032312E53"
}
```

#### Authorization Request using a Token

```json
{
"merchid":"123456789012",
"amount":"5.00",
"expiry":"1222",
"account":"9656535197710133"
}
```
