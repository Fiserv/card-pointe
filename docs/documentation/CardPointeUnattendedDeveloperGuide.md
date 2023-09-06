<!-- type: tab -->

<!-- type: card
description: This solution is currently in development and the information in this guide is subject to change.
-->

<!-- type: row-end -->

# Overview

The CardPointe Unattended solution extends CardPointe Integrated Payments to unattended kiosk applications.

The CardPointe Unattended solution includes the following components:

- **Unattended card reader device** - The unattended device accepts EMV (chip), NFC (contactless), and MSR (magnetic stripe) payments, and connects via serial/USB to the host system and your client application.
- **Payment Application Engine** - The Payment Application Engine (PAE) provides the interface between your client application and the unattended device. Your application sends commands to the device to initiate the card reader, and securely capture the card data.
- **Web Payment Application** - The web payment application retrieves the encrypted card data from the device and initiates an authorization request to the CardPointe Gateway, via HTTP/Ethernet.
- **App Loader** - The app loader runs on the unattended device to check for and automatically download and install updates to the firmware, PAE, and SDK installed on the device.
- **CardPointe Gateway API** - The CardPointe Gateway API provides methods for managing and reporting on your transactions. Your application uses the CardPointe Gateway API to void, refund, or check the status of a transaction.

> Similarly to the CardPointe Integrated Terminal solution, your application **must** integrate the CardPointe Gateway API to manage transactions and perform other business functions. See the CardPointe Gateway API documentation for detailed information on integrating the CardPointe Gateway API.

## Features

The CardPointe Unattended solution offers a simple integration for retrieving encrypted payment card data from an unattended kiosk, tokenizing that data, and using the token to process a payment or refund on the CardPointe Gateway.

If you are currently integrating the CardPointe Integrated Terminal API for an attended payment solution, the CardPointe Unattended payment process is similar to a simplified authCard process.

For additional transaction management and reporting features, your application **must** use the CardPointe Gateway API to communicate directly with the CardPointe Gateway. See the CardPointe Gateway API for detailed information.

See [Understanding Payment Flows](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#Understanding-Payment-Flows), later in this guide for more information on making payment requests.

## Requirements

The unattended device requires the following connections:

- A Serial/USB connection to your application.
- An Ethernet connection to validate the device's configuration and to send payment requests to the CardPointe Gateway.
- A power connection using the included AC adapter. 

Your client application must:

- Run on a host system within the unattended kiosk.
- Send ASCII command strings to the unattended device over the Serial/USB connection.
- Send RESTful HTTP requests (for example, to void a transaction) to the CardPointe Gateway over a network (Ethernet or WiFi) connection.
- Retain the `processorReference` (the `retref`, or retrieval reference number, provided by the CardPointe Gateway and returned in the DPT response) in order to look up and manage, refund, or void payments.

<!-- theme: warning -->
> It is strongly recommended that you store the `processorReference` in a database table, which your application can use to manage transactions.

# Developer Resources

The following resources are available to help you integrate the CardPointe Unattended solution with your application.

## RS-232 Serial Drivers

Most workstations should support the serial-to-USB connection natively, without any additional drivers. If your workstation does not recognize the connected device, you may need to install a driver.

Visit the following site to download the necessary drivers for your operating system: 

https://www.prolific.com.tw/US/ShowProduct.aspx?pcid=41&showlevel=0017-0037-0041

## Serial Terminal Applications

To test the device connection and to send and receive data, you can use a serial terminal application.

The following serial terminal applications have been tested for use with the unattended device:

- For macOS: https://www.macupdate.com/app/mac/31352/coolterm
- For Windows: https://www.microridge.com/comtestserial.htm

Before you begin, note the name or number of the port that the device is connected to on your workstation. 

Additionally, configure the port configuration settings as follows:

- **Baudrate**: 115200
- **Data Bits**: 8
- **Parity**: None
- **Stop Bits**: 1

## Sample Checksum Calculator

As described in the CardPointe Unattended API, every command and response must include a checksum value. The checksum is derived from the CRC-16 decimal calculation of the ASCII command string up to and including the delimiter preceding the checksum field.

For testing, you can find a simple checksum calculator at crccalc.com.

<!-- theme: warning -->
> If using crccalc.com, ensure that you set the **Input type** to **ASCII**, **Output type** to **Decimal**, and click the **Calc CRC-16** button. The correct checksum value is displayed in the top-left corner of the results table (using the **CRC-16/CCITT-FALSE** algorithm).

Additionally, you can integrate the following sample code with your application to calculate checksums:

#### Sample Checksum Calculator Code

```C++
const int CRCTable[] =
{
0x0000, 0x1021, 0x2042, 0x3063, 0x4084, 0x50A5, 0x60C6, 0x70E7, 0x8108, 0x9129, 0xA14A, 0xB16B, 0xC18C, 0xD1AD, 0xE1CE, 0xF1EF, 0x1231, 0x0210, 0x3273, 0x2252, 0x52B5, 0x4294, 0x72F7, 0x62D6, 0x9339, 0x8318, 0xB37B, 0xA35A, 0xD3BD, 0xC39C, 0xF3FF, 0xE3DE, 0x2462, 0x3443, 0x0420, 0x1401, 0x64E6, 0x74C7, 0x44A4, 0x5485, 0xA56A, 0xB54B, 0x8528, 0x9509, 0xE5EE, 0xF5CF, 0xC5AC, 0xD58D, 0x3653, 0x2672, 0x1611, 0x0630, 0x76D7, 0x66F6, 0x5695, 0x46B4, 0xB75B, 0xA77A, 0x9719, 0x8738, 0xF7DF, 0xE7FE, 0xD79D, 0xC7BC, 0x48C4, 0x58E5, 0x6886, 0x78A7, 0x0840, 0x1861, 0x2802, 0x3823, 0xC9CC, 0xD9ED, 0xE98E, 0xF9AF, 0x8948, 0x9969, 0xA90A, 0xB92B, 0x5AF5, 0x4AD4, 0x7AB7, 0x6A96, 0x1A71, 0x0A50, 0x3A33, 0x2A12, 0xDBFD, 0xCBDC, 0xFBBF, 0xEB9E, 0x9B79, 0x8B58, 0xBB3B, 0xAB1A, 0x6CA6, 0x7C87, 0x4CE4, 0x5CC5, 0x2C22, 0x3C03, 0x0C60, 0x1C41, 0xEDAE, 0xFD8F, 0xCDEC, 0xDDCD, 0xAD2A, 0xBD0B, 0x8D68, 0x9D49, 0x7E97, 0x6EB6, 0x5ED5, 0x4EF4, 0x3E13, 0x2E32, 0x1E51, 0x0E70, 0xFF9F, 0xEFBE, 0xDFDD, 0xCFFC, 0xBF1B, 0xAF3A, 0x9F59, 0x8F78, 0x9188, 0x81A9, 0xB1CA, 0xA1EB, 0xD10C, 0xC12D, 0xF14E, 0xE16F, 0x1080, 0x00A1, 0x30C2, 0x20E3, 0x5004, 0x4025, 0x7046, 0x6067, 0x83B9, 0x9398, 0xA3FB, 0xB3DA, 0xC33D, 0xD31C, 0xE37F, 0xF35E, 0x02B1, 0x1290, 0x22F3, 0x32D2, 0x4235, 0x5214, 0x6277, 0x7256, 0xB5EA, 0xA5CB, 0x95A8, 0x8589, 0xF56E, 0xE54F, 0xD52C, 0xC50D, 0x34E2, 0x24C3, 0x14A0, 0x0481, 0x7466, 0x6447, 0x5424, 0x4405, 0xA7DB, 0xB7FA, 0x8799, 0x97B8, 0xE75F, 0xF77E, 0xC71D, 0xD73C, 0x26D3, 0x36F2, 0x0691, 0x16B0, 0x6657, 0x7676, 0x4615, 0x5634, 0xD94C, 0xC96D, 0xF90E, 0xE92F, 0x99C8, 0x89E9, 0xB98A, 0xA9AB, 0x5844, 0x4865, 0x7806, 0x6827, 0x18C0, 0x08E1, 0x3882, 0x28A3, 0xCB7D, 0xDB5C, 0xEB3F, 0xFB1E, 0x8BF9, 0x9BD8, 0xABBB, 0xBB9A, 0x4A75, 0x5A54, 0x6A37, 0x7A16, 0x0AF1, 0x1AD0, 0x2AB3, 0x3A92, 0xFD2E, 0xED0F, 0xDD6C, 0xCD4D, 0xBDAA, 0xAD8B, 0x9DE8, 0x8DC9, 0x7C26, 0x6C07, 0x5C64, 0x4C45, 0x3CA2, 0x2C83, 0x1CE0, 0x0CC1, 0xEF1F, 0xFF3E, 0xCF5D, 0xDF7C, 0xAF9B, 0xBFBA, 0x8FD9, 0x9FF8, 0x6E17, 0x7E36, 0x4E55, 0x5E74, 0x2E93, 0x3EB2, 0x0ED1, 0x1EF0
};
int CalcCRC(const char buffer[], unsigned int len)
{
    int crc = 0xffff;
    for (int i = 0; i < len; i++)
    {
        crc = CRCTable[(((crc >> 8)) ^ (buffer[i] & 0xff))] ^ (((crc << 8) & 0xffff));
    }
return crc; }
```

# Understanding Payment Flows

To initiate a payment or to tokenize a card, your application sends a Do Payment Transaction (DPT) command to the unattended device. The device then captures and encrypts the customer's payment card data and sends an authorization request to the CardPointe Gateway. 

To refund, void, or inquire on an existing payment, your application must use the CardPointe Gateway API to interact directly with the CardPointe Gateway.

The following topics describe each of these payment flows in detail.

#### Authorize and Capture a Payment

To authorize and capture a payment, your application interacts with the unattended device as follows:

1) Your application sends a DPT sale (`"paymentType":1`) request.

    For example, to capture a payment for $1, your application sends the following command: 

    `*PAE|DPT|CMD|1|{"OrderID":"A1B2C3", "paymentAmount":100, "paymentType":1, "paymentUser":"John"}|03878|*!PAE!*`

    See the [DPT Request Syntax](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#DPT-Request-Syntax) for detailed information on the PAE command format and supported commands.

2) The PAE requests the payment card data from the unattended device.
3) The PAE retrieves the encrypted card data from the device, and forms an authorization (with capture) request to the CardPointe Gateway, including the Merchant ID and API Authorization Key provisioned for the device.
4) The PAE transmits the auth request and returns the response data from the CardPointe Gateway to your application. See the [DPT Response Syntax](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#DPT-Response-Syntax) for detailed information on the response data.

    **Note**: _The response includes the_ `processorReference` _(the CardPointe Gateway_ `retref` _or retrieval reference number) for the transaction. You must store this value in order to look up, refund, or void the transaction._

5) Your application presents a receipt or (or decline response) to the customer using the response data returned to your application.

