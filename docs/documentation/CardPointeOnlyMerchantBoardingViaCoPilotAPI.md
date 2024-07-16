# Overview

Merchants that process transactions through the CardPointe Gateway while still maintaining their relationship with their acquirer are known as CardPointe-Only accounts.

Boarding for these accounts can be done automatically through CoPilot allowing for minimal manual interaction with the account. 

The following guide illustrates how to allow for merchants to be automatically boarded to the CardPointe Gateway through the API while still maintaining a relationship with their acquirer. 

# Requirements

To allow for merchants to be automatically boarded to the CardPointe Gateway through the API the following requirements must be met.

If these requirements are not met, the account must be manually reviewed by an internal team. 

## API Endpoint Requirements

Account information provided must include the fields that are necessary to run the Token, Merchant, and Submit CoPilot API endpoints. For more information on the exact fields needed, refer to the [API Request Details](#api-request-details) section of this document. 

# Submitting a Merchant Application via CoPilot API

For general information on creating a merchant via the CoPilot API, refer to our documentation concerning Using the [CoPilot API to Submit a Merchant Application]().

# Running the API in Postman

To help you get started with your integration, you can use the sample API Integration Postman Collection, which includes a template of the API service endpoints documented below.

See our documentation on the [Postman Collection for the CoPilot API]() for more information. 

Click the button below to download the collection:

> [CardPointe Only Boarding Postman Collection](https://developer.cardpointe.com/assets/developer/assets/ACH-Payment-Services-NACHA-Operational-Guidelines-Mar-2021.pdf)

# Limitations

## Merchant Service and Account Details

Since these accounts are not acquired, a **Merchant Service** tab in CoPilot does not exist for CardPointe-Only accounts.

These accounts can be seen on the **Account Details** page in CoPilot. Use the **Advance Search** ability on the **Account Details** page and filter by **CardPointe-Only**. 

Changes to these accounts such as enabling/disabling **Amex**, **Visa**, **MC**, and **Discover** flags cannot be changed in CoPilot once the CardPointe-Only account is boarded. These fees will need to be handled wherever the merchant is boarded.

# API Request Details

The following API methods are used to create and submit a CardPointe-Only merchant.

## Token

To authenticate any request to the CoPilot API, you must first request an access token using the Token endpoint. The Token endpoint requires CoPilot user account credentials and returns the access token you will use to make API requests during a session.

The access token is valid for a limited timeframe, indicated by the <code>expires_in</code> value (in seconds). Once a token has expired, you must request a new token.
