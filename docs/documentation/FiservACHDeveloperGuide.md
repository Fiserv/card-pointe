# Overview

The CardPointe Gateway is integrated with ACH Payment Services from Fiserv (also known as Fiserv ACH).

This guide provides information for using the CardPointe Gateway API to accept ACH payments from your application.

> Fiserv ACH currently supports the US Dollar (USD) currency only.

# What's New?

## Date Updated: 9/20/2021 

This guide has been updated with additional information and clarifications on [ACH Reversals](#ACH-Reversals). 

## Date Updated: 3/13/2021 

The CardPointe Gateway API includes two additional fields for ACH authorization requests:

- `achDescription`

    An optional description for the transaction, truncated to a maximum of 10 characters.

    For ACH reversals, you **must** specify this field as `Reversal`, as required by NACHA guidelines. See [ACH Reversals](#ACH-Reversals) for more information.

- `achEntryCode`

    Determines the type of ACH transaction. If omitted, this value is determined by the CardPointe Gateway based on the merchant's configuration.

    One of the following values:

    - `CCD` - B2B payment with a signed company agreement on file.

    - `PPD` - Recurring payment, with a signed customer agreement on file.

    - `TEL` - Phone payment (call center), with the customer's recorded verbal consent on file.

    - `WEB` - Web or mobile payment, with a customer's web authorization or agreement on file.

    **Note**: _For refunds, this field is converted to either_ `CCD` _or_ `PPD` _depending on the merchant account configuration._

See [Making an ACH Authorization Request](#Making-an-ACH-Authorization-Request) for more information.

# Understanding ACH Rules and Regulations

Before you begin, you should review and understand the rules and regulations concerning ACH payments.

The National Automated Clearing House Association (NACHA) governs the industry-standard requirements that all merchants and service providers must abide by. The NACHA Operating Rules and Guidelines is a formal standard that outlines specific rules for accepting and processing ACH payments and data security guidelines to protect sensitive account information. As a merchant, it is your obligation to review and abide by these guidelines. To obtain a copy of the NACHA Operating Rules and Guidelines, see https://www.nacha.org/rulesyourway.

Additionally, the ACH Payment Services Operational Guidelines Overview provides helpful information for understanding and complying with the NACHA rules and guidelines.

Click below to download a copy

> [ACH Payment Services Operational Guidelines Overview March 2021](https://developer.cardpointe.com/assets/developer/assets/ACH-Payment-Services-NACHA-Operational-Guidelines-Mar-2021.pdf)

> The ACH Payment Services Operational Guidelines Overview is a supplemental companion to the NACHA Operating Rules and Guidelines. If you have any questions or concerns regarding your compliance, refer to the NACHA Operating Rules and Guidelines.

# Processing ACH Transactions

To process ACH payments on the CardPointe Gateway, you use the CardPointe Gateway API to make authorization requests just like you would for credit card transactions. The [authorization request](#Making-an-ACH-Authorization-Request) supports all of the required fields for processing ACH checking or savings account transactions, including sales and refunds. 

Unlike credit card payments, when a customer authorizes an ACH payment, the funds are withdrawn directly from his or her bank account. This process can take several days, so you should include a monitoring process in your integration to [verify the status of the transaction](#Verifying-ACH-Transactions).

To accept ACH payments, you must capture and handle the customer's bank account and routing number. While you can capture this information and pass it directly to the CardPointe Gateway in an [authorization request](#Making-an-ACH-Authorization-Request), it is a best practice to instead capture this information and tokenize it using a [CardSecure](CardSecure.md)-integrated web form.

## Account Validation

For WEB debit transactions (consumer payments originated on the internet or a mobile device or automatically debited), the account validation service checks the account and routing numbers against a list of known-good accounts to verify that the account can be used to complete the transaction.

If the account is successfully validated, the transaction is approved and settled. If the account is closed, unknown, or has been previously flagged, the transaction is declined in real time and must be reattempted with an alternate payment method. In the event of a decline, the CardPointe Gateway returns a declined authorization response with one of the following reasons:

- `"Declined: bank account failed validation"`
- `"Declined: bank account previously failed validation"`

<!-- theme: warning -->
> See the Fiserv ACH Account Validation support article for detailed information on the account validation service and requirements.

## ACH Reversals

An ACH reversal can be performed to correct an erroneous transaction (for example, the wrong bank account was debited). 

Similarly to a refund, a reversal credits the funds back to the debited account; however, a reversal is only acceptable in the event of an error by the initiator of the original transaction (merchant) and can only be performed by the initiator to correct the error. NACHA enforces strict rules and requirements for processing reversals, including:

- The amount (`amount`) of the reversal must exactly match the amount of the original transaction
- The SEC code (`achEntryCode`) in the reversal request must match the value used in the original transaction.
- The ACH Description (`achDescription`) must be `Reversal` to identify the transaction as a reversal.

See the following article for important updates to the NACHA rules and enforcement for ACH reversals: 

https://www.nacha.org/rules/reversals-and-enforcement

## Using a Web Form to Gather and Tokenize ACH Payment Data

To ensure the security of your customers' data, it is recommended that you use a customer-facing web form, integrated with CardSecure, to capture and tokenize bank account and routing information.

When using a web form to capture and tokenize customer bank account information, include separate fields for the routing number and account number. Send these fields in a CardSecure tokenization request in the format

`"account" : "<routing number>/<account number>"`

For example:

`"account" : "123456789/1234123412341234"`

CardSecure returns a token representing the ACH account information, which you can then use to make an [authorization request](#Making-an-ACH-Authorization-Request) to the CardPointe Gateway.

## Making an ACH Authorization Request

The following table describes the authorization request fields that you must include when processing an ACH payment:

> - Fields in **bold** are required.
> - Underlined fields can be saved to a profile, if `"profile" : "y"` in the request. See the Profile endpoint description for more information.
>   
>     **Note**: _If the authorization request fails or is declined, the profile is not created._
>
> - Most fields in the authorization request correspond to columns on the CardPointe application's Transactions table. See the CardPointe Web App User's Guide for more information.

| Field | Max Length | Type | Comments |
| --- | --- | --- | --- |
| **merchid**	| 16 | N | CardPointe merchant ID, required for all requests.
| <ins> **amount** </ins> | 14 | N | Amount with decimal or without decimal in currency minor units. <br> <br> **Notes**: <br> <br> Only USD is currently supported. <br> Negative amounts (forced credits) are currently not supported. |
| **accttype** | 4 | A | The ACH account type. One of the following values: <br> <br> `ECHK` - checking account <br> `ESAV` - savings account |
| **account** | 19 | N | Can be: <br> **CardSecure Token** - A token, including the bank account and checking numbers, retrieved from the Bolt API, CardSecure API, or the [Hosted iFrame Tokenizer](HostediFrameTokenizer.md) <br> **Bank Account Number** - The clear checking or savings account number. When using this field, the `bankaba` field is also required to include the routing number. <br> <br> **Note**: _To use a stored profile, omit the_ `account` _property and supply the profile ID in the_ `profile` _field instead. See the Profile description for more information._ |
| achDescription | 10	| AN | An optional description for the transaction, truncated to a maximum of 10 characters. <br> <br> For ACH reversals, you **must** specify this field as `Reversal`, as required by NACHA guidelines. See [ACH Reversals](#ACH-Reversals) for more information. |
| achEntryCode | 3 | AN	| Determines the type of ACH transaction. If omitted, this value is determined by the CardPointe Gateway. <br> <br> One of the following values: <br> <br> `CCD` - B2B payment with a signed company agreement on file. <br> `PPD` - Recurring payment, with a signed customer agreement on file. <br> `TEL` - Phone payment (call center), with the customer's recorded verbal consent on file. <br> `WEB` - Web or mobile payment, with a customer's web authorization or agreement on file. <br> <br> **Note**: _For refunds, this field is converted to either_ `CCD` _or_ `PPD` _depending on the merchant account configuration._ |
| **bankaba**	| 9	| N	| Bank routing (ABA) number. Required for ACH authorizations when a bank account number is provided in the `account` field. `bankaba` is **not** required if a CardSecure token (generated from the account/bankaba pair) is provided in the `account` field. |
| <ins> **name** </ins> | 30 | AN | Account holder's name. |
| currency | 3 | AN	| Currency of the authorization (for example, USD for US dollars or CAD for Canadian Dollars). <br> <br> **Note**: _If specified in the auth request, the currency value must match the currency that the MID is configured for. Specifying the incorrect currency will result in a_ `"Wrong currency for merch"` _response_. |
| receipt	| 1	| A	| Optional, to return receipt data fields in the authorization response. <br> <br> Specify **Y** to return additional merchant and authorization data to print on a receipt. <br> <br> Defaults to **N** if not provided. |
| profile	| 24 | AN	| Optional, to create an account profile or to use an existing profile. <br> <br> To create a profile using the account holder data provided in the request, specify **Y**. <br> <br> To use an existing profile for this authorization, omit the account parameter and instead use the profile parameter to supply the 20-digit profile id and 1-3-digit account id string in the format <profile>/<account>.  <br> <br> See Profiles for more information. |
| tokenize | 1 | A | Optional, specify **N** or omit to return an account token in the account field in the response. <br> <br> If tokenize is **Y**, the masked account number is returned in the response. |
| signature	| 6144 | AN	| JSON-escaped, Base64-encoded, Gzipped, BMP of signature data. If the authorization is using a token with associated signature data, then the signature from the token is used. |
| <ins> address </ins>	| 30 | AN	| Account holder's street address, required for AVS verification. |
| <ins> address2 </ins> | 30	| AN | Second address line (for example, apartment or suite number) if applicable.
| <ins> city </ins> | 30	| AN | Account holder's city.
| <ins> postal </ins> | 9 | AN	| The account holder's postal code. <br> <br> If country is "US", must be 5 or 9 digits. Otherwise any alphanumeric string is accepted. Defaults to "55555" if not included in the request or stored profile. <br> <br> **Note**: _It is strongly recommended that your application prompts for postal code for e-commerce or telephone transactions to prevent fraud and downgrades, and to avoid declines on the CardPointe Gateway if the AVS verification option is enabled for your merchant account._
| <ins> region </ins> | 20	| AN | Account holder's region, US State, Mexican State, Canadian Province
| <ins> country	</ins> | 2	| AN | Account holder's country (2-character country code), defaults to "US". <br> <br> Required for all non-US addresses |
| <ins> phone	</ins> | 15 | AN	| Account holder's phone number.
| <ins> email	</ins> | 128	| AN | Account holder's email address.
| orderid	| 19 | AN	| Source system order number. <br> <br> **Note**: _If you include an order ID it must meet the following requirements:_ <br> <br> _The order ID must be a unique value. Using duplicate order IDs can lead to the wrong transaction being voided in the event of a timeout._ <br> _The order ID must not include any portion of a payment account number, and no portion of the order ID should be mistaken for a PAN. If the order ID passes the Luhn check performed by the CardPointe Gateway, the value will be masked in the database, and attempts to use the order ID in an inquire, void, or refund request will fail._
| authcode | 6 | AN	| Authorization code from original authorization (VoiceAuth). <br> <br> For Voice/Capture-Only, the request must include a valid `authcode`.
| taxexempt	| 1	| A	| If `taxexempt` is set to `"N"` for the transaction and a tax is not passed, the default configuration data is used. <br> <br> If `taxexempt` is set to `"Y"` the `taxamnt` is $0.00. |
| taxamnt	| 12 | N | Tax amount for the order, either decimal or in currency minor units (for example, USD Pennies or MXN Centavos). <br> If `taxexempt` is `"Y"` `taxamnt` must be zero or omitted. If `taxexempt` is `"N"`, `taxamnt` must be a positive, non-zero value. |
| ecomind	| 1	| A	| An e-commerce transaction origin indicator, for card-not-present transactions only. <br> <br> One of the following values: <br> <br> `T` - telephone or mail payment <br> `R` - recurring billing <br> `E` - e-commerce web or mobile application <br> <br> **Note**: _While not required, it is recommended that you include the appropriate_ `ecomind` _value for all card-not-present transactions, to ensure that the transaction processes at the appropriate interchange level. _

> This table does not include the complete list of authorization request parameters. See the CardPointe Gateway API's authorization service description for more detailed information on the authorization request and response parameters.

#### Sample ACH Authorization Request

```json
{
    "merchid": "BCX100264539177",
    "account": "123456789",
    "bankaba": "031000503",
    "accttype": "ECHK",
    "amount": "1.00",
    "name": "Test",
    "ecomind": "T"
}
```

#### Sample ACH Authorization Response

```json
{
    "batchid": "100",
    "respcode": "00",
    "expiry": "1120",
    "amount": "1.00",
    "token": "9030425311936789",
    "merchid": "BCX100264539177",
    "respproc": "BPAY",
    "retref": "308055048239",
    "respstat": "A",    
    "account": "9030425311936789",
    "authcode": "AUTH",
    "resptext": "Approved",
    "cvvresp": "U",
    "avsresp": "U",
    "commcard": "N"
}
```

## Verifying ACH Transactions

ACH transactions typically take several business days to process and settle, therefore, it is a best practice to periodically check the status of the transaction to ensure that it is successfully processed and that you are credited for the authorized amount.

You can use the CardSecure Gateway API to programmatically verify the transaction status using the funding and inquire service endpoints.

### Using the Funding Endpoint

> To use the funding endpoint to retrieve your ACH funding data, you must use your Fiserv ACH MID and API credentials associated with this MID.
The funding endpoint provides additional useful information for ACH transactions. Specifically, you can use the funding endpoint to retrieve an ACH return code (`achreturncode`), which provides additional information for rejected ACH transactions.

To use the funding endpoint, you make a request using the merchant ID and the date of the funding event that included the transaction. The funding endpoint returns an array of transaction details for that date.

Use the `retref` for the ACH transaction to locate it in the txns node of the response data. For ACH transactions, the response includes an `achreturncode` field that includes a specific code that explains the reason for the rejection.

The following table describes the possible ACH return code values.

### ACH Return Codes

The following codes are returned when an ACH transaction is rejected. 

| Code | Description |
| --- | --- |
| R01 | Insufficient funds
| R02 | Bank account closed
| R03 | No bank account/unable to locate account
| R04 | Invalid bank account number
| R06 | Returned per ODFI request
| R07 | Authorization revoked by customer
| R08 | Payment stopped
| R09 | Uncollected funds
| R10 | Customer advises not authorized
| R11 | Check truncation entry return
| R12 | Branch sold to another RDFI
| R13 | RDFI not qualified to participate
| R14 | Representative payee deceased or unable to continue in that capacity
| R15 | Beneficiary or bank account holder
| R16 | Bank account frozen
| R17 | File record edit criteria
| R18 | Improper effective entry date
| R19 | Amount field error
| R20 | Non-payment bank account
| R21 | Invalid company ID number
| R22 | Invalid individual ID number
| R23 | Credit entry refused by receiver
| R24 | Duplicate entry
| R25 | Addenda error
| R26 | Mandatory field error
| R27 | Trace number error
| R28 | Transit routing number check digit error
| R29 | Corporate customer advises not authorized
| R30 | RDFI not participant in check truncation program
| R31 | Permissible return entry (CCD and CTX only)
| R32 | RDFI non-settlement
| R33 | Return of XCK entry
| R34 | Limited participation RDFI
| R35 | Return of improper debit entry
| R36 | Return of Improper Credit Entry
| R39	| Improper Source Document
| R40	| Non-Participant in ENR program
| R41	| Invalid transaction code
| R42	| Transit/Routing check digit error
| R43	| Invalid DFI account number
| R44	| Invalid individual ID number
| R45	| Invalid individual name
| R46	| Invalid representative payee indicator
| R47	| Duplicate enrollment
| R50	| State Law affecting RCK Acceptance
| R51	| Item is Ineligible, Notice Not Provided, Signature Not Genuine, or Item Altered (adjustment entries)
| R52	| Stop Payment on Item (adjustment entries)
| R61	| Misrouted return
| R62	| Incorrect trace number
| R63	| Incorrect dollar amount
| R64	| Incorrect individual identification
| R65	| Incorrect transaction code
| R66	| Incorrect company identification
| R67	| Duplicate return
| R68	| Untimely return
| R69	| Multiple errors
| R70	| Permissible return entry not accepted
| R71	| Misrouted dishonored return
| R72	| Untimely dishonored return
| R73	| Timely original return
| R74	| Corrected return
| R80	| Cross Border Payment Coding Error
| R81	| Non-Participant in Cross-Border Program
| R82	| Invalid Foreign Receiving DFI identification
| R83	| Foreign Receiving DFI Unable to Settle

### Using the Inquire Endpoint

The inquire endpoint provides information on completed authorizations.

You can use the inquire endpoint if you have the retrieval reference number (`retref`) from the authorization response. If you don't have the `retref`, but you included a unique order ID in the authorization request, then you can use the inquireByOrderId endpoint instead.

The inquire response includes a settlement status (`setlstat`) field that displays the settlement status of the transaction. Note that the settlement status initially displays "Queued for Capture" for ACH transactions, and the value is updated once the batch is transmitted. If `"setlstat" : "rejected"` you can use the funding endpoint to gather more detailed information.
