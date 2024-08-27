# CoPilot API Changelog

The following entries describe changes to the [CoPilot API](?path=docs/APIs/CoPilotAPI.md) and documentation.

## Date Updated: 4/16/2024 

An update to the CoPilot API has been released to the Production environment on 4/16/2024. This release includes the following update in addition to internal fixes and enhancements:

### New Delivery Percentages Field

The **Delivery Percentages** object now includes a **dlvrySameDayPct** field to specify the percentage of orders that the merchant fulfills same-day, represented as a whole number.

The following example illustrates a possible entry including the new field:

```
"deliveryPercentages": {
    "dlvrySameDayPct": 96,
    "dlvry0To7DaysPct": 1,
    "dlvry8To14DaysPct": 1,
    "dlvry15To30DaysPct": 1,
    "dlvryOver30DaysPct": 1
}
```

The addition of this field aligns with the same-day delivery option being made available to newly created accounts in CoPilot and the Digital Application. For more information on this, please refer to the CardPointe Support Site. 

## Date Updated: 11/8/2023 

An update to the CoPilot API has been released to the Production environment on 11/8/2023. This release includes the following updates in addition to internal fixes and enhancements:

### Enhanced Field Validation 

Additional validations have been incorporated for the `legalBusinessName`, `akaBusinessName`, `taxFilingName`, and `dbaName` fields to check for string sizes that exceed the expected maximum length. The following example illustrates two possible errors:

```
{
    "errors": [
        {
            "code": "1105",
            "message": "Legal Business Name has invalid length.",
            "errorfield": "merchant.legalBusinessName",
            "status": "BAD_REQUEST"
        },
        {
            "code": "1178",
            "message": "AKA Business Name has invalid length.",
            "errorfield": "merchant.akaBusinessName",
            "status": "BAD_REQUEST"
        }
    ]
}
```

### Shipping Bill To Field 

Previously, the Shipping Bill To field was not pulled from the template. However, it is now automatically provided from the active template. There is no need to utilize the `shippingBillToCd` field in the API, and if it is used, it will be disregarded.

### Non-Swiped Auth Fee 

`cloverNonSwipedAuthFee` has been added to the fee class. This fee is only applicable for North Retail ISO merchants. 


## Date Updated: 1/19/2022 

This release contains the following updates:

### Third Party Provider Details 

The Merchant Definition now includes an additional `thirdPartyProviderDetails` object, to specify third party provider (TPP) details in a merchant create or update request. This object is also returned in a GET merchant details response. 

This change aligns with a previous update to the MPA and CoPilot to ask if the merchant uses any third party provider (TPP) to store, process, or transmit cardholder data, and if so, to provide the TPP Name and contact phone and email.

The thirdPartyProviderDetails object is optional, but requires the following fields when provided:

| Field	| Type | Description |
| --- | --- | --- |
| thirdPartyProviderFlg	| Boolean | A true/false indicator the determines whether or not the merchant uses a third party provider to store, process, or transmit cardholder data. <br> <br> If **true**, `thirdPartyProviderCd`, `thirdPartyProviderEmail`, and `thirdPartyProviderPhone` are required. <br> <br> If **false**, all other fields can be `null`.
| thirdPartyProviderCd | String | A 2-character code representing the TPP. <br> <br> If `thirdPartyProviderFlg` is **true**, this field is required and must be one of the following values: <br> <br> 01 - Yahoo <br> 02 - Authorize.net <br> 03 - Cybersource <br> 04 - VeriFone <br> 05 - Merchant Link <br> 06 - Shift 4 <br> 07 - Apriva <br> 08 - FIS <br> 09 - Six Payment Services Corp <br> 10 - Verisign <br> 99 - Other <br> <br> If **99**, the `thirdPartyProviderOtherName` field is **required**.
| thirdPartyProviderOtherName | String | The TPP name if `thirdPartyProviderCd` is **99**. <br> <br> Must be null or omitted if `thirdPartyProviderCd` is any other value. <br> <br> This field must not contain the a name from the `thirdPartyProviderCd` list (for example, "Yahoo").
| thirdPartyProviderEmail	| String | The contact email address for the TPP. <br> <br> Must be a less than or equal to 32 characters.
| thirdPartyProviderPhone	| String | The contact phone number for the TPP. <br> <br> Must be 7-12 characters.

## Date Updated: 11/17/2021 

This release contains the following updates:

### Clover Platform Fee 

The Fees definition now includes a Clover Platform Fee, `cloverPlatformFee`, which is **required** to submit merchant applications that include Clover orders.

### Billing Plan Updates 

This release includes the following updates to billing plan requests:

- Billing Plan Definition

    The `profileGuid` field has been added.