#### Authorize a Payment with Delayed Capture

To authorize a payment, but delay the capture, your application interacts with the unattended device as follows:

1) Your application sends a DPT Authorization-only (`"paymentType":6`) request. For example: `*PAE|DPT|CMD|1|{"OrderID":"A1B2C3", "paymentAmount":100, "paymentType":6, "paymentUser":"John"}|50955|*!PAE!*`

    See the [DPT Request Syntax](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#DPT-Request-Syntax) for detailed information on the PAE command format and supported commands.

2) The PAE requests the payment card data from the unattended device.
3) The PAE retrieves the encrypted card data from the device, and forms an authorization request to the CardPointe Gateway, including the Merchant ID and API Authorization Key provisioned for the device.
4) The PAE transmits the auth request and returns the response data from the CardPointe Gateway to your application. See the [DPT Response Syntax](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#DPT-Response-Syntax) for detailed information on the response data.
5) Your application can use the `cardToken` and `processorCode` (authcode) from the response to make a delayed capture request using the CardPointe Gateway API.

#### Refund a Payment without Reference (Forced Credit)

<!-- theme: danger -->
> **Note**: _Your merchant account must be enabled to process forced credit transactions._

To refund a payment using the customer's card, instead of using the retrieval reference number (`retref`), your application interacts with the unattended device as follows:

1) Your application sends a DPT refund (`"paymentType":3`) request.

    For example, to refund a $1 payment, your application sends the command `*PAE|DPT|CMD|1|{"orderId":"A1B2C3", "paymentAmount":100, "paymentType":3, "paymentUser”:”John”}|09282|*!PAE!*`

    **Note**: _The_ `PaymentAmount` _value must be positive._

    See the [DPT Request Syntax](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#DPT-Request-Syntax) for detailed information on the PAE request format and supported payment types.

2) The PAE requests the payment card data from the unattended device.
3) The PAE retrieves the encrypted card data from the device, and forms a negative authorization request to the CardPointe Gateway, including the Merchant ID and API Authorization Key provisioned for the device.
4) The PAE transmits the auth request and returns the response data from the CardPointe Gateway to your application. See the [DPT Response Syntax](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#DPT-Response-Syntax) for detailed information on the response data.
5) Your application presents a receipt or (or decline response) to the customer using the response data returned to your application.

#### Refund or Void a Payment with Reference

To refund or void a transaction, you use the `processorReference` value returned in the [DPT response](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#DPT-Response-Syntax). 

Your application makes a CardPointe Gateway API refund or void request, using the `processorReference` value in the `retref` field in the request.

See the refund and void endpoint descriptions for detailed information.

#### Validate and Tokenize a Payment Card

To validate and tokenize a payment card, your application interacts with the unattended device as follows:

1) Your application sends a DPT tokenization (`"paymentType":5`) request. For example: `*PAE|DPT|CMD|1|{"OrderID":"A1B2C3", "paymentAmount":100, "paymentType":5, "paymentUser":"John"}|02519|*!PAE!*`

    **Note**: _The_ `paymentAmount`, `paymentType`, _and_ `paymentUser` _fields are required to preserve the message format, but the values are ignored for a tokenization request._

    See the [DPT Request Syntax](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#DPT-Request-Syntax) for detailed information on the PAE command format and supported commands.

2) The PAE requests the payment card data from the unattended device.
3) The PAE retrieves the encrypted card data from the device, and a tokenization request to CardSecure.
4) The PAE returns the response data , including the token, from CardSecure to your application. See the [DPT Response Syntax](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#DPT-Response-Syntax) for detailed information on the response data.
5) Your application can use the token to make a payment request or create a customer profile using the CardPointe Gateway API.

## Handling Offline Payments

The CardPointe Unattended device requires a persistent network connection in order to send and receive transaction data. If the device loses its network connection, it can temporarily operate in Offline Mode. In Offline Mode, the device forms and securely stores payment requests, then forwards the transactions to the web payment application for processing, once the network connection is successfully re-established. 

