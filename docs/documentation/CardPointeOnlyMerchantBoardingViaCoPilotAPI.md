# Overview

Merchants that process transactions through the CardPointe Gateway while still maintaining their relationship with their acquirer are known as CardPointe-Only accounts.

Boarding for these accounts can be done automatically through CoPilot allowing for minimal manual interaction with the account. 

The following guide illustrates how to allow for merchants to be automatically boarded to the CardPointe Gateway through the API while still maintaining a relationship with their acquirer. 

# Requirements

To allow for merchants to be automatically boarded to the CardPointe Gateway through the API the following requirements must be met.

If these requirements are not met, the account must be manually reviewed by an internal team. 

## API Endpoint Requirements

Account information provided must include the fields that are necessary to run the Token, Merchant, and Submit CoPilot API endpoints. For more information on the exact fields needed, refer to the [API Request Details](#API-Request-Details) section of this document