- Billing Plan Schedule Definition

    The `billingPlanId` field is now required in the request.

- Mark as Paid Request

    The `billingPlanId` and `merchId` fields are now required in the request.

- Cancel Payment Request

    The `merchId` field is now required in the request.

### New Sponsor Bank Code 

The Hierarchy definition `sponsorBankCd` enumeration now includes `PNC` to support the migration from BBVA to PNC Bank sponsorship.

## Date Updated: 9/1/2021 

This release contains the following updates:

### PCI Program Selection 

The Fees definition now includes a **required** PCI Program Code field, `pciProgramCd`, to identify the merchant's PCI program enrollment. 

Additionally, the Fees definition includes a new `pciConciergeMonthlyFee`, required to submit merchant applications when the `pciProgramCd` is `CONCIERGE`.

## Date Updated: 6/23/2020 

This release contains the following updates:

### New Clover Security Bundle Code 

The Clover Security Bundle Codes now include a new `TDPPCI` code for the **TransArmor Data Protection + PCI Compliance Tool** bundle.

### New Attachment Type Code 

The Attachment Type Codes now include a new `ACKACCEPTAGRMT` code for the **Acknowledgement of Acceptance of Agreement**.

### New Cancel Billing Plan Payment Endpoint 

A new Cancel Billing Plan Payment endpoint is now available for cancelling a single scheduled payment within a billing plan.

## Date Updated: 5/5/2020 

This release contains the following updates:

### New Interchange Type Field 

The IC Plus Pricing schema includes a new optional `interchangeTypeCode` field to set the Passthrough Interchange Costs.

When `interchangeTypeCode` is not included in the request, the default interchange type remains **GROSS** interchange.

### New Clover Security and TransArmor Fields 

The Merchant schema now includes a new cloverSecurityAndTransarmor object, to add a Clover Security bundle or enable TransArmor Data Protection for Clover equipment.

> TansArmor Data Protection and Clover Security bundles are not applicable to Bolt on Clover terminals.

The Fees schema now includes new `cloverSecurityFee` and `pciAnnualFeeMonthToBill` fields.

### New Corporate Level and Chain Level Fields 

The Merchant schema now includes a new Hierarchy object, containing optional fields to set `corpLevel` and `chainLevel` for merchants boarding to First Data North.

### New Publicly Traded Flag and Stock Symbol Fields 

The Ownership Schema now includes a new `publiclyTradedFlag` field and `stockSymbol` field for boarding merchants with the `PUBCORP` Ownership Type Code to the First Data North or Omaha platforms.

### New Attachment Type Codes 

The Attachment Type Codes include a new `SIGNEDCONFIRMATION` attachment type.

Use the new `SIGNEDCONFIRMATION` code when uploading a physical copy of the Program Guide signed by the merchant.

## Date Updated: 10/8/2019 

This release includes the following update:

### BlueChex Volume Fields 

The CoPilot API includes a new `blueChexVolume` object in the Processing schema.

The `blueChexVolume` details are required for merchants using the BlueChex add-on.

This object includes the following fields:

| Field                         | Size | Type    | Description                            |
|-------------------------------|------|---------|----------------------------------------|
| avgPerTransactionSalesAmount  | 18.2 | Decimal | Average sales amount per BlueChex transaction.                        |
| maxPerTransactionSalesAmount  | 18.2 | Decimal | Maximum sales amount per BlueChex transaction.                        |
| avgPerTransactionCreditAmount | 18.2 | Decimal | Average credit (refund) amount per BlueChex transaction.              |
| maxPerTransactionCreditAmount | 18.2 | Decimal | Maximum credit (refund) amount per BlueChex transaction.              |
| avgPerMonthSalesAmount        | 18.2 | Decimal | Average monthly sales amount for all BlueChex transactions.           |
| maxPerMonthSalesAmount        | 18.2 | Decimal | Maximum monthly sales amount for all BlueChex transactions.           |
| avgPerMonthCreditAmount       | 18.2 | Decimal | Average monthly credit (refund) amount for all BlueChex transactions. |
| maxPerMonthCreditAmount       | 18.2 | Decimal | Maximum monthly credit (refund) amount for all BlueChex transactions. |

For example:

```
"processing": {
     "blueChexVolume" : {
          "avgPerTransactionSalesAmount" : "250.00",
          "maxPerTransactionSalesAmount" : "9999.00",
          "avgPerTransactionCreditAmount" : "200.00",
          "maxPerTransactionCreditAmount" : "9999.00",
          "avgPerMonthSalesAmount" : "25000.00",
          "maxPerMonthSalesAmount" : "50000.00",
          "avgPerMonthCreditAmount" : "5000.00",
          "maxPerMonthCreditAmount" : "10000.00"
         }
      }
```
