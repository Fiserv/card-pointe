# Overview

The CardPointe Hosted Payment Page (HPP) is a secure and customizable payment form that can be deployed quickly with little to no development work. As an alternative to redirecting your customers to a standalone checkout page, the HPP allows you to embed the PCI-compliant form in an iFrame on your website. Integrating the HPP with your website allows you to easily accept payments and can reduce your scope of PCI compliance by ensuring that sensitive payment data is never shared with your application.

This guide provides information for integrating the HPP's secure payment features with your web application.

## Features and Benefits

By integrating the HPP with your web application, you can:

- Securely accept credit card and e-check (ACH) payments
- Reduce your scope of PCI compliance
- Authorize and capture payments, or authorize-only and capture later. 
- Customize the look and feel to match your branding, including confirmation pages and receipts.
- Add custom and user-definable fields.
- Establish recurring billing plans and secure, stored customer profiles.
- Use multiple merchant IDs (MIDs) on one hosted plan (helpful for businesses with multiple cost centers).
- Display selectable line items.
- Configure webhooks to automatically send transaction details to your accounting or reporting application.
- Manage advanced settings such as:
    - Maximum number of declined payments
    - Minimum and maximum payment amounts

# Getting Started

Setting up the HPP is quick and easy. Contact integrationdelivery@cardconnect.com to set up your account. 

Once you have Super Admin access to your account, you can configure and customize your HPP to meet your needs. See the CardPointe HPP User Guide for detailed information on setting up your HPP.

## Testing your HPP

To design and test your HPP, you use a sandbox HPP account which is set to **Test Mode (Sandbox)** and configured with a UAT CardPointe merchant ID and credentials. In addition to the initial setup, this allows you to redesign and test your changes on your sandbox HPP without disrupting your live HPP.

## Prefilling Payment Form Fields

When you build your payment form on the HPP Connect Tab, you can prefill the **Amount**, **Invoice #**, and **Customer ID** fields, which are then inserted into the payment form URL as field=value pairs.  

If you want to prefill additional details, you can manually generate or modify HPP URLs to include values for most fields. For example, this can be helpful if your webpage or application already includes a form to gather shipping and billing information, to prevent customers from having to enter their information again on the HPP.

The following example illustrates an HPP URL with the payment amount, invoice ID, customer ID, and customer name  prefilled:

```
https://cphppdemo.securepayments.cardpointe.com/pay?total=10.00&invoice=testinvoice123&customerId=JDoe123&billFName=John&billLName=Doe
```

You can include the following fields in the URL, to prefill values on the HPP:

