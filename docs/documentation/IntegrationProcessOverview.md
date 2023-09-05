# Overview

Our Integration Solutions Team is here to ensure that your payment integration process is guided, efficient, and ultimately successful.

This guide provides an overview of the typical integration process to help you get started.

## Integration Requirements

Before you begin, note the following requirements:

- You must complete a Merchant Application Agreement and obtain a CardPointe merchant account.
- If accepting card-present transactions, you must use devices pre-provisioned and provided for use with your merchant account.
- You must complete integration certification in a testing environment before processing live transactions.

# Preparing for Integration

- Our Integration Solutions Team will consult with you to collect information on your business type, size, payment processing needs, special considerations, and other relevant details in order to recommend the best combination of integrated solutions to fit your business needs.
- A Merchant Processing Application will be submitted to create your merchant account and Merchant ID for payment processing. 
- If your business will perform card-present transactions, you'll be sent a terminal device for integration development. For e-commerce or card-not-present transactions, you can begin development almost immediately.
- The Integration Solutions Team will provide additional documentation and guidance for your specific use case.

<!-- theme: warning -->
> To get started with a CardPointe Gateway API integration, see [Testing Your Integration](../../docs/documentation/CardPointeGatewayDeveloperGuides.md#Testing-Your-Integration) for test credentials and details on testing in the UAT environment.
> 
> Additionally, check out the following guides for more helpful information:

<!-- type: row -->

<!-- type: card
title: API Basics and Best Practices
description: Provides helpful information and important best practices to help you get the most of your integration
link: ?path=docs/documentation/APIBasicsAndBestPractices.md
-->

<!-- type: card
title: API Connectivity Guide
description: Provides an overview of our APIs and services and how your integrated solution connects to them
link: ?path=docs/documentation/APIConnectivityGuide.md
-->

<!-- type: row-end -->

# Developing Your Integration

- If you are integrating a payment terminal, establish a session or connection with the device from your POS software, or API testing software such as Postman.
- Tokenize a payment card or complete a transaction in the test environment.
- Continue to develop your application in the test environment, accounting for all payment processing scenarios relevant to your business and application.

<!-- theme: warning -->
> Review the [Developer Guides](../../docs/getting-started.md) for additional information and resources to help you complete your integration.

# Validating Your Integration

- Once development is complete, perform the required test transactions for your integration and record the transaction information in the supplied Certification Guide.
- Submit the Certification Guide with all required test transactions for review.
- The Integration Solutions Team will review your submitted test transactions, and provide guidance to resolve any issues and pass validation.
- When all required test transactions are validated, your certification is complete, and the Integration Solutions Team will provide production credentials.

# Deploying to the Production Environment

- Replace the connection details and credentials used for the testing environment with that of the live environment.
- Tokenize a payment card or complete a transaction in the live environment.
- Order additional devices as necessary, or begin a new integration with a different payment solution.
