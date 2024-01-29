# Using the Batch Authorization Service to Process Batch Files

This guide provides information for using the [CardPointe Gateway's](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md) Batch Authorization Service (BAS) to submit batch files for authorization processing. 

The request file submitted to BAS will contain all required data elements within an [Authorization Request](.../api/?type=post&path=/cardconnect/rest/auth) to the CardPointe Gateway. BAS collects the supplied data and submits all requests on behalf of the client. A response file is returned, containing data from the associated [Authorization Response](.../api/?type=post&path=/cardconnect/rest/auth).

> Using BAS to transmit batch files is **not** a drag-and-drop solution. Integrating BAS requires development work and testing.

## Client Onboarding Checklist

**1.** [Obtain the CardPointe Gateway PGP public key](#pgp-keys). Clients must encrypt all request files using this key.
   
**2.** Send your client pgp public key to integrationdelivery@fiserv.com. Your public key will be used to encrypt response files.
   
**3.** Obtain your SFTP credentials required to submit requests and poll for response files
   
**4.** Obtain your CardPointe Gateway API URL and credentials from.

## Supported Authorization Types

The Batch Authorization Service supports all data elements that the CardPointe Gateway [Authorization Service](.../api/?type=post&path=/cardconnect/rest/auth) supports.

| Authorization Type | Details |
| --- | --- |
| Authorization | 'capture' column set to 'N' |
| Authorization with Capture | 'capture' column set to 'Y' |
| Refund without Reference | 'amount' column set to a negative value <br> **Note**: _The merchant account must be configured to support this transaction type._ | 

## PGP Keys

All files that are submitted to or retrieved from Secure Exchange must be PGP encrypted. This requires two PGP key pairs:

- **Request keypair** - We hold the private key and you download the public key. You must use this key to encrypt all BAS request files before uploading them to Secure Exchange.
- **Response keypair** - You hold the private key and send the public key to us. The Secure Exchange service uses your key to encrypt all BAS response files.

Click below to download the public PGP keys for the UAT and Prod environments:

>[Secureexchange_uat](https://developer.cardpointe.com/assets/developer/assets/secureexchange_uat.txt)

>[Secureexchange_cardconnect_prod](https://developer.cardpointe.com/assets/developer/assets/secureexchange_cardconnect_prod.txt)

Note that these keys are "armored." Run the "gpg --dearmor" command to de-armor the keys.

For example:

`$ gpg --dearmor < secureexhange_uat.txt > secureexchange_uat.gpg`

## BAS Request File Specifications

The Batch Authorization Service supports all fields and data elements that the CardPointe Gateway [Authorization Service](.../api/?type=post&path=/cardconnect/rest/auth) supports.

<!-- theme: warning -->
> Note the following important considerations:
>
> **CardSecure Tokens**
>
> The value within each record's account field **must** be a valid CardSecure token. BAS does not handle clear-text card numbers.
>
> If your integration does not yet support CardSecure tokens, contact integrationdelivery@fiserv.com.
> 
> **Handling Stored Credentials**
> 
> If your application stores and reuses payment tokens and customer information to process recurring payments, or if you use the CardPointe Gateway's profile service to create and store customer profiles and process card-not-present payments using the associated `profileid` and `accountid`, you may need to update your application to include additional fields for these transactions in your BAS request file. See the [Visa and Mastercard Stored Credential Transaction Framework Mandate](https://support.cardpointe.com/compliance/visa-stored-credential-transaction-framework-mandate) article for detailed information on these requirements.

### BAS Request File Name

The BAS Request file name must be unique, and must end with `.csv.pgp.`

We recommend the following format:

`<merchantname>_<date>_<sequence#>.csv.pgp`

Where:

- `<merchantname>` is your merchant ID or other identifier.

- `<date>` is the date of the batch.

- `<sequence#>` is a unique identifier that you assign to the batch.

- `.csv.pgp` is **required**.

- The file name must not include spaces.

For example:

`mystore_20210715_1.csv.pgp`

### Sample BAS Request File

```html
merchid,orderid,accttype,account,expiry,amount,capture,currency,email,name,address,address2,city,region,country,postal,phone,company,taxexempt,items,invoiceid,ponumber,shiptozip,shipfromzip,shiptocountry,orderdate,taxamnt,frtamnt,dutyamnt,discamnt,waiver,fee_value,fee_format,gsacard
081800000001,1000102424,VISA,9404077753845057,1299,100,Y,USD,test@test.com,name1,add1,add2,city1,,US,19020,,,,Y,"unitcost='1.00',description='desc with no comma',uom='lb'|unitcost='1.00',description='desc with,comma',uom='lb'",,,,,,,,,,,Y,,,Y
081800000001,1000102425,VISA,9404077753845057,1299,100,N,USD,,name1,add1,add2,,,,19020,111-111-11111,,,,"unitcost='1.00',description='desc with no comma',uom='lb'",,,,,,,,,,,,,,
081800000001,1000102426,VISA,9404077753845057,1299,100,N,USD,test@test.com,,add1,,city1,,US,19020,,,,,"unitcost='1.00',description='desc with no comma',uom='lb'|unitcost='1.00',description='desc with,comma',uom='lb'",,,,,,,,,,,,,,
081800000001,1000102427,VISA,9404077753845057,1299,100,Y,USD,test@test.com,name1,,add2,,,US,19020,111-111-11111,,,N,,,,,,,,,,,,,,,
```

### BAS Request Fields

Fields in **bold** are required.

Fields with a # indicate these fields can be used as part of a profile. See the CardPointe Gateway API [profile](.../api/?type=post&path=/cardconnect/rest/profile) description for more information.

| Fields | Size | Type | Comments |
| --- | --- | --- | --- |
| **orderid** | 50 | AN | Source system order number. <br> <br> Populate this field with a unique value that can identify the sales order. <br> <br> This value will be associated with the response record. |
| **merchid** | 12 | N | CardPointe merchant ID (MID), required for all requests |
| **amount** | 12 | N | Amount with decimal or without decimal in currency minor units (for example, USD Pennies or EUR Cents), <br> <br> This value identifies the authorization type: <br> <br> **Positive** - Authorization request <br> **Zero** - Account Verification request, if AVS and CVV verification is enabled for your merchant account. Account Verification is not supported for eCheck (ACH) authorizations. <br> **Negative** - Refund without reference (Forced Credit). Merchants must be enabled for forced credit. When referencing an existing authorization, use [refund](.../api/?type=post&path=/cardconnect/rest/refund). |
| currency | 3 | AN | Currency of merchant settlement <br> (USD for US dollars, CAD for Canadian Dollars, etc.) |
| **expiry#** | 3 | N | Card Expiration in either MMYY or YYYYMMDD format, not required for ECHK. |
| **account#** | 19 | N | **CardSecure Token** - Retrieved from the CardPointe Integrated Terminal API, CardSecure API, or the Hosted iFrame Tokenizer. <br> <br> The token can be generated from from either credit card or ACH (banking/routing number) data. If ACH, the `merchid` must be a specific Profit Stars Merchant ID. <br> <br> **Note**: To use a CardPointe profile, omit the `account` field and supply the `profileid`. See the CardPointe Gateway API [profile](.../api/?type=post&path=/cardconnect/rest/profile) description for more information. |
| profile | 1 or 16 | AN | Optional, **Y** to create an account profile upon initial authorization. <br> <br> Creating a profile generates a 20-digit numeric `profileid` / `acctid` (optional) value to use in authorizations when the `account` field is omitted. See the CardPointe Gateway API [profile](.../api/?type=post&path=/cardconnect/rest/profile) description for more information. |
| acctid | 3 | N | Account identifier within a [profile](.../api/?type=post&path=/cardconnect/rest/profile), specifies unique token |
| capture | 1 | A | Optional, **Y** to capture the transaction for settlement if approved. |
| signature | 6144 | AN | JSON-escaped, Base64-encoded, Gzipped, BMP of signature data. <br> <br> If the authorization is using a token with associated signature data, then the signature from the token is used. |
| cvv2 | 4 | N | CVV2/CVC/CID value |
| name# | 30 | AN | Account name <br> <br> Optional for credit cards but required for electronic checks. |
| address# | 30 | AN | Account street address (required for AVS) |
| city# | 30 | AN | Account city |
| postal# | 9 | AN | Account postal code, defaults to "99999". <br> <br> If country is "US", must be 5 or 9 digits; otherwise, any alphanumeric string is accepted. |
| region# | 20 | AN | US State, Mexican State, Canadian Province |
| country# | 2 | AN | Account country (2 character country code), defaults to "US." <br> <br> Required for all non-US addresses. |
| phone# | 15 | AN | Account phone |
| email# | 30 | AN | E-Mail address of the account holder |
| authcode | 6 | AN | Authorization code from original authorization (VoiceAuth). <br> <br> For Voice/Capture-Only, include valid `authcode`. |
| taxexempt | 1 | A | If `taxexempt` is set to **No** for the transaction and a `taxamnt` is not specified, the default configuration data is used. <br> <br> If `taxexempt` is set to **Yes**, the default tax rate is used. |
| taxamnt | 12 | N | Tax amount for the order, either decimal or in currency minor units (for example, USD Pennies or MXN Centavos). <br> <br> If `taxexempt` is **Y**, `taxamnt` must be zero or omitted. If `taxexempt` is **N**, `taxamnt` must be a positive, non-zero value. |
| termid | 30 | AN | Terminal Device ID |
| accttype# | 6 | A | One of PPAL, PAID, GIFT, PDEBIT, otherwise not required. |
| ecomind | 1 | A | Identifies the payment origin. Options are: <br> <br> **T** - telephone or mail <br> **R** - recurring <br> **E** - ecommerce/Internet |
| invoiceid | 12 | AN | Invoice ID, optional. <br> <br> Defaults to `orderid` from authorization request |
| userfields | 4000 Bytes | AN | The userfields object, or array, is a series of name-value pairs that are meaningful to the merchant. <br> <br> See the [userfields](.../api/?type=post&path=/cardconnect/rest/profile) description for more information. <br> <br> Example of userfields value: <br> <br> `"{""UDF1"": ""SomeText"", ""UDF2"": ""SomeMoreText""}"` |

#### Capture Level 2 Data

If available, the Level 2 fields can be provided in the capture request and should be provided for corporate or purchase cards to qualify for improved interchange rates.

| Field | Size | Type | Comments |
| --- | --- | --- | --- |
| ponumber | 36 | AN | Customer purchase order number |
| taxamnt | 12 | N | Tax amount either decimal or in currency minor units (i.e. USD Pennies, MXN Centavos), `taxamnt` of zero indicates tax-exempt purchaser <br> <br> **Note**: Do not send a non-zero tax amount for a tax exempt order. |

Many Level 2 protocols refer to a "Customer Code" described as "an identifier the customer would recognize". The CardPointe Gateway populates this field with the `ponumber`. If the `ponumber` is not provided, then the orderid from the authorization request is used. If neither the `ponumber` or the `orderid` is available, the CardPointe Gateway Retrieval Reference Number (`retref`) is used.

The Level 2 protocol also requires certain fields about the merchant, such as Postal Code and Tax Identification Number. These fields are filled from the configuration table created during the merchant boarding process.

#### Capture Level 3 Data

If available, Level 3 line item data can be sent with the capture request, particularly for commercial or corporate payment cards. To qualify for Level 3 Interchange rates, Level 2 data (described above) must also be provided.

Level 3 data includes additional **Order Level** items such as freight and discount amount, as well as information from each line item of the order. These are stored as an array in the items field.

| Field | Size | Type | Comments |
| --- | --- | --- | --- |
| frtamnt | 12 | N | Total order freight amount, default 0 |
| dutyamnt | 12 | N | Total order duty amount, default 0 |
| orderdate | 8 | N | YYYYMMDD <br> <br> For most industries this is the delivery date for the order <br> <br> Defaults to System Date when captured |
| shiptozip | 12 | AN |	If country is "US", must be 5 or 9 digits <br> <br> Otherwise, any alphanumeric string is accepted |
| shipfromzip |	12 | AN | Same as above |
| shiptocountry | 2 | A | Ship To Country Code, default is US |
|Items | varies | Array | Array of Items |

#### Items Field for BAS

The items field can be passed as a pipe-delimited string, or as a JSON array.

If using a pipe-delimited string:

- The Items field must be enclosed in double quotation marks (").

- Each item must be separated by a pipe delimiter (|).

- Elements in each item must be a `key='value'` pair.

- Elements in each item must be separated by a comma (,).

- Each value must be enclosed in single quotation marks (').

- Values containing a single quote (') must be escaped with a backslash (\).

	For example:
	```json
 	description='desc with a single quote\''
	```
- Values containing a comma (,) must also be enclosed in double quotation marks (").

	For example:
	```json
 	"unitcost='1.00',description='desc with no comma',uom='lb'|unitcost='1.00',description='desc with,comma',uom='lb'"
	```
 
If using a JSON array:

- The array must be enclosed in double quotation marks (" ").

- Each item in the array must be enclosed in curly braces ({ })

- Each key-value pair must be formatted as `""key"":""value""`.

- Each key-value pair must be separated by a comma (,).

	For example:
	```json
 	"[{""uom"":""lb"",""unitcost"":""1.00"",""description"":""desc with no comma""},{""uom"":""lb"",""unitcost"":""1.00"",""description"":""desc with,comma""}]"
	```

 #### Line Item Records

| Field | Size | Type | Comments |
| --- | --- | --- | --- |
| lineno | 4 | N | Optional line number for each item. Line numbers do not need to be specified sequentially; however, if no `lineno` value is included, items are assigned sequential line numbers. |
| material | 12 | AN | Optional material code (also referred to as a Commodity Code) for each item. |
| description | 26 | AN | An item description, required. |
| upc | 14 | N | Optional UPC code for each item. If `upc` is included, the value must not be all zeros. <br> <br> Some processors limit this value to 12 characters. |
| quantity | 12 | N | Quantity of the item purchased. Can be a whole amount or amount with up to three decimal places. |
| uom | 8 | AN | Unit of measure (for example, "each" or "ton"). <br> <br> Some processors limit this value to 4 characters. |
| unitcost | 12 | N | Optional item cost, excluding tax as a decimal amount or in currency minor units (for example, USD Pennies or MXN Centavos formatted as 1.23 or 123 for $1.23).<br> <br> When omitted, this value is calculated as `netamnt`/`quantity`. |
| netamnt | 12 | N | Net total of `unitcost` x `quantity`, excluding tax and discount, as a decimal amount or in currency minor units. <br> <br> `netamnt` is always a positive value, even for refunds. <br> <br> Zero amounts are allowed for no-charge items, such as a manual, that are included in the order. |
| taxamnt | 12 | N | Tax amount for each item, as a decimal amount or in currency minor units. The sum total of `taxamnt` for all items equals the total tax amount for the order. <br> <br> The total of `netamnt` + `taxamnt` - `discamnt` equals the order amount. <br> <br> `taxamnt` must be zero or omitted if `taxexempt` is **N** and non-zero if `taxexempt` is **Y**. |
| taxexempt | 1 | true/false | Indicates whether or not the order is tax exempt. <br> <br> `taxexempt` should be **true** if: <br> <br> The payment card BIN type is GSA (government). <br> **Note**: _All GSA orders are set to taxexempt regardless of this setting_. <br> <br> The merchant is a government agency. <br> <br> In this case, you must include `taxexempt` = true in the request for each order. <br> `taxexempt` defaults to false. |
| discamnt | 12 | N | Optional discount amount for each item, as a decimal amount or in currency minor units. |

## BAS Response File Specifications

BAS response files are pgp-encrypted with the key that you provided.
Response files will return one CardPointe Gateway response per line.

### BAS Response File Name

The response file name matches the corresponding request file name with the extension ".done" appended.

For example:

`<name>_<yymmdd>_<seq>.csv.pgp.done`

### Sample BAS Response File

```json
id,status,message,orderid,retref,amount,resptext,commcard,cvvresp,batchid,avsresp,respcode,merchid,token,authcode,respproc,respstat,account1,DONE,,1000102424,2345676543,2.00,Approval,C ,P,543,A,00,081800000001,9404077753845057,PPS001,FNOR,A,94040777538450572,DONE,,1000102425,2345676543,3.00,Approval,C ,P,543,A,00,081800000001,9404077753845057,PPS001,FNOR,A,94040777538450573,ERROR,401 Un-Authorized,10001024264,DONE,,1000102427,2345676543,4.00,Declined,C ,P,543,U,00,081800000001,9404077753845057,PPS063,FNOR,C,94040777538450575,LOAD_ERROR,IOException reading next record: java.io.IOException: (line 2) invalid char between encapsulated token and delimiter
```

### BAS Response File Fields

| Field | Content | Max Len | Comments |
| --- | --- | --- | --- |
| id | Integer | 20 | BAS transaction ID |
| status | String | 10 | Final processing status: "DONE" or "ERROR" |
| message | String | None | Details explaining "ERROR" or "LOAD_ERROR" |
| orderid | Order ID | 50 | The orderid specified in the Request file |
| retref | Retrieval reference number | 12 | CardPointe retrieval reference number from authorization response |
| amount | Amount | 12 | Authorized amount. Same as the request amount for most approvals. The amount remaining on the card for prepaid/gift cards if partial authorization is enabled. Not relevant for declines. |
| resptext | Response text | - | Text description of response |
| commcard | Commercial card flag | 1 | Y if a Corporate or Purchase Card |
| cvvresp | CVV response code | 1 | Alpha-numeric CVV response. |
| batchid | Batch ID | 10 | Automatically created and assigned unless otherwise specified |
| avsresp | AVS response code | 2 | Alpha-numeric AVS response. |
| respcode | [Response code](?path=docs/documentation/GatewayResponseCodes.md) | - | Alpha-numeric response code that represents the description of the response |
| merchid | Merchant ID | 12 | Copied from request |
| token (if requested) | Token | 19 | A token that replaces the card number in capture and settlement requests if requested |
| authcode | Authorization code | 6 | Authorization Code from the Issuer |
| respproc | Response processor | 4 | Abbreviation that represents the platform and the processor for the transaction |
| respstat | Status | 1 | **A** - Approved <br> **B** - Retry <br> **C** - Declined |
| account | Account number | 19 | Copied from request, masked |

## Submitting a Batch File

You submit request files and retrieve response files using Secure Exchange, via SFTP. When you log in to Secure Exchange you have access to three directories: /request, /response, and /dlc (dead-letter channel).

The following procedure describes each folder's purpose:

**1.** You upload an encrypted request file into the **/request** directory.

- BAS decrypts the file and runs authorization requests, logging each response in a new response file.

**3.** You poll the server for encrypted responses files in the **/response** directory.

**4.** If an error occurs when attempting to decrypt the pgp file, the encrypted request file is placed in the **/dlc** directory.

- Contact Support if you encounter this scenario.

## Viewing Batch File Transactions in CardPointe

To view reporting details for processed batches in CardPointe, do the following:

**1.** Log in to CardPointe and access the merchant account.
  
**2.** Click **Reporting** in the menu bar, and select the **Transactions** tab.
   
**3.** Click the **Front End** column header and select **SecureExchange** to display the transactions processed by BAS.
   
See the [CardPointe Web App support page](https://support.cardpointe.com/cardpointe/cardpointe-desktop-app#reporting) for more information on using CardPointe.