> - Field names are case-sensitive.
> - Only the total (##INVOICE_AMOUNT##) can be prefilled on the Mini version of the HPP form.

| URL Field Name | HPP Design Field Name | Notes | 
| --- | --- | --- |
| total | ##INVOICE_AMOUNT## | The total amount of the requested payment. |
| invoice | ##INVOICE_NUMBER## | The Invoice Number. <br> <br> The Invoice Number is sent to the CardPointe Gateway as the `orderid` for the transaction, allowing you to look up and manage transactions by `orderid`, using the CardPointe Gateway API. See [Using the CardPointe Gateway API to Manage Transactions](#using-the-cardpointe-gateway-api-to-manage-transactions), below, for more information. |
| customerId | ##CUSTOMER_NUMBER## | The customer's unique customer ID, if applicable.
| billCompany | ##COMPANY##	| The customer's company name, if applicable.
| billFName	| ##FIRST_NAME## | The customer's first name.
| billLName	| ##LAST_NAME##	| The customer's last name.
| billAddress1	| ##ADDRESS##	| The first line of the customer's billing street address.
| billAddress2 |  | The second line of the customer's billing street address, if applicable.
| billCity	| ##CITY##	| The customer's billing address city.
| billZip	| ##ZIP##	| The customer's billing address zip code.
| billState | ##STATE##	| The customer's billing address state.
| billCountry	| ##COUNTRY##	| The 2-character country code for the customer's billing address country.
| email	| ##EMAIL##	| The customer's email address.
| phone	| ##PHONE##	| The customer's phone number.
| cf_<custom field name> | ##CUSTOM_FIELDS## | Custom fields, configured for your page. Include as many cf_<custom field name> values as necessary, where <custom field name> is the name of the field configured on the HPP Design tab. |
| location |  | The specific MID/location when the HPP includes multiple locations.

# Deploying your HPP in an iFrame

Once you have completed the initial configuration, you can generate the URL and iFrame code snippet to embed the HPP in your webpage. 

To generate the URL, do the following:

**1.** Click the **Connect** tab, then select the **Build Your Pay Components** page.

**2.** Click Build your links/button.

**3.** Under **Form types**, select the **Mini** or the **Full** form type, depending on your needs. 

**4.** Click **Copy URL to clipboard** to copy the HPP URL iFrame code snippet.

**5.** Paste the iFrame into your HTML to embed the payment form.

# Retrieving Transaction Data

The HPP offers several methods for viewing and managing transactions. 

Depending on your needs, you can

- use the HPP Merchant Reports page or the CardPointe web app Transactions tab to review and manage transactions.
- use webhooks to send transaction data from the HPP to your application.
- use the CardPointe Gateway API to retrieve reporting data and manage transactions.

## Using HPP Webhooks to Retrieve Transactions

If you use a standalone reporting or accounting application, you can use the HPP's webhook feature to automatically send transaction details to your application. 

When a payment is successfully processed, the HPP sends the authorization response returned by the CardPointe Gateway to the URL specified in the webhook configuration in an HTTP POST request. The transaction details are sent as a URL-encoded JSON string, which your application can parse to obtain the required data.

The data includes the `retref` for each transaction, which your application can use to void or refund transactions using the CardPointe Gateway API.

To configure a webhook, do the following:

**1.** On the HPP Admin page, select the **Connect** tab.

**2.** On the Connect tab, select **Notifications** in the left pane.

**3.** On the Auto Notifications page, in the **Webhook URL** field, enter the URL used to receive transaction details.

**4.** Click **Save notification settings**. 

Additionally, you can click **Example** to view an example HPP Webhook Response:

#### Example HPP Webhook Response (JSON String)

```json
{"merchantId":"496160873888","URL":"cphostedpayment.securepayments.cardpointe.com","paymentType":"CC","type":"Pay","total":"54.80","invoice":"45678965699-1236","customerId":"234567:BERGER-IND","details":[{"sku":"ABC123","qty":"1","item":"My first Item","price":"12.15"},{"sku":"DEF456","qty":"4","item":"Blue Widget (green)","price":"8.00"},{"sku":"SHIPPING","qty":"1","item":"UPS Ground","price":"6.00"},{"sku":"TAX","qty":"1","item":"Sales Tax","price":"4.65"}],"ipAddress":"50.203.208.241","date":"2017-09-29 09:23:28","cardType":"Visa","number":"**** 0124","expirationDateMonth":"02","expirationDateYear":"2019","billCompany":"Berger Industries","billFName":"Michael","billLName":"Berger","billAddress1":"4353 James Avenue","billAddress2":"Ste 100","billCity":"Wichita","billState":"KS","billZip":"67202","billCountry":"US","email":"email@domain.com","phone":"407-555-1212","cf_Customfield1":"field1","cf_Customtextarea2":"field2","cf_Customfield3":"field3","cf_Customfield4":"field4","cf_Customfield_5":"field_5","cf_Customfield6":"field6","cf_Customfield7":"field7","cf_Customfield8":"field8","cf_Customfield9":"field9","gatewayTransactionId":"074574351662","avsResponseCode":"Y","cvvResponseCode":"M","responseText":"Approval","responseCode":"00","authCode":"337568","responseProc":"FNOR","token":"9466536339578351"}
```

#### Example HPP Webhook Response (Parsed)

```json
{
    "merchantId":"496160873888",
    "URL":"cphostedpayment.securepayments.cardpointe.com",
    "paymentType":"CC",
    "type":"Pay",
    "total":"54.80",
    "invoice":"45678965699-1236",
    "customerId":"234567:BERGER-IND",
    "details":[
       {
          "sku":"ABC123","qty":"1",
          "item":"My first Item",
          "price":"12.15"
       },
       {
          "sku":"DEF456",
          "qty":"4",
          "item":"Blue Widget (green)",
          "price":"8.00"
       },
       {
          "sku":"SHIPPING",
           "qty":"1",
           "item":"UPS Ground",
           "price":"6.00"
       },
       {
          "sku":"TAX",
          "qty":"1",
          "item":"Sales Tax",
          "price":"4.65"
       }],
    "ipAddress":"50.203.208.241",
    "date":"2017-09-29 09:23:28",
    "cardType":"Visa",
    "number":"**** 0124",
    "expirationDateMonth":"02",
    "expirationDateYear":"2019",
    "billCompany":"Berger Industries",
    "billFName":"Michael",
    "billLName":"Berger",
    "billAddress1":"4353 James Avenue",
    "billAddress2":"Ste 100",
    "billCity":"Wichita",
    "billState":"KS",
    "billZip":"67202",
    "billCountry":"US",
    "email":"email@domain.com",
    "phone":"407-555-1212",
    "cf_Customfield1":"field1",
    "cf_Customtextarea2":"field2",
    "cf_Customfield3":"field3",
    "cf_Customfield4":"field4",
    "cf_Customfield_5":"field_5",
    "cf_Customfield6":"field6",
    "cf_Customfield7":"field7",
    "cf_Customfield8":"field8",
    "cf_Customfield9":"field9",
    "gatewayTransactionId":"074574351662",
    "avsResponseCode":"Y",
    "cvvResponseCode":"M",
    "responseText":"Approval",
    "responseCode":"00",
    "authCode":"337568",
    "responseProc":"FNOR",
    "token":"9466536339578351"
}
```

## Managing Transactions from the HPP or CardPointe Web App 

The HPP includes basic reporting and management features that allow you to view the status of your HPP transactions, as well as to void or refund a payment, or to manage your customer subscriptions. On the HPP, click the **Payments** tab to access the reporting page. See the CardPointe HPP User's Guide for more information.

Additionally, if you use the CardPointe Web application, you can view and manage your HPP payments on the Reporting page along with all other transactions for your merchant account. To view only your HPP payments, click the **Front End** filter and select **Hosted Payment Page**. The Reporting table will refresh and display only the transactions processed by your HPP. See the CardPointe User's Guide for more information.

## Using the CardPointe Gateway API to Manage Transactions 

The CardPointe Gateway API offers the most flexible methods for reconciling and managing transactions. 

If you are [using webhooks](#using-hpp-webhooks-to-retrieve-transactions) to gather transaction details, you can use the `retref` for a transaction to make inquire, refund, or void requests.

If you are not using webhooks, you can include an Invoice Number in each transaction. The Invoice Number value is sent to the CardPointe Gateway as the `orderid`, which you can use to make an inquireByOrderid request to retrieve the `retref` and other transaction details to allow you to manage your payments.

See the CardPointe Gateway API for detailed information.
