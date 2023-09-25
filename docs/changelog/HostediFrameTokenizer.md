# Hosted iFrame Tokenizer Changelog

The following entries describe changes to the [Hosted iFrame Tokenizer](?path=docs/documentation/HostediFrameTokenizer.md) and documentation.

Visit status.cardconnect.com and click **subscribe to updates** to receive important release and status notifications.

## Date Updated: 8/20/2021 

The changes below were deployed to the UAT environment on 6/25/2021 and later deployed to the production environment on 7/24/2021.

### New Optional Parameters 

A new `sendcardtypingevent` parameter is available in this release. If **true**, events are sent to the parent page when input to the Card Number field is detected and when the input has completed or timed out.

Additionally, a new `selectinputdelay` parameter is available and intended for mobile implementations where user input unexpectedly causes already populated fields to be cleared.

| Parameter | Default Value | Description |
| --- | --- | --- |
| sendcardtypingevent	| false |	If **true**, an event is sent to the parent page with `'cardTyping':true` when the user begins entering a value into the Card Number field. When an `onBlur` event occurs or the `inactivityto` value has been met, the card number value is submitted for tokenization and an event is sent to the parent page with `'cardTyping':false`. <br> <br> An example of this event can viewed in the web browser's console on the following example page: <br> https://fts-uat.cardconnect.com/itoke/outer-page.html?sendcardtypingevent=true
| selectinputdelay | 0 | _**Note**: This parameter is intended for iOS implementations where user input unexpectedly causes already populated fields to be cleared._ <br> <br> Controls how long (in milliseconds) to ignore input to a newly selected field after changing focus. The default value of 0 represents no delay in accepting input to a newly selected field, whereas the maximum value of 1000 represents a 1 second delay before accepting input to a newly selected field. A value of 100 is typically sufficient to avoid unintended clearing without interfering with the user experience. |

### Accessibility Enhancement 

The iFrame Tokenizer now includes the `for` HTML attribute for all `label` HTML elements, containing a value that matches the name of the respective input field. This enhancement allows clicking on the label to focus the cursor on the label's respective input field.

Additionally, all `input` and `select` HTML elements now contain the `aria-label` attribute, containing a value that matches the value of the element's `title` for enhanced accessibility.

<!-- theme: warning -->
> When no optional parameters are included to specify a custom title for input or select elements, the element's default title is used for the `aria-label` attribute. See the Input Labels section for information on the default values used.

You can view an example of the new `for` and `aria-label` attributes for the Card Number label and input field in a snippet of the iFrame source code below:

```
<label for="ccnumfield" id="cccardlabel">Card Number</label>
<input id="ccnumfield" type="tel" name="ccnumfield" size="19" maxlength="2000" autocomplete="off" title="Credit Card Number" aria-label="Credit Card Number">
```

### Support for RSA Encryption 

The Hosted iFrame Tokenizer now supports RSA encryption to encrypt account data before sending the tokenize request to CardSecure.

If you already have had a CardSecure RSA key pair generated for you, the iFrame Tokenizer now automatically obtains the public key and encrypts the data during the tokenization request to CardSecure. You can use the browser's developer tools to verify that the data is encrypted using your RSA key when `"encryptionhandler" : "RSA"` is included in the tokenize request.

If you do not have a CardSecure RSA key pair already generated, the iFrame Tokenizer is unable to obtain a public key for encryption and you will now encounter a 'No public key found' error when viewing the request within the browser's developer tools. You may disregard this error, as the iFrame Tokenizer continues to transmit the account data over a secure connection to CardSecure.

If you would like to utilize RSA encryption with the Hosted iFrame Tokenizer, contact CardPointe Support.

## Date Updated: 10/31/2020 

The changes below were deployed to the UAT environment on 10/15/2020 and to the production environment on 10/31/2020. 

