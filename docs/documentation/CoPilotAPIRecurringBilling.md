# Overview

The CoPilot API Billing Plan endpoints allow you to create and manage recurring billing plans using tokenized and stored customer payment data.

<!-- theme: danger -->
> It is a violation of PCI DSS standards to store Card Verification Value (CVV) data. Neither the CardPointe Gateway nor the merchant can store this data for the purpose of recurring billing.

## How it Works

The following process provides a general overview of the steps required to set up a recurring payment schedule using the CoPilot API.

**1.** Tokenize the customer's payment data and create a profile. Depending on your existing integration, there are several ways to tokenize payment data and create a payment profile:

- Use the customer’s payment card or ACH payment data to make a CardPointe Gateway API authorization request including `"profile" : "y"` to create a stored profile (optionally, include `"capture":"y"` to also capture a payment). The authorization response includes the `profileid` and `acctid` for the new profile.
- Use the [Hosted iFrame Tokenizer](HostediFrameTokenizer.md) to gather and tokenize the payment data, then use the CardPointe Gateway API to create a profile.
- Use a CardPointe Integrated Terminal and the Terminal API authCard or authManual request, including `"createProfile":"true"`, to accept a payment and create a profile.

**2.** Take note of the `profileid` and `acctid` generated during profile creation, as these fields are required to create the billing plan.

<!-- theme: warning -->
> Note that the CardPointe Gateway stores these fields as `profileid` and `acctid`; however the CoPilot Billing Plan endpoint refers to these values as `profileId` and `acctId`.

**3.** Gather your billing requirements. Determine the start date and length of the billing plan, the payment amount and frequency, and any additional information that you'll need to include in your requests.

