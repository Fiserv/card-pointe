# CardPointe Integrated Terminal API Changelog

The following entries describe changes to the [CardPointe Integrated Terminal API](?path=docs/APIs/CardPointeIntegratedTerminalAPI.md) and documentation.

Visit status.cardconnect.com and click **subscribe to updates** to receive important release and status notifications.

## Date Updated: 8/16/2022 

Our Bolt family of integrated solutions is becoming **CardPointe Integrated Payments**.

Over the coming weeks, your terminals will receive an update with minor changes, including an updated default theme and branding for terminals that are not configured with custom branding.

Terminals will update automatically during the nightly reboot window; therefore, we ask that you leave your terminals powered on and connected to your network overnight. Once your terminal is updated, you may notice the following changes:

- If you are using the default device wallpaper and theme, the Bolt wallpaper and logo will change to **CardPointe Integrated Terminal**.
- The **Bolted** and **Unbolted** statuses will change to **Connected** and **Disconnected**, respectively.

## Date Updated: 10/30/2021 

An update to the terminal service was deployed to the UAT environment on 10/22/2021 and to the Production environment on 10/30/2021.

This release includes the following changes:

### New default clearDisplayDelay time 

The default `clearDisplayDelay` time is now **1.5 seconds**, reduced from 5 seconds. 

The `clearDisplayDelay` parameter in the authCard and authManual requests determines the number of milliseconds to wait, after the request completes, before sending a clearDisplay command to the terminal to clear the authorization status and return to the idle display. If your authCard or authManual requests do not include a custom `clearDisplayDelay` value, the default delay of 1.5 seconds is used.

Additionally, the `clearDisplay` command within the authCard and authManual transaction flow is now sent to the terminal before the `authResponse` object is received; therefore, if using a non-zero `clearDisplayDelay` value (including the default), you may experience a slight delay (equal to the `clearDisplayDelay` value) in receiving the authorization response.

### Improved Handling for Canceled Transactions on Clover Terminals 

For Clover terminals, when the user attempts to cancel a transaction in progress, but the transaction has already been processed on the CardPointe Gateway, the  transaction is now automatically voided, and the API returns an `"Authorization Voided"` error and `authResponse` object for the voided transaction, containing the authorization response details from the CardPointe Gateway.

For example:

```
{
   "errorCode":900,
   "errorMessage":"Authorization Voided",
   "authResponse":
      {
         "token":"9445123546981111",
         "expiry":"1225",
         "name": "John Doe",
         "retref":"100002039310",
         "avsresp":"Z",
         "respproc":"FNOR",
         "amount":"100.00",
         "resptext":"Approval",
         "authcode":"PPS862",
         "respcode":"00",
         "merchid":"496400000840",
         "cvvresp":"M",
         "respstat":"A",
         "orderid":"123ABC"
      }
}
```

Previously, this scenario required a manual void request via the CardPointe Gateway API.  

### Improved Timeout/Disconnect Error Handling 

When the terminal's connection to the terminal service is disconnected or times out after an authorization request has successfully sent and processed, the error message returned via the API now includes the `authResponse` object, containing the authorization response details from the CardPointe Gateway. 

For example:

```
{
   "errorCode":1,
   "errorMessage":"Terminal request timed out",
   "authResponse":
      {
         "token":"9445123546981111",
         "expiry":"1225",
         "name": "John Doe",
         "retref":"100002039310",
         "avsresp":"Z",
         "respproc":"FNOR",
         "amount":"100.00",
         "resptext":"Approval",
         "authcode":"PPS862",
         "respcode":"00",
         "merchid":"496400000840",
         "cvvresp":"M",
         "respstat":"A",
         "orderid":"123ABC"
      }
}
```

Previously, the API would return one of the following errors:

```
{"errorCode":6,"errorMessage":"Terminal <hsn> is not connected"}

{"errorCode":10,"errorMessage":"Unexpected response received from terminal <hsn>"}
```

### Improved Tip Error Handling 

Tip requests with a blank or invalid amount in the `tipPercentPresets` array (for example, a decimal amount) now return a more user-friendly error message. For example:

```
{"errorCode":3,"errorMessage":"Invalid value for param: 'tipPercentPresets'."}

{"errorCode":3,"errorMessage":"Presets list must not contain null or empty entries"}
```

## Date Updated: 2/27/2021 

The following updates were deployed to the UAT environment on 1/8/2021 and to the Production environment on 2/27/2021.

### New terminalDetails Endpoint 

The Terminal API includes a new terminalDetails endpoint, which allows you to identify the model and supported features for each terminal associated with your merchant ID. This can be useful for merchants with multiple terminals, to determine which supports signature capture or receipt printing, or for merchants with multiple locations, who need to determine which terminals are associated with each merchant ID. See the terminalDetails endpoint description for more information.