This release includes the changes described in the [3/6/2020 Update](#date-updated-3-6-2020), which were previously deployed to the UAT environment only. The changes that are new to this release are outlined in the section below.

### New Optional Parameters for Field Validation 

The following optional URL parameters have been added to validate and return error messages for input to the Card Number, CVV, and Expiration Date fields:

| Parameter	| Default | Value	| Description |
| --- | --- | --- | --- |
| invalidcreditcardevent | false | If **true**, a `'message'` event is sent to the parent page when the iFrame determines that the value for the Card Number field is invalid or empty. <br> <br> The `'data'` property in the event will contain an empty string, and the `'validationError'` property will contain a description of the validation failure. <br> <br> _**Note**: This parameter should be used in place of_ `invalidinputevent` _when enabling the_ `usecvv` _or_ `useexpiry` _parameters._ |
| invalidcvvevent	| false | If **true**, a `'message'` event is sent to the parent page when the iFrame determines that the value for the CVV field is invalid or empty. <br> <br> The `'data'` property in the event will contain an empty string, and the `'validationError'` property will contain a description of the validation failure. <br> <br> _**Note**: Requires_ `usecvv=true`. |
| invalidexpiryevent | false | If **true**, a `'message'` event is sent to the parent page when the iFrame determines that the value for the Expiration Date field is invalid or empty. <br> <br> The `'data'` property in the event will contain an empty string, and the `'validationError'` property will contain a description of the validation failure. <br> <br> _**Note**: Requires_ `useexpiry=true`. |

## Date Updated: 3/6/2020 

The following updates have been deployed **only to the UAT environment** on 3/6/2020, and will be deployed to the production environment at a future date.

### URL Parameter Validation 

This update includes additional validation to ensure that integrated iFrames are coded following standard best practices. Specifically, it is important that **all** special characters (including curly braces `{}` )provided within the iFrame URL parameters are properly URL-encoded, as described in [iFrame Styling](?path=docs/documentation.HostediFrameTokenizer.md#iframe-styling).

<!-- theme: danger -->
> Failure to properly encode the URL string will result in iFrame display and rendering issues.

### Additional CSS Support 

The `css` parameter now supports custom colors for placeholder text, as well accepting CSS3 media queries for responsive design.

> All arguments to the css parameter must be URL-encoded.

### New Optional Parameters 

The following optional parameters are new in this release:

| Parameter |	Default Value	| Description |
| --- | --- | --- |
| cardinputmaxlength | 2000	| Controls the maximum character limit for the card number field. <br> <br> _**Note**: Do not set cardinputmaxlength if using a USB card reader._
| monthnames | January-February-March-April-May-June-July-August-September-October-December | A string value to be used as alternative labels for each month in the drop-down list. <br> <br> The alternative label for each month is separated by a hyphen, starting with the 1st month and ending with the 12th month. <br> <br> _**Note**: The alternative labels are used only when_ `usemonthnames` _is true._ |
| cardnumbernumericonly	| false | If **true**, the card number field ignores non-numeric values.
| maskfirsttwo | false | If **true**, the first 2 digits of the card number are masked.

### Support for iOS AutoFill Feature 
\Selecting the Card Number input field now triggers the native AutoFill feature on iOS devices when using Safari. This allows users to enter card data by selecting a card stored in the Wallet, or by using the device's camera scan a physical card.

_**Note**: The ability to store or save card details in the browser remains disabled._

### Additional Character Support for Placeholder Values 

The maximum character limit for the placeholder parameter has been increased to **60** characters.

Support for the following accented and special characters has been added: **Á, É, Í, Ñ, Ó, Ú, Ü, à, á, â, ä, ã, ā, ç ,è, é, ê, ē, í, î, ī, ñ, ó, ô, ö, ō, õ, ú, û, ü, ū, ß, ё, й, ъ, ы, э, щ.**

## Date Updated: 8/13/2019 

This release includes the following updates:

### Support for CVV and Expiry Fields 

The iFrame Tokenizer now supports fields for capturing the CVV and Expiry values and storing these values in the token. Additionally, these fields can contain placeholder values.

See [Examples](?path=docs/documentation/HostediFrameTokenizer.md) for sample implementations including the CVV and Expiry fields.

The following URL parameters have been added to support this feature:

| Parameter	| Type | Description
| --- | --- | ---
| **useexpiry** | Boolean	| If **true**, includes two drop-down selectors to specify the expiration month (MM) and Year (YYYY).
| **useexpiryfield** | Boolean | If **true**, uses displays entry fields instead of drop-down selectors. <br> <br> The Month field supports 2-digit month values from 1-12 (including support for leading zeros for single-digit months, for example "01"). <br> <br> The Year field supports a 4-digit year value within the next 20 years (including the current year). |
| **usemonthnames**	| Boolean	| If **true**, displays Month names instead of numbers.
| **usecvv** | Boolean | If **true**, includes a field to enter the Cardholder Verification Value (CVV). <br> <br> **Notes:** <br> <br> _If_ `usecvv` _is **true**, the CVV must be provided to initiate the tokenization request._ <br> _CVV is not masked. This value remains in clear text._ |
| **orientation**	| AN | Controls the orientation of the elements within the iFrame. <br> <br> Supported values are: <br> <br> **default** - the default orientation, used if no value is passed for this parameter. <br> **horizontal** <br> **vertical** <br> **custom** 
| **cardlabel**	| N	| Controls the text inside the label for the card number field. <br> <br> This label is only present when either `useexpiry` or `usecvv` are true.
| **expirylabel**	| N	| Controls the text inside the label for the expiration date selectors. <br> <br> The label is only present when `useexpiry` is true.
| **cvvlabel** | N | Controls the text inside the label for the CVV field. <br> <br> The label is only present when `usecvv` is true.
| **placeholdercvv** | N | A placeholder value to display in the CVV field. <br> <br> The placeholder value must be a 3 or 4-digit numeric value.
| **placeholdermonth** | N | A placeholder value to display in the Month field, in the format MM.
| **placeholderyear**	| N	| A placeholder value to display in the Year field, in the format YYYY.

### Support for Screen Readers 

Title tags have been added to the Card Number, Expiry, and CVV fields to enable compatibility with screen reader applications. These fields are now tagged with titles, which can be understood and read by screen reader applications for users with vision impairments.

The default title values are:

- **Credit Card Number** - "Credit Card Number"
- **Expiry Month** - "Expiration Month"
- **Expiry Year** - "Expiration Year"
- **CVV** - "Card Verification Value"

You can use the following URL parameters to customize the default titles:

| Parameter        | Type | Description                                           |
|------------------|------|-------------------------------------------------------|
| **cardtitle**        | AN   | A meaningful custom title for the card number field.  |
| **expirymonthtitle** | AN   | A meaningful custom title for the expiry month field. |
| **expiryyeartitle**  | AN   | A meaningful custom title for the expiry year field.  |
| **cvvtitle**         | AN   | A meaningful custom title for the cvv field.          |

### New Field IDs 

The following Field IDs have been added in this release:

| iFrame Element        | ID                    |
|-----------------------|-----------------------|
| Card Number Label     | cccardlabel        |
| Card Number Field     | ccnumfield         |
| Expiry Label          | ccexpirylabel      |
| Expiry Month Dropdown | ccexpirymonth      |
| Expiry Year Dropdown  | ccexpiryyear       |
| Expiry Month Field    | ccexpiryfieldmonth |
| Expiry Year Field     | ccexpiryfieldyear  |
| CVV Label             | cccvvlabel         |
| CVV Field             | cccvvfield         |

### Clear Fields when Modified 

After a token is returned, if a user modifies any field on the form, all fields are cleared and must be re-entered before a new tokenization request can be made.

## Date Updated: 6/25/2019 

This release includes the following updates:

### New fullmobilekeyboard URL Parameter 

The iFrame Tokenizer includes a new **fullmobilekeyboard** URL parameter to allow users to enter a forward slash (/) when entering an ACH routing/account number string.

### Card Expiry Returned for for IDTECH USB Card Readers 

The iFrame Tokenizer response message now includes the **expiry** when you use a supported IDTECH USB card reader device (IDTECH SREDkey or IDTECH SecuRED) to capture card data. The expiry and name on card (if available) are captured and stored in the data associated with the token.

### Additional CSS Parameters 

The following CSS parameters are now whitelisted for use with the iFrame Tokenizer:

- appearance: 
- -moz-appearance:
- -webkit-appearance: 
