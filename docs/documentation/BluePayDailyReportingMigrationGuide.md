# BluePay Daily Reporting Migration Guide

This guide provides information to assist integrators in the migration from using BluePay daily reports to using the Funding endpoint of the CardPointe Gateway API to gather merchant-level transaction data.

For detailed information on the CardPointe Gateway API and reporting capabilities, see the [CardPointe Gateway API Developer Documentation](?path=docs/APIs/CardPointeGatewayAPI.md).

Merchants and partners who previously used the BluePay Daily Report 2 interface to obtain details can now use the CardPointe Gateway API Funding endpoint to obtain similar data. However, due to the differences in the request and response content, it is important to understand how this change affects your reporting options.

# Migrating to the CardPointe Funding Endpoint 

## Prerequisites 

Before you begin, you must meet the following prerequisites:

- To use the Funding endpoint to gather transaction data for a merchant, the merchant account must be boarded to the CardPointe Gateway. 
- To make requests to the CardPointe Gateway API and the Funding endpoint, you must obtain user credentials. 

Contact the [ISV Integrations Team](isvintegrations@cardconnect.com) for assistance.

## Mapping your Data to the Funding Endpoint Data 

The following spreadsheet provides a detailed mapping of values returned for the BVDailyReport2 and their corresponding values in the CardPointe Gateway Funding endpoint responses. 