<!-- theme: danger -->
> Accepting payments in Offline Mode can help to prevent customer-facing business disruptions; however these payments are **not** authorized and processed until the device is reconnected. Therefore, you may incur the risk of receiving payment declines after you have provided goods or services.

### Configuring Offline Mode

Offline Mode is disabled by default, and must be individually configured for each device. Additionally, the following settings must be configured:

- **Maximum Amount** - The maximum amount threshold for each offline transaction (in pennies). 
- **Maximum Time** - The maximum amount of time for which offline transactions will be permitted (in hours).

If your application sends a DPT request with an amount that exceeds the configured maximum, or after the configured offline timeframe has expired, the DPT response returns an error.

<!-- theme: warning -->
> Additionally, it is strongly recommended that you include a unique `orderId` value in every [DPT](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#DPT-Do-Payment-Transaction) request. You can use the `orderId` to reconcile offline transactions using the CardPointe Gateway API. See [Reconciling Offline Transactions](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#Reconciling-Offline-Transactions) for more information.

> Contact isvsupport@cardconnect.com for assistance configuring these settings.

### Processing Offline Transactions

When your application sends a DPT command while the device is in Offline Mode, the payment is automatically approved, and returns DPT response code `020D` as well as an offline receipt payload. Because offline transactions are not authorized until they are processed, this offline receipt includes fewer details than a normal receipt.

When the device is in Offline Mode, your application can send the GOT (Get Offline Transactions) command to check for any currently stored transactions (identified by timestamps) and the total amount of all stored transactions. See the [GOT](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#GOT-Get-Offline-Transactions) description for more information.

Additionally, the GCS (Get Current Status) command provides information on currently stored transactions, and identifies when the device is actively processing offline transactions. See the [GCS](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#GCS-Get-Current-Status) description for more information.

When the unattended device re-establishes the network connection, the device temporarily remains in Offline Mode while it creates an offline batch and transmits the offline transactions to the web payment application for processing. Once all stored transactions are processed, the device exits Offline Mode and resumes normal operation.

> It can take up to five (5) minutes for the device to begin transmitting offline transactions once the network connection is restored.

### Reconciling Offline Transactions

To reconcile offline transactions, you can use either of the following CardPointe Gateway API requests:

- Use the `inquireByOrderid` request to retrieve a transaction's status by its unique `orderId`, if specified in the DPT command.
- Use the `settlestatByBatchSource` request to retrieve the entire batch of offline transactions for a given day.

#### Using inquireByOrderid to Retrieve A Single Offline Transaction

To reconcile individual offline transactions, you can use the CardPointe Gateway APIs `inquireByOrderid` endpoint to retrieve transaction status and retrieval reference number (retref). See the Inquire By Order ID description in the CardPointe Gateway API for additional information on the inquireByOrderid request and response.

<!-- theme: warning -->
> In order to use the `inquireByOrderid` request, you must specify a unique `orderId` value in each [DPT](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#DPT-Do-Payment-Transaction) payment request. 

#### Using settlestatByBatchSource to Retrieve Offline Batches

To reconcile offline batches, you can use the CardPointe Gateway API's `settlestatByBatchSource` endpoint to retrieve the settlement status and retrieval reference number (`retref`) for all offline transactions in a given day's offline batch.

> You can only use the `settlestatByBatchSource` request to retrieve the batch details after the batch has closed, typically at the end of the business day.

When the network connection is restored, the device makes a request to the CardPointe Gateway to create a new batch with a unique `batchsource` identifier in the format `<hsn>-<yyyymmdd>` (for example, `103T662929-20210224`). This batch remains open for the remainder of the business day, and is used to capture all offline transactions for that day.

The `settlestatByBatchSource` request requires the `merchid` and unique `batchsource`. The response returns the total number of sales and refunds, the total amount, and a `txns` array, with the details for each transaction in the batch. 

The `salesdoc` field in the response is populated by the `orderId` sent in the initial [DPT](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#DPT-Do-Payment-Transaction) request, which can be useful in reconciling 

See [Using the settlestatByBatchSource Endpoint](../../docs/documentation/CardPointeGatewayDeveloperGuides.md#using-the-settlestatbybatchsource-endpoint) in the [CardPointe Gateway Developer Guides](../../docs/documentation/CardPointeGatewayDeveloperGuides.md) for more information.

# CardPointe Unattended API

The CardPointe Unattended API provides a simple ASCII messaging format for communicating with the device over the serial/USB connection.

All commands and responses share a common format, and every field in the message string is required to preserve the expected format and sequence of fields.

```
*PAE|<message key>|<message type>|<response code>|{JSON data object}|<checksum>|*!PAE!*
```

<!-- theme: warning -->
> - Every message must include these seven fields.
> - Each field in the message is pipe-delimited (`|`). Back-to-back delimiters must be used to specify an empty field (`||`).

| Field | Description |
| --- | --- |
| `*PAE` | The header, required for all messages. |
| `<message key>` | A 3-character alpha-numeric string that indicates which message your application is sending or receiving. <br> One of the following values: <br> <br> [CPT](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#CPT-Cancel-Payment-Transaction) - Cancel Payment Transaction <br> [DPT](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#DPT-Do-Payment-Transaction) - Do Payment Transaction <br> [GCS](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#GCS-Get-Current-Status) - Get Current Status <br> [GOT](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#GOT-Get-Offline-Transactions) - Get Offline Transactions <br> [GTI](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#GTI-Get-Terminal-Info) - Get Terminal Info <br> **PAE** -  Internal messages and errors <br> [RBT](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#RBT-Reboot-Terminal) - Reboot Terminal |
| `<message type>` | A 3 or 4-character alphanumeric string that indicates the message type. <br> One of the following values: <br> <br> **CMD**- Indicates that the message is a command, or request. <br> **RESP** - Indicates that the message is a response. |
| `<response code>` | An 8-character hexadecimal string that indicates a successful response or error. See [Response Codes](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#Response-Codes) for a list of all possible response and error codes. <br> <br> This field is ignored for **CMD** messages; however if null, it must be included as an empty field (`||`) to preserve the message format. |
| `{JSON data object}` | Any additional data passed in the message, in a JSON object containing `"key":"value"` pairs. <br> <br> For a DPT command, the data field is used to specify the transaction details (for example the type and amount of the transaction). <br> <br> For a DPT, GCS, GOT, or GTI response, the data field returns the response data in a JSON object. <br> <br> **Note**: _If the message does not include any additional data, this field must be included as an empty field_ (`||`) _to preserve the message format._ |
| `<checksum>` | An alpha-numeric string used to authorize each request. The checksum must be a decimal value calculated using the ASCII command string up to the delimiter preceding the checksum as a base value, and using the CRC-16 algorithm. <br> <br> For example, for a GTI command, use the following section of the command string as the base value: `*PAE|GTI|CMD|||`. The CRC-16 checksum calculated for this value is `14392`, which you include in the command as follows: `*PAE|GTI|CMD|||14392|*!PAE!*`. <br> <br> See the [Sample Checksum Calculator](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#Sample-Checksum-Calculator) to generate the checksum, or develop your own utility. |
| `*!PAE!*` | *!PAE!* is the message terminator, used to indicate the end of the message.

## CPT (Cancel Payment Transaction)

The CPT request cancels an in-flight DPT command, if the customer has not yet inserted their card. If the customer has already inserted their card and the read was successful, then you must void the transaction.

### CPT Request Syntax

```
*PAE|CPT|CMD|1||<checksum>|*!PAE!*
```

> Fields 4 and 5 are not used, but **must** be present to preserve the expected message format.

#### Sample CPT Request

```json
*PAE|CPT|CMD|1||40451|*!PAE!*
```

### CPT Response Syntax

```
*PAE|CPT|RESP|<response code>||<checksum>|*!PAE!*
```

#### Sample CPT Response

```json
*PAE|CPT|RESP|00000000|Payment canceled|46408|*!PAE!*
```

Where `<response code>` is `00000000` for a successful response, or one of the following errors:

| Hex Value | Description | 
| --- | --- |
| 01A4	| Payment cannot be canceled.
| 01A5	| No active payment.
| 01A6	| Request to cancel payment failed.

See [Response Codes](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#Response-Codes) for more information.

## DPT (Do Payment Transaction)

The DPT request retrieves encrypted payment card data from the device, to tokenize and process a payment on the CardPointe Gateway.

The DPT command supports the following transaction types, as specified by the `paymentType` in the command:

- Sale, or authorization and capture.
- Authorization only, without capture.
- Refund without reference (forced credit), or negative authorization and capture.
- Tokenization only, no authorization or capture.

### DPT Request Syntax

```
*PAE|DPT|CMD|1|{payment details}|<checksum>|*!PAE!*
```

where `{payment details}` is a JSON object that includes the following parameters:

> Parameters are not case-sensitive.
>
> Parameters in **bold** are required.

| Parameter | Type | Description |
| --- | --- | --- |
| orderId | string | A unique order ID used to identify the transaction. This value is an alphanumeric string with a maximum of 19-characters. <br> <br> You can use the order ID to get information on a transaction using the CardPointe Gateway API inquireByOrderid endpoint. <br> <br> **Note**: _If you include an order ID it must meet the following requirements:_ <br> <br> - _The order ID must be a unique value. Using duplicate order IDs can lead to the wrong transaction being voided in the event of a timeout._ <br> - _The order ID must not include any portion of a payment account number (PAN), and no portion of the order ID should be mistaken for a PAN. If the order ID passes the Luhn check performed by the CardPointe Gateway, the value will be masked in the database, and attempts to use the order ID in an inquire, void, or refund request will fail._
| **paymentAmount** | numeral | The amount of the transaction, in cents (the decimal is implied). <br> <br> **Note**: `paymentAmount` _must be a positive number._ |
| **paymentType** | numeral | The type of transaction. Must be one of the following values: <br> <br> **1** - Sale <br> Authorizes and captures the amount specified in the request. <br> <br> **3** - Refund (Forced Credit) <br> Authorizes and processes a refund in the amount specified in the request. <br> **Note**: _The merchant account must be configured to process forced credits._ <br> <br> **5** - Tokenization Only <br> Validates the card and returns a CardSecure token, which can be used to process a payment or create a customer profile, using the CardPointe Gateway API. <br> <br> **6** - Authorization Only <br> Authorizes the amount specified in the request and returns a CardSecure token, but does not capture the transaction. You can use the token to capture the authorized amount using the CardPointe Gateway API. |
| **paymentUser** | string | A unique identifier for the user or attendant initiating the transaction. This can be useful to identify transactions processed during different shifts. <br> <br> **Note**: `PaymentUser` _is required; however you can specify a static value in all DPT requests if this data is not applicable to your environment._ |

For example, to send a payment request for $1.00, send the following command:

#### Sample DPT Request

```json
*PAE|DPT|CMD|1|{"orderId":"TEST01", "paymentAmount":100, "paymentType":1, "paymentUser":"John"}|54215|*!PAE!*
```

### DPT Response Syntax

The DPT command returns a series of responses throughout the transaction process.

In general, DPT responses are returned in the following format:

```
*PAE|DPT|RESP|<response code>|{JSON response object}|<checksum>|*!PAE!*
```

Where `{JSON response object}` contains either:

- an LCD code and LCD message used to provide user interaction details during the transaction process. 
- the authorization response details, once the transaction has completed.

#### Understanding DPT Transaction Flow and Responses

DPT response code `02010000` is used to pass along a series of LCD messages, which can be presented to the user on the POS application. These LCD messages are included as a `{JSON response object}` in the data field of the response.

For example, when the application sends the DPT command, the device responds to the application with the following response, to prompt the user to insert the card:

`*PAE|DPT|RESP|02010000|{"LCD_Code":13,"LCD_Msg":"INSERT CARD"}|59993|*!PAE!*`

The following examples illustrate some common LCD messages. See [LCD Codes and Messages](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#LCD-Codes-And-Messages) for a complete list.

#### Sample DPT LCD Message Responses

```json
*PAE|DPT|RESP|02010000|{"LCD_Code":35,"LCD_Msg":"WELCOME"}|6741|*!PAE!*

*PAE|DPT|RESP|02010000|{"LCD_Code":13,"LCD_Msg":"INSERT CARD"}|59993|*!PAE!*

*PAE|DPT|RESP|02010000|{"LCD_Code":39,"LCD_Msg":"REMOVE CARD"}|57627|*!PAE!*

*PAE|DPT|RESP|02010000|{"LCD_Code":26,"LCD_Msg":"PROCESSING..."}|9076|*!PAE!*
```

While response code `02010000` is used to pass along the LCD message for EMV transactions, the DPT  `<response code>` field can also return the following values:

| Hex Value	| Description |
| --- | --- |
| 01F4 | Payment accepted.
| 01F5 | Payment declined.
| 01F6 | Request missing required parameter.
| 01F7 | Invalid request.
| 01F8 | Payment already in progress.
| 01F9 | Times out waiting for card.
| 01FA | Card read error.
| 01FB | Payment canceled by POS.
| 01FC | Payment failed due to internal error.
| 01FD | Card read fallback to contact.
| 01FE | Internal error.
| 01FF | Unknown MSR response received from device.
| 0200 | General EMV failure.
| 0201 | LCD message passthrough, where the data field in the response includes an LCD code and message wrapped in a JSON object, which the POS application can present to the user. <br> <br> The following messages are most commonly returned: <br> <br> {"LCD_Code":13,"LCD_Msg":"INSERT CARD"} <br> {"LCD_Code":17,"LCD_Msg":"PLEASE WAIT..."} <br> {"LCD_Code":26,"LCD_Msg":"PROCESSING..."} <br> {"LCD_Code":35,"LCD_Msg":"WELCOME"} <br> <br> See LCD Codes and Messages, below, for a complete list of all possible messages. |
| 0202 | Unsupported AID.
| 0203 | Bad card read, terminating.
| 0204 | Bad card read, retrying.
| 0205 | Cannot accept offline payment - No offline storage remaining.
| 0206 | Cannot accept offline payment - Offline mode not enabled.
| 0207 | Cannot accept offline payment - The amount exceeds the offline maximum.
| 0208 | Cannot accept offline payment - The allowed offline time period has expired.
| 0209 | Invalid amount format. The amount must not contain decimals.
| 020A | Device is offline; the transaction was accepted in Offline Mode.
| 020B | No encryption key loaded.

Upon successful completion of a transaction, the response includes the authorization response returned from the CardPointe Gateway in a `{JSON response object}` in the data field in the response.

#### Sample DPT Successful Payment Response

```json
*PAE|DPT|RESP|01F40000|{"transactionID":"a32e49b4-eef1-48a1-841e-2575ddb5f91d","externalTransactionID":"Payment Test 1","paymentID":"3dcbc0dc-8674-4201-b2fc-3d499d00f747","cardToken":"9473656277440119","authorization":"080642","amountAuth":"150","processorMsg":"Approve","processorCode":"000","processorReference":"315074250123","receiptData":"{\"dateTime\":\"20201110135523\",\"dba\":\"CE RPCT Diner\",\"address2\":\"Pittsburgh, PA\",\"phone\":\"8778280720\",\"footer\":\"\",\"nameOnCard\":\"\",\"address1\":\"353 Test Lane\",\"orderNote\":\"\",\"header\":\"\",\"items\":\"\"}","expiry":"1222","bininfo":"{\"country\":\"USA\",\"product\":\"V\",\"bin\":\"476173\",\"cardusestring\":\"True credit, No PIN/Signature capability\",\"gsa\":false,\"corporate\":false,\"fsa\":false,\"subtype\":\"Visa Traditional\",\"purchase\":false,\"prepaid\":false,\"issuer\":\"B 2 16Test Visa\",\"binlo\":\"476173\",\"binhi\":\"476173\",\"success\":false,\"errormsg\":null}","emvTags":"{\"TVR\":\"0000000000\",\"ARC\":\"00\",\"PIN\":\"None\",\"Signature\":\"false\",\"Mode\":\"Issuer\",\"Network Label\":\"VISA\",\"TSI\":\"0000\",\"AID\":\"A0000000031010\",\"IAD\":\"06010A03A00000\",\"Entry method\":\"Contactless Chip\",\"Application Label\":\"VISA CREDIT\"}","merchantID":"177203351990"}|23862|*!PAE!*
```

#### Sample DPT Successful Tokenization Response

```json
*PAE|DPT|RESP|01F40000|{"transactionID":"bd63062b-4a95-46dc-9306-2ad2972532de","externalTransactionID":"Test_token","paymentID":"a0244e4e-7cc2-4d19-a564-43d0584d9aa5","cardToken":"9478550245190119","processorMsg":"No error"}|49018|*!PAE!*
```

<!-- theme: warning -->
> See the CardPointe Gateway API for additional information on the authorization response fields returned by the CardPointe Gateway.

> The fields returned in the JSON response object vary depending on the `paymentType` in the request. `paymentType` 5 (tokenization) only returns the `cardToken` and `processorMsg` fields.

| Field	| paymentType | Description |
| --- | --- | --- |
| amountAuth | 1, 3, 6 | The `amount` authorized. |
| authorization	| 1, 3, 6 | The `authcode` returned to the CardPointe Gateway by the card issuer. |
| bininfo | 1, 3, 6	| The card's BIN (bank identification number) information returned by the CardPointe Gateway. See the BIN response description in the CardPointe Gateway API for detailed information. |
| cardToken	| 1, 3, 5, 6 | The CardSecure `token`, returned by the CardPointe Gateway. |
| emvTags | 1, 3, 6	| The `emvTagData` array of receipt and EMV tag data (when applicable) returned from the processor. <br> <br> This data returned should be presented on a receipt if applicable, and recorded with the transaction details for future reference. <br> <br> See [Printing Receipts Using Authorization Data](../../docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md#Printing-Receipts-Using-Authorization-Data) in the [CardPointe Integrated Terminal Developer Guides](../../docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md) for detailed information. |
| expiry | 1, 3, 6 | The card's expiration date in the format `MMYY`. |
| externalTransactionID	| 1, 3, 5, 6 | The `orderID` included in the payment request. |
| merchantID | 1, 3, 6 | The CardPointe merchant ID (MID) used in the request. |
| paymentID	| 1, 3, 5, 6 | A unique, alphanumeric payment identifier generated by the unattended device. |
| processorCode	| 1, 3, 6 | The `respcode` returned to the CardPointe Gateway by the processor. An alphanumeric code that identifies the authorization response. |
| processorMsg | 1, 3, 5, 6	| The `resptext` returned to the CardPointe Gateway by the processor. A text description of the authorization response. |
| processorReference | 1, 3, 6 | The `retref` generated by the CardPointe Gateway, and used to inquire on and manage transactions. |
| receiptData | 1, 3, 6	| The `receiptData` object provides additional merchant and authorization details for use in printing receipts. The receipt data array includes merchant account information, populated using the merchant properties configured for the MID, as well as additional transaction details from the authorization response. <br> <br> See the Receipt Data Fields description in the CardPointe Gateway API for more information. |
| transactionID	| 1, 3, 5, 6 | A unique, alphanumeric transaction identifier generated by the unattended device. |

## GCS (Get Current Status)

The GCS request returns the current status and availability of the device.

### GCS Request Syntax

```
*PAE|GCS|CMD|1||<checksum>|*!PAE!*
```

#### Sample GCS Request

```json
*PAE|GCS|CMD|1||23478|*!PAE!*
```

### Request Syntax

The GCS response returns the device and application status in the following format:

```
*PAE|GCS|RESP|<response code>|{JSON response object}|<checksum>|*!PAE!*
```

`<response code>` is 00000000 for a successful response, or one or more error codes if the command failed. See [Response Codes](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#Response-Codes) for more information.

`{JSON response object}` is a JSON-encoded string that includes the following fields:

#### Sample GCS Response

```json
*PAE|GCS|RESP|00000000|{"Started":1,"Registered":1,"Connected":1,"Encryption":1,"CardInReader":1,"InPayment":0,"OfflineRecovery":0,"~OfflineTxnsAvail":1100,"AppLoaderStatus":"Idle"}|21083|*!PAE!*
```

| Parameter | Type | Description |
| --- | --- | --- |
| AppLoaderStatus | string | The status of the AppLoader update process. One of the following values: <br> <br> **Startup** - Starting up and waiting for PAE to initialize. <br> **Contacting Cloud** - Attempting to contact the update server. <br> **Downloading** - Downloading data from the update server. <br> **Reconnect Delay** - Waiting during the delay between attempts to connect to the update server. <br> **Validating** - Validating the downloaded update against the apps and SDKs already loaded on the device. <br> **Install Pending** - An update to the app or SDK is pending, allowing the POS application time to detect the pending update prior to installation. <br> **Installing** - App or SDK installation is starting.  <br> **Idle** - The update check is completed and AppLoader is in an idle state until the next device reboot/restart (typically the 24-hour self-check).
| CardInReader | numeral | Indicates whether or not a card is currently inserted into the device. <br> <br> One of the following values: <br> <br> **0** - No card inserted <br> **1** - Card inserted
| Connected	| numeral | Indicates whether or not the device is connected to the web payment application. <br> <br> One of the following values: <br> <br> **0** - Disconnected <br> **1** - Connected
| Encryption | numeral | Indicates whether or not the encryption key is installed and enabled on the device <br> <br> One of the following values: <br> <br> **0** - Not enabled <br> **1** - Enabled |
| InPayment	| numeral | Indicates whether or not the payment application is currently processing a payment. <br> <br> One of the following values: <br> <br> **0** - Ready <br> **1** - Processing a payment |
| OfflineRecovery | numeral	| Indicates whether or not the payment application is currently processing one or more offline payments. <br> <br> One of the following values: <br> <br> **0** - Not processing offline payments <br> **1** - Currently processing offline payments |
| ~OfflineTxnsAvail	| numeral | Indicates the approximate number of offline transactions that can be stored on the device's remaining available storage. |
| Registered | numeral | Indicates whether or not the device successfully registered with the web payment application. <br> <br> One of the following values: <br> <br> **0** - Not registered <br> **1** - Registered |
| Started | numeral | Indicates whether or not the payment application successfully started. <br> <br> One of the following values: <br> <br> **0** - Not started <br> **1** - Running |

## GOT (Get Offline Transactions)

The GOT request returns information on transactions that are currently stored while the unattended device is in [Offline Mode](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#Handling-Offline-Payments).

### GOT Request Syntax

```
*PAE|GOT|CMD|1|{optional parameters}|<checksum>|*!PAE!*
```

> Fields 4 and 5 might not contain values, but **must** be present to preserve the expected message format.

#### Sample GOT Request (all offline transactions)

```json
*PAE|GOT|CMD|||01197|*!PAE!*
```

#### Sample GOT Request (filtered)

```json
*PAE|GOT|CMD|1|{"page":2,"paymentType":"emv"}|64046|*!PAE!*
```

Optionally, you can use the following parameters to filter the results:

| Parameter | Type | Description |
| --- | --- | --- |
| page | numeral | Indicates the page number to return from the response payload (for example, `2` to return page 2 of 4). <br> <br> Each page includes a maximum of 400 offline transactions. |
| paymentType | string | Indicates the type of offline payments to return, either `EMV` or `MSR`. <br> <br> **Required** when a page is specified. |

### GOT Response Syntax

The GOT response returns the stored offline transaction details in the following format:

```
*PAE|GOT|RESP|<response code>|{JSON response object}|<checksum>|*!PAE!*
```

#### Sample GOT Response (No Stored Transactions)

```json
*PAE|GOT|RESP|00000000|{"OfflineTxnsStored":0,"Total$":0,"EMVPayments":"","MSRPayments":""}|55587|*!PAE!*
```

#### Sample GOT Response (Pending Stored Transactions)

```json
*PAE|GOT|RESP|00000000|{"OfflineTxnsStored":"2","Total$":"600","EMVPayments":2,"MSRPayments":0}|20242|*!PAE!*
```

#### Sample GOT Response (Filtered Results - "page":2,"paymentType":"emv")

```json
*PAE|GOT|RESP|00000000|{"EMVPayments":"2021-10-22T19:24:28,2021-10-22T19:25:05,2021-10-22T19:27:22,2021-10-22T19:27:45,2021-10-22T19:28:27"}|54967|*!PAE!*
```

The `<response code>` is `00000000` for a successful response, or one or more error codes if the command failed. See [Response Codes](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#Response-Codes) for more information.

The `{JSON response object}` includes the following fields:

| Field | Description | 
| --- | --- |
| OfflineTxnsStored | The number of stored offline transactions. <br> <br> Only returned for _unfiltered_ GOT requests.
| Total$ | The total amount (in pennies) of currently stored transactions. <br> <br> Only returned for _unfiltered_ GOT requests.
| EMVPayments | For _filtered_ requests (`"paymentType":"emv"`),a comma-separated list of timestamps for all stored EMV transactions, in the format `<YY>-<MM>-<DD>T<HH>:<MM>:<SS>`. <br> <br> for _unfiltered_ requests, the number of stored EMV transactions. |
| MSRPayments | For _filtered_ requests (`"paymentType":"msr"`), a comma-separated list of timestamps for all stored MSR transactions, in the format `<YY>-<MM>-<DD>T<HH>:<MM>:<SS>`. <br> <br> For _unfiltered_ requests, the number of stored MSR transactions. |

## GTI (Get Terminal Info)

The GTI request returns detailed information about the unattended device configuration, which can be useful for identifying or troubleshooting the device.

### GTI Request Syntax

```
*PAE|GTI|CMD|1||<checksum>|*!PAE!*
```

> Fields 4 and 5 are not used, but **must** be present to preserve the expected message format.

#### Sample GTI Request

```json
*PAE|GTI|CMD|1||28604|*!PAE!*
```

### GTI Response Syntax

The GTI response returns the device and application details in the following format:

```
*PAE|GTI|RESP|<response code>|{JSON response object}|<checksum>|*!PAE!*
```

#### GTI Response Example

```json
*PAE|GTI|RESP|00000000|{"PAE_Version":"1.5.3.5","AppLoader_Version":"0.2.0.3","FW_Version":"VP5300 FW v1.02.090.0444.S","HW_Version":"HW, VP5300
K81F Rev4","SerialNum":"103T662933","CompanyId":"00a2aa6d-5c4a-4ae1-ae48-f95e335550e1","ApplicationID":"ab02d9c6-64f1-48f6-8d50-245ee5aa71e8"}|59371|*!PAE!*
```

The `<response code>` is `00000000` for a successful response, or one or more error codes if the command failed. See [Response Codes](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#Response-Codes) for more information.

The `{JSON response object}` includes the following fields:

| Field	| Description
| --- | ---
| PAE_Version | The Payment Application Engine (PAE) version running on the device.
| AppLoader_Version	| The AppLoader version running on the device.
| FW_Version | The current firmware version running on the device.
| HW_Version | The current hardware revision of the device.
| SerialNum	| The hardware serial number (HSN) for the device, used to identify the device to the CardPointe Gateway.
| CompanyId	| A GUID, used by the web payments application, to identify the merchant associated with the device.
| ApplicationID	| A GUID, used by the web payments application, to identify the application running on the device.

## RBT (Reboot Terminal)

The RBT request remotely reboots, or power cycles, the device.

### RBT Request Syntax 

```
*PAE|RBT|CMD|1||<checksum>|*!PAE!*
```

> Fields 4 and 5 are not used, but **must** be present to preserve the expected message format.

#### Sample RBT Request

```json
*PAE|RBT|CMD|1||22891|*!PAE!*
```

### RBT Response Syntax

The RBT response returns a response code to indicate if the command was successful or if an error occurred in the following format:

```
*PAE|RBT|RESP|<response code>||<checksum>|*!PAE!*
```

The `<response code>` is `00000000` for a successful response, or one or more error codes if the command failed. See [Response Codes](../../docs/documentation/CardPointeUnattendedDeveloperGuide.md#Response-Codes) for more information.

#### Sample RBT Response

```json
*PAE|RBT|RESP|00000000||62093|*!PAE!*
```

# Response Codes

The payment application includes a series of response codes that provide status and error information within the response message. 

Response codes are generated as 8-character hexadecimal strings, in the format: `<general code>` `<detail code>`, where 

- `<general code>` is a 4-character value representing the general response information.
- `<detail code>` is a 4-character value representing a more detailed explanation. The `<detail code>` value can be null, represented as `0000`, if no detailed response is available.

For example, the following response code indicates that the DPT command was successful, and the device is waiting for the customer to insert a card:

```
*PAE|DPT|RESP|01F40000|{"cardToken":"9475419822370119","processorMsg":"No error"}|08414|*!PAE!*
```

In this example, `01F4` represents the general DPT successful status response and `0000` represents no additional detailed information.

The following table describes each possible response code, and the corresponding decimal value, returned in the previous version of the application:

<!-- theme: danger -->
> The following response codes are still in development and are subject to change. Undocumented values are reserved for future use.

| Decimal Value | Hex Value | Description | 
| --- | --- | ---
| **0-199 General Response Codes** |  |  |
| 0	| 0000 | Command succeeded.
| 1	| 0001 | Command failed.
| **200-209 PAE Message Errors** |  |  | 
| 200 | 00C8 | Command could not be sent because PAE is starting.
| 201 | 00C9 | Message received is not valid.
| 202 | 00CA | Message missing required fields.
| 203 | 00CB | Invalid checksum value.
| 204 | 00CC | Invalid command.
| 205 | 00CD | Message received is empty.
| **210-219 File System Errors** |  |  |
| 210 | 00D2 | Can't create file on device.
| 211 | 00D3 | Can't open file on device.
| 212 | 00D4 | Can't read file on device.
| 213 | 00D5 | Can't write file to device.
| 214 | 00D6 | Can't remove file from device.
| **220-229 Device Configuration Errors** |  |  | 
| 220 | 00DC | Configuration file can't be read.
| 221 | 00DD | Configuration file can't be parsed.
| 222 | 00DE | Configuration file can't be saved
| 223 | 00DF | Required configuration parameter missing.
| **230-249 Payment System Errors** |  |  | 
| 230 | 00E6 | Failed to start the payment process.
| **250-299 HTTP Communication Errors** |  |  |
| 250 | 00FA | Failed to register device with ITSCO server.
| 251 | 00FB | Failed to register PAE with ITSCO server.
| 252 | 00FC | Internal server error.
| 253 | 00FD | Payment cannot be finalized on server.
| 254 | 00FE | Failed to send HTTP request.
| 255 | 00FF | Timed out waiting for HTTP response.
| 256 | 0100 | HTTP response 400+ (client or server error).
| 257 | 0101 | HTTP response missing a required value.
| 258 | 0102 | Device SSL certificate error.
| 259 | 0103 | No network connection.
| **260-275 Firmware HTTP Errors** <br> <br> **Note**: _Descriptions of the following errors are intentionally omitted. Contact Support if you encounter one of the following errors._ |  |  |
| 260 | 0104 |
| 261 | 0105 |	
| 262 | 0106 |	
| 263 | 0107 |	
| 264 | 0108 |	
| 265 | 0109 |	
| 266 | 010A |
| 267 | 010B |	
| 268 | 010C |	
| 269 | 010D |	
| 270 | 010E |	
| 271 | 010F |	
| 272 | 0110 |	
| 273 | 0111 |	
| 274 | 0112 |	
| 275 | 0113 |	
| **300-399 General Errors** |  |  |
| 300 | 012C | Invalid JSON.
| 301 | 012D | Device cannot allocate memory.
| 302 | 012E | Firmware call failed.
| 303 | 012F | Unknown error.
| 304 | 0130 | PAE received a message with invalid JSON.
| 305 | 0131 | PAE could not generate mutex.
| 306 | 0132 | Encryption is not enabled.
| 307 | 0133 | Data encryption key missing.
| **400-419 CFU Response Codes** |  |  |
| **420-439 CPT Response Codes** |  |  |
| 420 | 01A4 | Payment cannot be canceled.
| 421 | 01A5 | No active payment.
| 422 | 01A6 | Request to cancel payment failed.
| **440-459 GCS Response Codes** |  |  |
| **460-479 GTI Response Codes** |  |  |
| 460 | 01CC | Not all data fields could be returned.
| **480-499 GOT Response Codes** |  |  |
| 480 | 01E0 | paymentType required when a page is specified.
| **500- 599 DPT Response Codes** |  |  |
| 500 | 01F4 | Payment accepted.
| 501 | 01F5 | Payment declined.
| 502 | 01F6 | Request missing required parameter.
| 503 | 01F7 | Invalid request.
| 504 | 01F8 | Payment already in progress.
| 505 | 01F9 | Times out waiting for card.
| 506 | 01FA | Card read error.
| 507 | 01FB | Payment canceled by POS.
| 508 | 01FC | Payment failed due to internal error.
| 509 | 01FD | Card read fallback to contact.
| 510 | 01FE | Internal error.
| 511 | 01FF | Unknown MSR response received from device.
| 512 | 0200 | General EMV failure.
| 513 | 0201 | LCD message passthrough, where the data field in the response includes an LCD code and message wrapped in a JSON object, which the POS application can present to the user. <br> <br> The following messages are most commonly returned: <br> <br> {"LCD_Code":13,"LCD_Msg":"INSERT CARD"} <br> {"LCD_Code":17,"LCD_Msg":"PLEASE WAIT..."} <br> {"LCD_Code":26,"LCD_Msg":"PROCESSING..."} <br> {"LCD_Code":35,"LCD_Msg":"WELCOME"} <br> <br> See LCD Codes and Messages, below, for a complete list of all possible messages. |
| 514 | 0202 | Unsupported AID.
| 515 | 0203 | Bad card read, terminating.
| 516 | 0204 | Bad card read, retrying.
| 517 | 0205 | Cannot accept offline payment - No offline storage remaining.
| 518 | 0206 | Cannot accept offline payment - Offline mode not enabled.
| 519 | 0207 | Cannot accept offline payment - The amount exceeds the offline maximum.
| 520 | 0208 | Cannot accept offline payment - The allowed offline time period has expired.
| 521 | 0209 | Invalid amount format. The amount must not contain decimals.
| 522 | 020A | Device is offline; the transaction was accepted in Offline Mode.
| 523 | 020B | No encryption key loaded.
| **600 - 610 RBT Response Codes** |  |  |

## LCD Codes and Messages

See the following topic below to review the complete list of LCD codes and messages.

```
{"LCD_Code":0,"LCD_Msg":""}
{"LCD_Code":1,"LCD_Msg":"AMOUNT:"}
{"LCD_Code":2,"LCD_Msg":"AMOUNT OK?"}
{"LCD_Code":3,"LCD_Msg":"APPROVED"}
{"LCD_Code":4,"LCD_Msg":"CALL YOUR BANK"}
{"LCD_Code":5,"LCD_Msg":"CANCEL OR ENTER"}
{"LCD_Code":6,"LCD_Msg":"CARD ERROR"}
{"LCD_Code":7,"LCD_Msg":"DECLINED"}
{"LCD_Code":8,"LCD_Msg":"ENTER AMOUNT"}
{"LCD_Code":9,"LCD_Msg":"PLEASE ENTER PIN"}
{"LCD_Code":10,"LCD_Msg":"INCORRECT PIN"}
{"LCD_Code":11,"LCD_Msg":"INSERT/SWIPE CARD"}
{"LCD_Code":12,"LCD_Msg":"CARD"}
{"LCD_Code":13,"LCD_Msg":"INSERT CARD"}
{"LCD_Code":14,"LCD_Msg":"USE CHIP  READER"}
{"LCD_Code":15,"LCD_Msg":"NOT ACCEPTED"}
{"LCD_Code":16,"LCD_Msg":"GET PIN OK"}
{"LCD_Code":17,"LCD_Msg":"PLEASE WAIT..."}
{"LCD_Code":18,"LCD_Msg":"PROCESSING ERROR"}
{"LCD_Code":19,"LCD_Msg":"USE MAGSTRIPE"}
{"LCD_Code":20,"LCD_Msg":"TRY AGAIN"}
{"LCD_Code":21,"LCD_Msg":"AUTHORIZING..."}
{"LCD_Code":22,"LCD_Msg":"TRANSACTION ERROR"}
{"LCD_Code":23,"LCD_Msg":"TERMINATED"}
{"LCD_Code":24,"LCD_Msg":"ADVICE"}
{"LCD_Code":25,"LCD_Msg":"TIMEOUT"}
{"LCD_Code":26,"LCD_Msg":"PROCESSING..."}
{"LCD_Code":27,"LCD_Msg":"PIN TRY LIMIT EX"}
{"LCD_Code":28,"LCD_Msg":"ISSUER AUTH FAIL"}
{"LCD_Code":29,"LCD_Msg":"CONTINUE PROCESS"}
{"LCD_Code":30,"LCD_Msg":"GET PIN ERROR"}
{"LCD_Code":31,"LCD_Msg":"GET PIN FAIL"}
{"LCD_Code":32,"LCD_Msg":"NO KEY GET PIN"}
{"LCD_Code":33,"LCD_Msg":"CANCELED"}
{"LCD_Code":34,"LCD_Msg":"LAST PIN TRY"}
{"LCD_Code":35,"LCD_Msg":"WELCOME"}
{"LCD_Code":36,"LCD_Msg":"AMOUNT OTHER"}
{"LCD_Code":37,"LCD_Msg":"ENTER AMOUNT OTHER"}
{"LCD_Code":38,"LCD_Msg":"CAPK HASH VALUE FAIL"}
{"LCD_Code":39,"LCD_Msg":"REMOVE CARD"}
------Codes 40-63 Reserved for Future Use------
{"LCD_Code":64,"LCD_Msg":"TRY MSR AGAIN"}
{"LCD_Code":65,"LCD_Msg":"LAST MSR TRY"}
{"LCD_Code":66,"LCD_Msg":"TRY ICC AGAIN"}
{"LCD_Code":67,"LCD_Msg":"PLEASE REMOVE CARD"}
{"LCD_Code":68,"LCD_Msg":"THANK YOU"}
{"LCD_Code":69,"LCD_Msg":"NOT AUTHORIZED"}
{"LCD_Code":70,"LCD_Msg":"TRANSACTION COMPLETED"}
{"LCD_Code":71,"LCD_Msg":"FAIL"}
{"LCD_Code":72,"LCD_Msg":"ERROR"}
{"LCD_Code":73,"LCD_Msg":"STOP"}
{"LCD_Code":74,"LCD_Msg":"SEE MOBILE PHONE"}
{"LCD_Code":75,"LCD_Msg":"PLEASE SIGN RECEIPT"}
{"LCD_Code":76,"LCD_Msg":"SIGNATURE REQUIRED"}
{"LCD_Code":77,"LCD_Msg":"NOT CONNECTED"}
{"LCD_Code":78,"LCD_Msg":"TOO MANY TAPS"}
{"LCD_Code":79,"LCD_Msg":"FORM ERROR"}
{"LCD_Code":80,"LCD_Msg":"OFFLINE AMOUNT"}
{"LCD_Code":81,"LCD_Msg":"BALANCE:"}
{"LCD_Code":82,"LCD_Msg":"REFUND"}
{"LCD_Code":83,"LCD_Msg":"SWIPE CARD"}
{"LCD_Code":84,"LCD_Msg":"PRESENT CARD"}
{"LCD_Code":85,"LCD_Msg":"PLEASE TAP OR SWIPE CARD"}
{"LCD_Code":86,"LCD_Msg":"INSERT/PRESENT CARD"}
{"LCD_Code":87,"LCD_Msg":"INSERT/PRESENT/SWIPE CARD"}
{"LCD_Code":88,"LCD_Msg":"USE CHIP & PIN"}
{"LCD_Code":89,"LCD_Msg":"TRY OTHER"}
{"LCD_Code":90,"LCD_Msg":"USE OTHER CARD"}
{"LCD_Code":91,"LCD_Msg":"PLEASE USE OTHER VISA CARD"}
{"LCD_Code":92,"LCD_Msg":"USE ALTERNATIVE PAYMENT METHOD"}
{"LCD_Code":93,"LCD_Msg":"NO CARD"}
{"LCD_Code":94,"LCD_Msg":"PRESENT ONE CARD ONLY"}
{"LCD_Code":95,"LCD_Msg":"INTERNATIONAL CARD ONLY"}
{"LCD_Code":96,"LCD_Msg":"SWIPE CARD AGAIN"}
{"LCD_Code":97,"LCD_Msg":"LAST SWIPE CARD"}
{"LCD_Code":98,"LCD_Msg":"INSERT CARD AGAIN"}
{"LCD_Code":99,"LCD_Msg":"CARD BLOCKED"}
{"LCD_Code":100,"LCD_Msg":"CORRECT PIN"}
{"LCD_Code":101,"LCD_Msg":"ENTER PIN AGAIN"}
{"LCD_Code":102,"LCD_Msg":"UNABLE TO ENTER PIN"}
{"LCD_Code":103,"LCD_Msg":"SELECT NEXT CANDIDATE"}
{"LCD_Code":104,"LCD_Msg":"CARD READ OK REMOVE CARD"}
{"LCD_Code":105,"LCD_Msg":"AVAILABLE:"}
{"LCD_Code":106,"LCD_Msg":"PLEASE WAIT"}
{"LCD_Code":107,"LCD_Msg":"INITIALIZING PLEASE WAIT..."}
{"LCD_Code":108,"LCD_Msg":"APPROVED, BAL:"}
{"LCD_Code":109,"LCD_Msg":"DECLINED, BAL:"}
{"LCD_Code":110,"LCD_Msg":"PIN REQUIRED"}
{"LCD_Code":111,"LCD_Msg":"PAYMENT TYPE NOT ACCEPTED"}
{"LCD_Code":112,"LCD_Msg":"ONLINE APPROVED"}
{"LCD_Code":113,"LCD_Msg":"ONLINE DECLINED"}
{"LCD_Code":114,"LCD_Msg":"ONLINE FAIL OFFLINE DECLINED"}
{"LCD_Code":115,"LCD_Msg":"BLACKLIST"}
{"LCD_Code":116,"LCD_Msg":"CARD EXPIRED"}
{"LCD_Code":117,"LCD_Msg":"ONLINE APPROVED BAL:"}
{"LCD_Code":118,"LCD_Msg":"ONLINE DECLINED BAL:"}
{"LCD_Code":119,"LCD_Msg":"ODA PERFORMED SDA SUCCESS"}
{"LCD_Code":120,"LCD_Msg":"ODA PERFORMED FDDA SUCCESS"}
{"LCD_Code":121,"LCD_Msg":"ODA PERFORMED SDA FAIL"}
{"LCD_Code":122,"LCD_Msg":"ODA PERFORMED FDDA FAIL"}
{"LCD_Code":123,"LCD_Msg":"ODA FAIL"}
{"LCD_Code":124,"LCD_Msg":"SEE PINPAD"}
{"LCD_Code":125,"LCD_Msg":"OFFLINE APPROVED"}
{"LCD_Code":126,"LCD_Msg":"OFFLINE DECLINED"}
{"LCD_Code":127,"LCD_Msg":"OFFLINE APPROVED BAL:"}
{"LCD_Code":128,"LCD_Msg":"OFFLINE DECLINED BAL:"}
{"LCD_Code":129,"LCD_Msg":""}
```

## Hex to Decimal Response Code Converter

You can use the following C/C++ code snippet to convert the hexadecimal response codes into decimal values, if needed to retain backwards compatibility with your application.

#### Sample Response Code Converter

```C++
void DecodeResponse(char* responseCode, unsigned short* detailCode, unsigned short* generalCode)
{
    unsigned long tmp = (unsigned long)strtol(responseCode, NULL, 16);
    *generalCode = tmp >> 16;
    *detailCode = (unsigned short)(0x0000FFFF & tmp);
}
```