**4.** Call the [Create Billing Plan endpoint](#Create-Billing-Plan) to create the billing plan. Specify the `profileID` and `acctId` Billing Plan parameters as described in the [Billing Plan Definition](#Object-Definitions).

# Using the Billing Plan API

The Billing Plan API supports the following features:

- Create a new billing plan
- Get a list of billing plans
- Get detailed information on a specific billing plan
- Update a payment account in a billing plan
- Update a payment status
- Cancel a payment
- Cancel a billing plan

The following topics describe the Billing Plan API.

> See the CoPilot API documentation for general information on using the CoPilot API.

## Create Billing Plan

Use the Create Billing Plan endpoint to set up recurring payments using a payment profile created via the CardPointe Gateway API, or using a stored customer profile saved via the Customers page of the CardPointe web interface.

|  |  |
| --- | --- |
| **Method** | POST |
| **Host** | https://api-uat.cardconnect.com
| **Path** | /billingplan/create
| **Headers**	| Authorization: Bearer <br> X-CopilotAPI-Version: 1.0 <br> Content-Type: application/json
| **Consumes**	| application/json
| **Produces**	| application/json

### Create Billing Plan Request Body

| Field | Size | Type | Required | Comments | 
| --- | --- | --- | --- | ---
| billingPlan	| n/a	| object | yes | The [billing plan object](#Object-Definitions). <br> <br> **Note**: _Leave the_ `billingPlanId` _field blank or omit it when creating a new billing plan. The_ `billingPlanId` _is automatically assigned when the plan is created._ |

### Create Billing Plan Response

| Field | Type | Comments | 
| --- | --- | ---
| billingPlan	| object | The [billing plan object](#Object-Definitions).
| errors | array | The errors array containing one or more errors if there was a problem creating the billing plan.

## Get Billing Plan List

Retrieve a list of billing plans already configured for a specific front end MID.

|  |  |
| --- | --- |
| **Method**	| GET |
| **Host** |	https://api-uat.cardconnect.com
| **Path** | /billingplan/list/{merchId}
| **Headers**	| Authorization: Bearer <br> X-CopilotAPI-Version: 1.0 <br> Content-Type: application/json
| **Consumes** | n/a
| **Produces** | application/json

### Get Billing Plan List Request

```
GET https://api-uat.cardconnect.com/billingplan/list/111111111
```

### Get Billing Plan List Response

| Field	| Type | Comments
| --- | --- | ---
| billingPlans | array | An array containing a [billing plan object](#Object-Definitions) for each billing plan configured for the MID. <br> <br> **Note**: _The_ `schedules` _field of the billing plan objects returned in the response is always null. To view the billing schedule for a specific plan, use the [Get Billing Plan](#Get-Billing-Plan) endpoint._ |
| errors | array | The errors array containing one or more errors if there was a problem retrieving the list of billing plans.

## Get Billing Plan

Use the Get Billing Plan endpoint to retrieve the details of a specific billing plan for a front end MID.

|  |  | 
| --- | --- 
| **Method** | GET
| **Host** | https://api-uat.cardconnect.com
| **Path** | /billingplan/{merchId}/{billingPlanId}
| **Headers**	| Authorization: Bearer <br> X-CopilotAPI-Version: 1.0 <br> Content-Type: application/json
| **Consumes** | n/a
| **Produces** | application/json

### Get Billing Plan Request

```
GET https://api-uat.cardconnect.com/billingplan/111111111/123456
```

### Get Billing Plan Response

| Field	| Type | Comments
| --- | --- | ---
| billingPlan	| object | The [billing plan object](#Object-Definitions).
| errors | array | The errors array containing one or more errors if there was a problem retrieving billing plan.

## Update Account

Stored customer profiles or payment profiles created via the CardPointe Gateway API can contain more than one payment method, referred to as a payment account. Use the Update Account endpoint to modify the payment account used for a billing plan.

|  |  |
| --- | ---
| Method	| POST
| Host	| https://api-uat.cardconnect.com
| Path | /billingplan/updateAccount
| Headers	| Authorization: Bearer <br> X-CopilotAPI-Version: 1.0 <br> Content-Type: application/json
| Consumes | application/json
| Produces | application/json

#### Example Account Update Request

```json
{
    "merchId": "434315170887",
    "billingPlanId": "11992",
    "profileId": "11416055053282854657",
    "acctId": "2"
}
```

### Update Account Request Body

| Field	| Size | Type	| Required | Comments
| --- | --- | --- | --- | ---
| merchId	| 11 | string	| yes	| The front end MID associated with the billing plan.
| billingPlanId	| 11 | string	| yes	| The ID of the billing plan to update.
| profileId	| 20 | string	| yes	| The CardPointe Gateway profile ID to use for future billing.
| acctId | 3 | string	| no | The account ID of the card within the CardPointe Gateway profile to use for future billing. <br> <br> **Note**: _When omitted, the default account is used._ |

### Update Account Response

| Field	| Type | Comments
| --- | --- | ---
| billingPlan	| object | The [billing plan object](#Object-Definitions).
| errors | array | The errors array containing one or more errors if there was a problem updating the account used for the billing plan.

## Mark as Paid

Use the Mark as Paid endpoint to set the status of a scheduled payment as PAID.

|  |  |
| --- | ---
| **Method** | POST
| **Host** | https://api-uat.cardconnect.com
| **Path** | /billingplan/markAsPaid
| **Headers**	| Authorization: Bearer <br> X-CopilotAPI-Version: 1.0 <br> Content-Type: application/json
| **Consumes** | application/json
| **Produces** | application/json

#### Example Mark as Paid Request

```json
{
    "billingPlanScheduleId": "1234567",
    "retref": "R123456789012"
}
```

### Mark as Paid Request Body

| Field | Size | Type | Required | Comments | 
| --- | --- | --- | --- | --- |
| billingPlanScheduleId | 11 | string	| yes	| The ID of the scheduled payment to update.
| retref | 13	| string | no	| The retref (or refnum) of a transaction to associate with the payment. This can be left as null if unavailable or in the case of a cash payment. If required, multiple payments may be associated to the same transaction by making multiple calls to this endpoint.

### Mark as Paid Response

| Field	| Type | Comments
| --- | --- | ---
| billingPlan	| object | [The billing plan object](#Object-Definitions).
| errors | array | The errors array containing one or more errors if there was a problem marking the scheduled payment as paid.

## Cancel Payment

Use the Cancel Payment endpoint to cancel a scheduled payment within a billing plan. You can use the [Get Billing Plan](#Get-Billing-Plan) endpoint to view all payment schedules within the plan, the associated `billingPlanScheduleId`, and confirm that the payment you intend to cancel has a `paymentStatus` of 'Scheduled.'

|  |  |
| --- | ---
| Method	| POST
| Host	| https://api-uat.cardconnect.com
| Path | /billingplan/cancelPayment
| Headers	| Authorization: Bearer <br> X-CopilotAPI-Version: 1.0 <br> Content-Type: application/json
| Consumes	| application/json
| Produces	| application/json

#### Example Cancel Payment Request

```json
{
    "billingPlanScheduleId": "1234567"
}
```

### Cancel Payment Request Body

| Field	| Size | Type	| Required | Comments
| --- | --- | --- | --- | ---
| billingPlanScheduleId	| 11 | string	| yes	| The ID of the scheduled payment to cancel.

### Cancel Payment Response

| Field	| Type | Comments
| --- | --- | ---
| billingPlan	| object | The [billing plan object](#Object-Definitions).
| errors | array | The errors array containing one or more errors if there was a problem cancelling the scheduled payment.

## Cancel Billing Plan

Use the Cancel Billing Plan endpoint to end a billing plan and cancel all the remaining payments scheduled.

|  |  |
| --- | ---
| **Method**	| POST
| **Host**	| https://api-uat.cardconnect.com
| **Path** | /billingplan/cancel
| **Headers**	| Authorization: Bearer <br> X-CopilotAPI-Version: 1.0 <br> Content-Type: application/json
| **Consumes** | application/json
| **Produces** | application/json

#### Example Cancel Billing Plan Request

```json
{
    "merchId": "123456789012",
    "billingPlanId" : "12345"
}
```

### Cancel Billing Plan Request Body

| Field	| Size | Type	| Required | Comments
| --- | --- | --- | --- | ---
| merchId	| 11 | string	| yes	| The front end MID associated with the billing plan.
| billingPlanId	| 11	| string	| yes	| The ID of the billing plan to cancel.

### Cancel Billing Plan Response

| Field	| Type | Comments
| --- | --- | ---
| billingPlan	| object | The [billing plan object](#Object-Definitions).
| errors | array | The errors array containing one or more errors if there was a problem cancelling the billing plan.

## Object Definitions

### Biling Plan Object

| Field	| Size | Type	| Required | Comments
| --- | --- | --- | --- | ---
| billingPlanId	| 11 | string | no | The unique identifier for the billing plan, generated at the time of creation. Required to update or cancel a billing plan. <br> <br> **Note**: _Leave this field blank or omit it when creating a new billing plan. The billingPlanId is automatically assigned when the plan is created._ |
| merchId	| 11 | string	| yes	| The front end MID associated with the billing plan.
| profileGuid	| n/a	| string | no	| The GUID representing the account within the CardPointe Gateway profile to charge for this order. <br> <br> **Note**: _Leave this field blank or omit it when creating a new billing plan. The profileGuid is automatically populated when the plan is created._ |
| profileId	| 20 | string	| yes	| The CardPointe Gateway profile ID to charge for this order.
| acctId | 3 | string	| no | The account ID of the card within the CardPointe Gateway profile to charge for this order. When not provided, the default account is used. This field is always returned when retrieving a billing plan.
| amount |  | number | yes | The amount to bill for each scheduled payment.
| timeSpan | 1 | number	| yes	| The billing frequency, represented as 1 for daily, 2 for weekly, 3 for monthly, or 4 for yearly.
| every	| 1	| number | yes | The billing interval, where 1 represents every period specified in the timeSpan field, and 2 represents every other period specified in the `timeSpan` field.
| untilCondition | 1 | string	| yes	| The condition that ends the billing plan, where C is until cancelled, N is until a number of payments are made, or D to end the billing plan on a specific date.
| untilNumPayments | | number	| See Comments | The number of payments required to automatically end the billing plan. Required when `untilCondition` = N.
| untilDate	| 8	| string | See Comments	| The date the billing plan ends, formatted as `MMDDYYYY`. Required when `untilCondition` = D.
| currencySymbol | 1 | string | yes	| The currency symbol for the transaction.
| startDate	| 8	| string | yes | The date the billing plan begins, formatted as `MMDDYYYY`.
| billingPlanName	| 30 | string	| yes	| The name used to distinguish the billing plan from other billing plans.
| options	| | array	| yes	| The email_receipt option should be provided with a value of 0 for no or 1 for yes. See example JSON.
| planStatus | 1 | string	| no | The billing plan status, one of the following values: <br> <br> A = Active <br> C = Canceled <br> F = Finished <br> <br> This cannot be set, and is only returned when fetching a billing plan.
| schedules | | array	| no | An array of [billing plan schedules](#billing-plan-schedule-definition) for each of the payments in the billing plan. This cannot be set, and is only returned when fetching a billing plan. This array will always contain all past payments. In addition, it will also contain future scheduled payments (up to 1000 when untilCondition = N or D, or one year of payments when untilCondition = C) <br> <br> **Note**: _This field is always null for billing plan objects returned in the [Get Billing Plan List](#Get-Billing-Plan-List) response. To view the billing schedule for a specific plan, use the [Get Billing Plan](#Get-Billing-Plan) endpoint._

### Example Billing Plan Object

#### Example Billing Plan Object

```json
{
    "billingPlanId": "11382",
    "merchId": "434315170887",
    "profileId": "12345678901234567890",
    "profileGuid": "cnhwefcq3t497fbxqg37r8439r7xt34",
    "acctId" : "1",
    "amount": 12.99,
    "timeSpan": 2,
    "every": 1,
    "untilCondition": "C",
    "untilNumPayments": null,
    "untilDate": null,
    "currencySymbol": "$",
    "startDate": "03082021",
    "billingPlanName": "api test plan",
    "options": [
        {
            "name": "email_receipt",
            "value": "0"
        }
    ],
    "planStatus": "A",
    "schedules" : [ ... ]
}
```

### Billing Plan Schedule Definition

| Field	| Type | Required	| Comments
| --- | --- | --- | ---
| billingPlanScheduleId	| number | yes | The id of the billing plan schedule entry.
| actualAmount | number	| no | The amount that was actually billed, if the payment was processed.
| actualPaymentDate	| string	| no	| The date this payment was made, if it was processed.
| paymentStatus	| string	| yes	| The payment status, such as Scheduled, Cancelled, Paid, or Failed.
| retref	| string	| no	| The transaction id, if the payment was processed.
| scheduledAmount	| number	| yes	| The amount scheduled to be billed.
| scheduledPaymentDate	| string	| yes	| The date scheduled for this payment.

### Example Billing Plan Schedule Object

#### Example Billing Plan Schedule Onkect

```json
{
    "billingPlanScheduleId": 1234567,
    "actualAmount": null,
    "actualPaymentDate": null,
    "paymentStatus": "Scheduled",
    "retref": null,
    "scheduledAmount": 12.99,
    "scheduledPaymentDate": "03/08/2021"
}
```