> [BVDailyReport To CardPointe Funding Endpoint API Mapping](https://developer.cardpointe.com/assets/developer/assets/BVDailyReport2-to-CardConnect-Funding-Endpoint-API-Mapping.xlsx)

See the following section for detailed information on using the Funding endpoint, and for understanding the response values.

# Integrating the Gateway API Funding Endpoint 

## Overview 

The Funding endpoint provides merchant funding information, including supplemental transaction and funding adjustment detail. This information is provided to the CardPointe Gateway by the host payment processing platform (for example, First Data Omaha), typically 1 or 2 days after the merchant's activity.

## Using the Funding Endpoint 

### Funding Request 

Use the following request to get information from the Funding endpoint:

| Method | URL | Headers
| --- | --- | ---
| GET	| https://hostname:port/cardconnect/rest/funding?merchid=<merchid>&date=<MMDD> | Content-Type: application/json <br> Authorization: Basic |

> Merchant ID is required. If the date field is null, the service will check for any funding data that has not already been retrieved. Otherwise, status information will be returned for batches matching the two parameters. If no batches are found or are ready to return, the service returns an empty vector.

### Funding Response 

Requests to the Funding endpoint return merchant funding data in an array of chargeback, adjustment, funding, and transaction (txn) records. The following table illustrates the high-level content of the funding response.

#### Funding Response Example (JSON)

```json
{
    "fundingmasterid": "15200186146335"",
    "fundingdate": "2018-05-18",
    "chargebacks": [],
    "adjustments": [],
    "datechanged": null,
    "fundings": [],
    "merchid": "542041",
    "txns": []
}
```

The following table describes each property or node returned by the Funding response.

| Field	| Size | Type	| Comments
| --- | --- | --- | ---
| fundingmasterid	| 22 | Funding Master ID | Unique key for an aggregated merchant's daily funding.
| fundingdate	| 8	| Funding Date YYYY-MM-DD	| The date of the bank funding event.
| chargebacks	| Varies | Data Array	| This array represents all the chargebacks for the given date provided. The chargebacks node is only available for merchants configured to obtain this data. <br> <br> See chargebacks Node for more information.
| adjustments	| Varies | Data Array	| Amounts credited to or deducted from account to resolve processing or billing discrepancies. <br> <br> See adjustments Node for more information.
| datechanged	| 8	| Date YYYY-MM-DD	| The date the record was last updated.
| fundings | Varies	| Data Array | This array represents information about the deposit executed. <br> <br> See fundings Node for more information.
| merchid	| 16 | N | The merchant account ID related to the funding event.
| txns | Varies	| Data Array | This array represents all the transactions funded for the given date provided. <br> <br> See txns Node for more information.

### chargebacks Node 
Chargeback data is only applicable to specific clients that have been configured appropriately.

#### chargebacks Node Example

```json
"chargebacks": [
        {
            "depositdate": "null",
            "amount": "92.15",
            "transactiondate": "2017-10-05",
            "reasondescription": "Duplicate Processing",
            "reasoncode": "34",
            "datechanged": "2018-09-27",
            "sourcetransactionid": "18749749",
            "transactionamount": "92.15",
            "chargebackdate": "2017-10-05",
            "sequencenumber": "268693",
            "dateadded": "2018-09-27",
            "merchid": "542041",
            "authcode": "B70WFN",
            "fundingmasterid": "15200186146335",
            "fundingchargebackid": 5,
            "casenumber": "771886445001",
            "acquirereferencenumber": "55500807123206000059099",
            "cardnumber": "533248XXXXXX8987",
            "retref": "278090245692",
            "invoicenumber": "0000005909"
        }
    ],
```

The following table describes each property returned by the chargebacks node.

| Field                  | Content                                               | Max Length |
|------------------------|-------------------------------------------------------|------------|
| depositdate            | Date YYYY-MM-DD                                       | 8          |
| amount                 | The Chargeback amount                                 | 20, 2 dec  |
| transactiondate        | Date YYYY-MM-DD                                       | 8          |
| reasondescription      | Chargeback description                                | 200        |
| reasoncode             | Chargeback reason code                                | 4          |
| datechanged            | Date Changed YYYY-MM-DD                               | 8          |
| sourcetransactionid    | Source Transaction ID                                 | 100        |
| transactionamount      | The Transaction amount                                | 20, 2 dec  |
| chargebackdate         | Date of Chargeback YYYY-MM-DD                         | 8          |
| sequencenumber         | Chargeback Sequence Number                            | 30         |
| dateadded              | Date Added YYYY-MM-DD                                 | 8          |
| merchid                | Merchant ID                                           | 30         |
| authcode               | Authorization code                                    | 8          |
| fundingmasterid        | Funding Master ID                                     | 22         |
| fundingchargebackid    | Funding Chargeback ID                                 |            |
| casenumber             | Chargeback Case Number                                | 30         |
| acquirereferencenumber | Acquirer Reference Number                             | 30         |
| cardnumber             | Masked Card Number                                    | 30         |
| retref                 | Retrieval Reference Number, CardPointe Transaction ID | 30         |
| invoicenumber          | Transaction Invoice Number                            |

### adjustments Node 

If no adjustment detail is listed by the host, any offset to the funding amount will be listed as other adjustment in this record.

#### adjustments Node Example

```json
"adjustments": [
        {
            "fundingmasterid": "15200186146335",
            "amount": "-31.72",
            "datechanged": "2018-09-27",
            "fundingadjustmentid": "15200349146338",
            "description": "THE CARDHOLDER DID NOT AUTHORIZE THE CHARGE.",
            "currency": "USD",
            "category": "REVERSAL",
            "type": "CHARGEBACKS/CHARGEBACK REVERSALS",
            "dateadded": "2018-09-27",
            "merchid": "542041"
        },
        {
            "fundingmasterid": 15200186146335,
            "amount": "-5.49",
            "datechanged": "2018-09-27",
            "fundingadjustmentid": "15200350146338",
            "description": "THIRD PARTY ADJUSTMENTS",
            "currency": "USD",
            "category": "THIRD PARTY",
            "type": "THIRD PARTY ADJUSTMENTS",
            "dateadded": "2018-09-27",
            "merchid": "542041"
        }
    ],
```

The following table describes each property returned by the adjustments node.

| Field               | Content                      | Max Length | Comments                                                                                                                                       |
|---------------------|------------------------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| fundingadjustmentid | Funding Adjustment ID        | 22         | CardPointe unique system identifier for adjustment                                                                                             |
| fundingmasterid     | Funding Master ID            | 22         | Unique key for an aggregated merchant's daily funding                                                                                          |
| merchid             | MID                          | 25         | Merchant identifier assigned by host <br> <br> Refer to the Fundings Response Appendix (Adjustment Group) for possible field values.           |
| category            | Adjustment Group             | 100        | Category of deposit adjustment <br> <br> Refer to the Fundings Response Appendix (Adjustment Group) for possible field values.                 |
| type                | Adjustment Group Description | 100        | Type of deposit adjustment                                                                                                                     |
| description         | Adjustment Description       | 200        | Detailed description of adjustment                                                                                                             |
| amount              | Adjustment Amount            | 12         | Amount of adjustment                                                                                                                           |
| currency            | Currency Code                | 10         | Three digit code identifying the funded currency <br> <br> Refer to the Fundings Response Appendix (Currency Code) for possible field values.  |
| datechanged         | Date YYYY-MM-DD              | 8          | The date when the record was last updated                                                                                                      |
| dateadded           | Date YYYY-MM-DD              | 8          | The date when the transaction was created                                                                                                      |

### fundings Node 

The fundings node response provides information on the deposit record.

#### fundings Node Example

```json
"fundings": [
        {
            "fundingid": "15200208146336",
            "netsales": "10.03",
            "totalfunding": "-27.18",
            "fee": "0",
            "datechanged": "2018-09-27",
            "deposittrancode": "22",
            "ddanumber": "3XXXXX0540",
            "thirdparty": "-5.49",
            "dateadded": "2018-09-27",
            "fundingmasterid": "15200186146335",
            "reversal": "-31.72",
            "interchangefee": "0",
            "adjustment": "0",
            "currency": "USD",
            "depositachtracenumber": "121000240002309",
            "servicecharge": "0",
            "otheradjustment": "0",
            "abanumber": "121140399"
        }
    ],
```

The following table describes each property returned by the adjustments node.

| Field                 | Content                  | Max Length | Comments                                                                                                                                         |
|-----------------------|--------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| fundingmasterid       | Funding Master ID        | 22         | Unique key for an aggregated merchant's daily funding                                                                                            |
| merchid               | Merchant ID              | 25         | Merchant identifier assigned by host                                                                                                             |
| netsales              | Net Sales                | 12         | Total sales processed by merchant                                                                                                                |
| thirdparty            | Third Party              | 12         | Transactions that are passed directly to a third party service provider for processing and/or funding                                            |
| adjustment            | Adjustment               | 12         | Amounts credited to or deducted from account to resolve processing or billing discrepancies                                                      |
| interchangefee        | Interchange Fees         | 12         | Variable amounts established by the Card Associations for processing transactions                                                                |
| servicecharge         | Service Charges          | 12         | Amounts charged to authorize process and settle card transactions                                                                                |
| fee                   | Fees                     | 12         | A range of transaction-based and/or fixed amounts for specific card processing services                                                          |
| reversal              | Reversals                | 12         | Transactions that are challenged or disputed by a cardholder or card-issuing bank.                                                               |
| otheradjustment       | Other Adjustment         | 12         | General adjustment classification. Adjustment type not provided by host.                                                                         |
| totalfunding          | Funding Amount           | 12         | Total amount of debit or credit event                                                                                                            |
| fundingdate           | Funding Date             | 8          | The date of the bank funding event                                                                                                               |
| currency              | Currency Code            | 3          | Three digit code identifying the funded currency. <br> <br> Refer to the Fundings Response Appendix (Currency Codes) for possible field values.  |
| ddanumber             | DDA Number               | 20         | Masked demand deposit account number                                                                                                             |
| abanumber             | ABA Number               | 20         | American Bankers Association (ABA) that identifies the bank                                                                                      |
| fundingid             | N                        | 22         | Unique key identifier for merchants with daily funding activity                                                                                  |
| datechanged           | Date YYYY-MM-DD          | 8          | The date when the record was last updated                                                                                                        |
| dateadded             | Date YYYY-MM-DD          | 8          | The date when the transaction was created                                                                                                        |
| deposittrancode       | Deposit Transaction Code | 10         |                                                                                                                                                  |
| depositachtracenumber | Deposit ACH Trace Number | 20         |

### txns Node 

The transactions node provides information on each transaction record within the funding event.

#### txns Node Example

```json
"txns": [
        {
            "date": "2018-05-17",
            "parentretref": "166432268693",
            "sourcetransactionid": "15578657",
            "type": "SALE",
            "respcode": "00",
            "fundingtxnid": "15200186146335",
            "achreturncode": "R03",
            "currency": "USD",
            "cardtype": "Credit",
            "retref": "137016250883",
            "invoicenumber": "2389343",
            "amount": "5.01",
            "downgradereasoncodes": "27 37 39 44 89",
            "fundingid": "15200230146336",
            "cardproc": "PSTR",
            "batchid": "313",
            "interchangeunitfee": "0",
            "authcode": "P9WMW0",
            "plancode": "363",
            "authdate": "20180517140803",
            "cardbrand": "ECHK",
            "terminalnumber": "23837493",
            "cardnumber": "21XXXXXXXXXXXX9999",
            "status": "Processed",
            "interchangepercentfee": "0"
        },
        {
            "date": "2018-05-17",
            "amount": "5.02",
            "downgradereasoncodes": null,
            "fundingid": "15200231146336",
            "cardproc": "PSTR",
            "sourcetransactionid": null,
            "type": "SALE",
            "batchid": "313",
            "respcode": "00",
            "interchangeunitfee": "0",
            "authcode": "8BWMW0",
            "plancode": null,
            "authdate": "20180517140814",
            "fundingtxnid": "15200186146335",
            "achreturncode": "R03",
            "cardbrand": "ECHK",
            "currency": "USD",
            "terminalnumber": null,
            "cardnumber": "21XXXXXXXXXXXX9999",
            "cardtype": "Credit",
            "retref": "137017250894",
            "status": "Processed",
            "interchangepercentfee": "0",
            "invoicenumber": null
        }
    ]
```

The following table describes each property returned by the txns node.

| Field                 | Content                           | Max Length | Comments                                                                                                                                                 |
|-----------------------|-----------------------------------|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| fundingtxnid          | Transaction ID                    | 22         | Transaction ID for each record                                                                                                                           |
| retref                | Gateway ID                        | 30         | Gateway reference number (aka RetRef)                                                                                                                    |
| interchangeunitfee    | Interchange unit fee              | 12         | Unit-based fee charged for acceptance of card transaction                                                                                                |
| interchangepercentfee | Interchange percent fee           | 12         | Percentage-based fee charged for acceptance of card transaction                                                                                          |
| date                  | Transaction Date                  | 8          | Date the transaction occurred                                                                                                                            |
| cardnumber            | Card Number                       | 25         | Masked card number used for transaction                                                                                                                  |
| cardtype              | Card Type                         | 20         | Type of card used for transaction <br> <br> Refer to the Fundings Response Appendix (Card Type) for possible field values.                               |
| cardbrand             | Card Brand                        | 30         | The brand associated with the card <br> <br> Refer to the Fundings Response Appendix (Card Brand) for possible field values.                             |
| amount                | Amount                            | 12         | The transaction amount                                                                                                                                   |
| currency              | Currency Code                     | 10         | Three character currency code associated to transaction <br> <br> Refer to the Fundings Response Appendix (Currency Codes) for possible field values.    |
| date                  | Date                              | 8          | Date the transaction occurred                                                                                                                            |
| type                  | Type                              | 25         | Type of transaction Processed <br> <br> Refer to the Fundings Response Appendix (Transaction Type) for possible field values.                            |
| status                | Type                              | 100        | Status of transaction Funding <br> <br> Refer to the Fundings Response Appendix (Transaction Status) for possible field values.                          |
| userfield [0..9]      | Custom Field                      | 20         | A valued passed by the merchant with the original authorization to further identify the transaction (see "userfields" within the Authorization Service). |
| fundingid             | Funding Identifier                | 22         | CardConnect unique key identifier for merchant transaction funding activity.                                                                             |
| plancode              |                                   | 10         |                                                                                                                                                          |
| downgradereasoncodes  | Downgrade Reason Codes            | 100        |                                                                                                                                                          |
| invoicenumber         | Invoice Number                    | Num        |                                                                                                                                                          |
| sourcetransactionid   | Source Transaction ID             | 100        |                                                                                                                                                          |
| terminalnumber        | Terminal Number                   | 10         |                                                                                                                                                          |
| achreturncode         | ACH Return Code                   | 5          |                                                                                                                                                          |
| parentretref          | Parent Retrieval Reference Number | 30         |

### Fundings Response Appendix 

<!--
type: tab
titles: Card Type Responses, Card Brand Responses
-->

See below for a legend of possible return values for the `cardtype` field. 

| Card Type | Description                                                        |
|-----------|--------------------------------------------------------------------|
| Credit    | Payment card issued by a bank, business, etc., allowing the holder to purchase goods or services on credit.  |
| Debit     | Payment card issued by a bank allowing the holder to transfer money electronically to another bank account when making a purchase.  |
| EBT       | Electronic Benefit Transfer is an electronic system that allows state welfare departments to issue benefits via a magnetically encoded payment card. |

<!--
type: tab
-->

See below for a legend of possible return values for the `cardbrand` field. 

| Card Brand | Description         |
|------------|---------------------|
| MC         | MasterCard          |
| VISA       | Visa                |
| DSCV       | Discover            |
| UNKN       | Unknown             |
| AMEX       | American Express    |
| WEX        | Wright Express      |
| VYGR       | Voyager             |
| TEL        | Telecheck           |
| DNR        | Diners              |
| JCB        | Japan Credit Bureau |
| BML        | Bill Me Later       |
| RM         | Revolution Money    |

<!-- type: tab-end -->

#### Currency Code Responses

The CardPointe Gateway adheres to the ISO 4217 standards for currency code identification as set forth by the International Organization for Standards (ISO). Please refer to [http://www.iso.org/iso/currency_codes](http://www.iso.org/iso/currency_codes) for a comprehensive mapping of alphabetic codes.

<!--
type: tab
titles: Transaction Type Responses, Transaction Status Responses
-->

See below for a legend of possible return values for the `type` field. 

| Transaction Type | Description                                        |
|------------------|----------------------------------------------------|
| SALE             | Sale or ticket-only sale.                          |
| REFUND           | Credit back of a sale due to a return or refund.   |
| CASH ADVANCE     | Cash Advance                                       |
| VOID SALE        | Void a sale or ticket-only sale or a cash advance. |
| VOID REFUND      | Void a return or refund.                           |
| AUTH REQUEST     | Authorization request.                             |
| ACCOUNT VERIFY   | Account verification.                              |
| UNKNOWN          | Source data transaction type not mapped.           |

<!--
type: tab
-->

See below for a legend of possible return values for the `status` field. 

| Transaction Status | Description                                                                                                                                                                                                           |
|--------------------|---------------------------------------------------------|
| Auth               | The credit card authorization request was processed successfully.                                                                                                                                                     |
| Captured           | The credit card authorization was captured, and the request was processed successfully by the payment processor.                                                                                                      |
| Voided             | A deletion of the transaction information.                                                                                                                                                                            |
| Failure            | The credit card (authorization, capture, or credit) or check (debit or credit) request failed.                                                                                                                        |
| Rejected           | A rejection of the transaction information due to duplicate transaction or fraud protection.                                                                                                                          |
| Declined           | A response from the card issuer denying the use of the card for the attempted transaction.                                                                                                                            |
| Settled            | The check debit or credit request was processed successfully.                                                                                                                                                         |
| Processed          | The transaction was fully processed. <br> <br> _**Note:** for ACH transactions, the Processed status is displayed for rejected transaction. See the_ `achreturncode` _field for more information on the transaction._ |
| Unknown            | Transaction status not supplied.                                                                                                                                                                                      |

<!-- type: tab-end -->

#### Adjustment Group Responses

See below for a legend of possible returns for the `category` field.

| Adjustment Group    | Adjustment Group Detail           |
|---------------------|-----------------------------------|
| ADJUSTMENTS         | DEPOSIT ADJUSTMENTS               |
| ADJUSTMENTS         | FINANCIAL ADJUSTMENTS             |
| ADJUSTMENTS         | AGENT BANK                        |
| ADJUSTMENTS         | CASH ADVANCE ADJUSTMENT           |
| ADJUSTMENTS         | COST OF FUNDS                     |
| ADJUSTMENTS         | DEPOSIT ADJUSTMENTS               |
| FEES                | FEES                              |
| FEES                | ADDRESS VERIFICATION              |
| FEES                | AUTHORIZATIONS                    |
| FEES                | ACCOUNT MANAGEMENT FEES           |
| FEES                | WS INFO. MGR.                     |
| FEES                | CHARGEBACK TRANSACTION PROCESSING |
| FEES                | DEBIT                             |
| FEES                | DATA CAPTURE                      |
| FEES                | CES IMAGE                         |
| FEES                | EQUIPMENT                         |
| FEES                | COMMUNICATION                     |
| FEES                | CASH ADVANCE                      |
| FEES                | CES LINK                          |
| FEES                | CHECK SERVICES                    |
| FEES                | FUNDS TRANSFER                    |
| FEES                | SUPPLIES                          |
| FEES                | FOREIGN HANDLING                  |
| FEES                | REPORT                            |
| FEES                | CES TRANSLINK                     |
| FEES                | USAVE                             |
| INTERCHANGE CHARGES | INTERCHANGE CHARGES               |
| INTERCHANGE CHARGES | INTERCHANGE                       |
| INTERCHANGE CHARGES | ASSESSMENT                        |
| INTERCHANGE CHARGES | NON-QUAL TRANSACTIONS             |
| INTERCHANGE CHARGES | AGENT INTERCHANGE                 |
| INTERCHANGE CHARGES | INTERCHANGE/ASSESSMENT ADJ.       |
| INTERCHANGE CHARGES | DEBIT                             |
| REVERSAL            | CHARGEBACKS/CHARGEBACK REVERSALS  |
| REVERSAL            | DEBIT ADJUSTMENTS                 |
| REVERSAL            | DEBIT ADJUSTMENT REVERSAL         |
| SERVICE CHARGES     | SERVICE CHARGES                   |
| SERVICE CHARGES     | SERVICE CHARGE ADJUSTMENTS        |
| SERVICE CHARGES     | DEBIT SERVICE CHARGE              |
| THIRD PARTY         | THIRD PARTY ADJUSTMENTS           |
| OTHER ADJUSTMENT    | OTHER ADJUSTMENT                  |