### Receipt Printing Enhancements for Clover Terminals 

The Terminal API now includes a printReceipt endpoint, which allows you to reprint receipts after the authorization, using the `orderId` associated with the transaction. Previously, receipts could only be printed at the time of the authorization. See the printReceipt endpoint description for more information.

Additionally, the authCard and authManual endpoints now support printing a second receipt during the transaction, using the following new parameters:

_**Note**: This feature is now available in the Terminal API; however it is not yet supported by the Clover terminal. See the [CardPointe Integrated Terminal Developer Guide for Clover Terminals](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuideForCloverTerminals.md) for additional updates._

- `printExtraReceipt` - If true, the terminal prints a second receipt.
- `printDelay` - Specifies the amount of time (in milliseconds) to wait before printing the second receipt.

### authCard and authManual Endpoint Enhancements 

In addition to the `printExtraReceipt` and `printDelay` parameters, the authCard and authManual endpoints have been updated with the following enhancements:

- `orderId` and `receiptData` response fields are now returned by default. 

    Previously, these response fields were only returned if "printReceipt":"true". was specified in the request. For Ingenico terminals, the printReceipt request field is no longer used. For Clover terminals, printReceipt is still used to select whether or not to print a receipt at the end of the transaction.

- `authCode` request parameter added.

    `authCard` and `authManual` requests now support the optional `authCode` parameter, to support voice authorizations. 

- `bin` request parameter added.

    `authCard` and `authManual` requests now support the optional bin parameter to return the `binInfo` object in the response. The `binInfo` data includes additional information about the payment card used in the authorization. See the CardPointe Gateway API's BIN endpoint description for more information.

- `invoiceId` request parameter added.

    `authCard` and `authManual` requests now support the optional `invoiceId` parameter for ClientLine reporting.

- `termId` request parameter added.

    `authCard` and `authManual` requests now support the optional `termId` parameter for ClientLine reporting.

### Updated Postman Collections 

The [Terminal API Postman Collection](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md) and the [Terminal API Postman Collection for Clover Terminals](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuideForCloverTerminals.md) have been updated to include these new features. 

## Date Updated: 6/2/2020 

### PIN Debit Support 

The CardPointe Integrated Terminal solution now supports PIN debit payments for merchants processing on the Rapid Connect platform for Ingenico. 

To update your integration to support PIN debit transactions, you must include the `aid` and `includePIN` parameters in your authCard or readCard requests. 

See **Accepting PIN Debit Cards** in the [CardPointe Integrated Terminal Developer Guides](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md) for detailed information on updating your integration to include PIN debit payment workflows.

<!-- theme: danger -->
> Before you begin to update your application, note the following important restrictions:
>
> - New or existing merchants who want to accept PIN debit payments must be boarded to the First Data Rapid Connect platform. Merchants currently accepting PIN debit payments on the First Data North platform will continue to be supported.
> - PIN debit is currently only supported on Ingenico terminals.
> - Terminals must be provisioned with the necessary encryption keys for handling PIN data.

## Date Updated: 3/28/2020 

The following updates were deployed to the UAT environment on 11/15/2019 and the production environment on 3/28/2020. 

Visit status.cardconnect.com and click **subscribe to updates** to receive important release and status notifications. 

### New authCard and authManual Response Fields 

The authCard and authManual responses now include the following fields:

| Request | Field	| Type | Description
| --- | --- | --- | ---
| authCard/authManual | `receiptData`	| Array | If `"printReceipt":"true"` is included in the request, the response includes the `receiptData` array. This array includes additional transaction information to be printed on a receipt. <br> <br> See the Receipt Data Fields description in the CardPointe Gateway API for a list of the possible fields returned.
| authCard/authManual | `orderid` | String | The order ID included in the authCard or authManual request, or the automatically-generated order ID if no value was included in the request.
| authCard/authManual | `entrymode`	| String	| _**Note**: Currently,_ `entrymode` _is only returned for merchants processing on the First Data North (FNOR) platform._ <br> <br> The point-of-sale (POS) payment entry mode. <br> <br> Possible values are: <br> <br> Keyed <br> Moto <br> ECommerce <br> Recurring <br> Swipe(Non EMV) <br> DigitalWallet <br> EMVContact <br> Contactless <br> Fallback to Swipe <br> Fallback to Keyed
| authCard/authManual | `bintype` | String | _**Note**: Currently,_ `bintype` _is only returned for merchants processing on the First Data North (FNOR) platform._ <br> <br> The type of card used, determined by the BIN. <br> <br> Possible values are: <br> Corp <br> FSA+Prepaid <br> GSA+Purchase <br> Prepaid <br> Prepaid+Corp <br> Prepaid+Purchase <br> Purchase

