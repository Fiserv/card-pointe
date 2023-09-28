# Transax to CardPointe

If you are currently integrated to the Transax Gateway, or are in the process of developing a new integration, you should consider an integration to the CardPointe Gateway instead. 

This guide provides information on the CardPointe Integrated Payments solutions and APIs that align with Transax integrations that you might be using today.

## Card-Present Payments

| Transax Integration | CardPointe Integration | Notes
| --- | --- | --- 
| [RoX Web API - Process Transaction](https://documenter.getpostman.com/view/10984668/2s847EREHr#0c060c3d-5b2c-43c8-be5a-d4ba79fc39f4) | [CardPointe Desktop SDK](?path=docs/documentation/CardPointeDesktopSDKDeveloperGuide.md) <br> <br> [CardPointe Integrated Terminal API](?path=docs/APIs/CardPointeIntegratedTerminalAPI.md) | Depending on your needs, consider either the [CardPointe Desktop SDK](?path=docs/documentation/CardPointeDesktopSDKDeveloperGuide.md) and ID Tech Augusta (for example, for in-office payments) or the [CardPointe Integrated Terminal API](?path=docs/APIs/CardPointeIntegratedTerminalAPI.md) and our suite of Ingenico and Clover payment terminals (for example, for a feature-rich retail POS).

## E-Commerce Payments

| Transax Integration | CardPointe Integration | Notes
| --- | --- | --- 
| [RoX Web API - Process Transaction](https://documenter.getpostman.com/view/10984668/2s847EREHr#0c060c3d-5b2c-43c8-be5a-d4ba79fc39f4) | [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) <br> <br> [Hosted iFrame Tokenizer](?path=docs/documentation/HostediFrameTokenizer.md) <br> <br> [CardSecure API](?path=docs/APIs/CardSecureAPI.md) | The [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) offers e-commerce card and electronic check (ACH) payments, reporting, stored profile management and more, offering a single integration for a complete e-commerce payments solution. <br> <br> Use the [Hosted iFrame Tokenizer](?path=docs/documentation/HostediFrameTokenizer.md)  to reduce your scope of PCI compliance by ensuring that sensitive cardholder data is never exposed to your application, or use the [CardSecure API](?path=docs/APIs/CardSecureAPI.md) to build a custom tokenizer that fits your needs.

## Transaction Reporting

| Transax Integration | CardPointe Integration | Notes
| --- | --- | --- 
| [RoX Web API - TransactionReportAPI](https://documenter.getpostman.com/view/10984668/2s847EREHr#a0569b23-fa41-4344-96b6-7a9ba2ce4524) | [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) | The CardPointe Gateway offers APIs to track the transaction lifecycle from authorization and capture through settlement and funding, allowing you to retrieve and manage transactions from within your application. <br> <br> Additionally, the CoPilot and CardPointe web apps offer non-integrated reporting dashboards for your entire business down to individual locations.

## Stored Profiles

| Transax Integration | CardPointe Integration | Notes
| --- | --- | --- 
| [SAFE REST API](https://secure.transaxgateway.com/WebAPI/APIDocs/HTML/SAFEDocsHTML.html) | [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) <br> <br> [CardPointe Card Account Updater API](?path=docs/documentation/CardAccountUpdaterIntegrationGuide.md) | The [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) offers a complete, card-on-file-compliant, solution for creating and managing stored profiles, which can be used across MIDs (under a shared Customer ID). <br> <br> Additionally, the [CardPointe Card Account Updater API](?path=docs/documentation/CardAccountUpdaterIntegrationGuide.md) allows integrators using stored profiles or tokens to keep their stored payment data up to date. |

## Recurring Billing

| Transax Integration | CardPointe Integration | Notes
| --- | --- | --- 
| [Recurring REST API](https://secure.transaxgateway.com/WebAPI/APIDocs/HTML/RecurringDocsHTML.html) | [CardPointe Gateway API](?path=docs/APIs/CardPointeGatewayAPI.md) | The CardPointe Gateway does not currently feature a standalone recurring payments API; however integrators can use stored tokens or profiles and custom scripting (CRON job) to create recurring billing plans within their application. See [Scheduling Recurring Payments](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md#scheduling-recurring-payments) in the CardPointe Gateway Developer Guide for more information. <br> <br> Additionally, the CardPointe web app includes a Billing Plan feature which can be used in lieu of a standalone API solution. See the [CardPointe User's Guide](https://support.cardpointe.com/cardpointe/cardpointe-desktop-app#billing-plans) for more information.

## Merchant Boarding

| Transax Integration | CardPointe Integration | Notes
| --- | --- | --- 
| [Boarding REST API](https://secure.transaxgateway.com/WebAPI/APIDocs/HTML/BoardingDocsHTML.html) | [CoPilot App](https://support.cardpointe.com/copilot) <br> <br> [CoPilot API](?path=docs/APIs/CoPilotAPI.md) | Board merchants to CardPointe using CoPilot. The CoPilot web app provides all of the tools you need to board and manage your merchant portfolio, including a digital merchant application. You can also use the CoPilot API to create a white-labeled merchant application flow in your website or application; however the CoPilot API does not offer the full capabilities of the web application.