### Terminal in Merchant Mode Error Response 

For Clover terminals, the following error message is now returned to the calling application when the device is in Merchant Mode:

`"errorMessage": "hsn: <hsn> is currently in merchant mode"`

See the [CardPointe Integrated Terminal Developer Guide for Clover Terminals](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuideForCloverTerminals.md) for more information.

## Date Updated: 7/24/2019 

This release includes the following updates:

### Updated Signature Capture Logic 

The authCard and authManual endpoints no longer prompt for signature when an authorization is declined. 

Previously, if the authCard or authManual request included `"includeSignature" : "true"`, the cardholder would be prompted for their signature even if the authorization was declined.

### Updated Error Messages 

#### "errorCode" : 1 and "errorCode" : 8

The response body returned for errorCode 1 and errorCode 8 now include the authorization response data returned from the CardPointe Gateway, in the event that an authorization attempt was successfully initiated before the command sequence was canceled or timed out. 

For example:

```
HTTP/1.1 500
Content-Type: application/json
 
{
  "errorCode": 1,
  "errorMessage": "Terminal request timed out",
  "authResponse": {
       "token" : "9445123546981111",
       "expiry" : "0224",
       "name" : "John Doe",
       "batchid" : "100",
       "retref" : "173006146691",
       "avsresp" : "Y",
       "respproc" : "RPCT",
       "amount" : "1.00",
       "resptext" : "Approval",
       "authcode" : "909443",
       "respcode" : "00",
       "merchid" : "1234",
       "cvvresp" : "",
       "respstat": "A",
       "orderId": "C032UQ82820315-20180422141315"    
    }  
}
```

This allows you to determine if the authorization was successfully completed before the authCard or authManual command sequence timed out or was canceled.

See [500: Client or Server Error](?path=docs/APIs/CardPointeIntegratedTerminalAPI.md#500-client-or-server-error) for more information.

#### {"errorCode": 400, "errorMessage: "PIN Debit not supported for merchantId <MID>"}

The "PIN Debit not supported for merchantId" message now includes the MID used in the authorization attempt.

## Date Updated: 5/3/2019 

This release includes the following updates:

### Improved Timeout Handling Logic 

The Terminal API has been updated with improved logic for handling timed out transactions. When the terminal service does not receive an authorization response from the CardPointe Gateway, the terminal service uses the CardPointe Gateway API inquireByOrderid endpoint to check on the status of the authorization attempt. If no status is returned, the terminal service sends three voidByOrderID requests to void the transaction. If enough time remains in the terminal service request sequence, the terminal service retries the authorization. Otherwise, the terminal service returns an authorization failed error, and the request must be resent. See Handling Timeouts in the [CardPointe Integrated Terminal Developer Guides](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuides.md) for more information.

### authCard and authManual Improvements 

The authCard and authManual endpoints include the following enhancements:

- **Support for refunds** - You can now specify a negative amount value to initiate a negative authorization (forced credit). Note that the merchant account must be enabled to process forced credit transactions.
- **Ability to create profiles** - You can now use the data in the request to create a payment profile. If you set the createProfile parameter to true, the terminal service initiates a request to the CardPoint Gateway profile endpoint to create a secure stored payment profile. See the CardPointe Gateway API Profile endpoint description for detailed information.
- **Default orderid values** - To support the improved timeout handling logic, the terminal now automatically generates a unique order ID in the format <HSN-timestamp> if the orderId parameter is not included in the request.

## Date Updated: 11/29/2018 

This release includes the following updates:

### New tip Endpoint 

The new tip endpoint allows you to prompt a user for a tip. You can specify up to four preset tip percentage amounts, including a custom amount option that allows the user to enter a custom tip amount.

### Developer Documentation Improvements 

The Terminal API documentation has been updated with numerous improvements, including:

- An updated [Postman Collection](?path=docs/APIs/CardPointeIntegratedTerminalAPI.md#additional-resources) and additional information for getting started.
- More detailed and consistent service endpoint descriptions and API documentation.
- Revised content and improved layout, throughout.

## Date Updated: 8/23/2018 

This release includes the following updates:

### Enhanced Authorization Capabilities 

- The authCard and authManual endpoints now support the optional **orderId** request parameter.
- EMV tags are now supported for authCard and readCard requests. See Authorization Response | EMV Tags in the CardPointe Gateway API Developer Documentation for more information on integrating EMV acceptance.

### Enhanced Terminal Integration 

- The new clearDisplay endpoint sends a request to clear the existing terminal display.
- The readInput and readConfirmation services now support the optional **beep** request parameter.
