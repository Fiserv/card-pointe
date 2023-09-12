# Overview

Our core APIs and services are at the center of all of our payment solutions. This guide provides detailed information to help you understand the architecture of our services, and how your integrated solutions connect to them. 

Specific information, including the site name that you will need, is provided in your welcome kit.

# APIs and Services

Whether you are developing a robust point-of-sale application using CardPointe Integrated Terminals for payment acceptance, or a web application for boarding and managing merchant accounts, your app will connect to one or more of our core services.

Click through the following topics for a brief overview of each service.

<!--
type: tab
titles: CardSecure, CardPointe Gateway, CardPointe Integrated Terminal API, CoPilot
-->

[CardSecure](https://cardconnect.com/cardsecure) is our patented payment card tokenization and encryption service. CardSecure is integrated with the CardPointe Integrated Terminal and CardPointe Gateway payment solutions; however you can also use the CardSecure API or Hosted iFrame Tokenizer to integrate CardSecure directly with your application.

See the CardSecure Support Page for more information on CardSecure.

See the CardSecure API documentation for detailed information on integrating CardSecure using the API.

<!--
type: tab
-->

The CardPointe Gateway is the core of the [CardPointe](https://cardconnect.com/cardpointe) business platform. The CardPointe Gateway API allows you to integrate our complete payment acceptance and transaction management solution with the tools that you use to run your business.

See the CardPointe Gateway API Support Page for more information on the CardPointe Gateway.

See the CardPointe Gateway API documentation for detailed information on integrating the CardPointe Gateway using the API.

<!--
type: tab
-->

The [CardPointe Integrated Terminal](https://cardconnect.com/cardpointe-integrated-payments) solution is the CardPointe platform's secure card-present payments integration for point-of-sale applications.  

See the Support Page for general information and terminal user guides.

See the CardPointe Integrated Terminal API documentation for detailed information on integrating the Terminal API.

<!--
type: tab
-->

CoPilot is our customer and merchant management platform. CoPilot provides our partners with all of the tools they need to manage their business portfolio. In addition to the cloud-based web portal, CoPilot provides an API for developers looking to integrate certain CoPilot capabilities directly with their business platform.

See the CoPilot API documentation for detailed information on integrating the CoPilot API.

<!-- type: tab-end -->

Each API exposes specific connection and authorization methods that your application uses to communicate with the web service. The following topics describe these methods in detail.

# Application Environments

Each service includes two instances, one for testing and validation, and one for production use.

## UAT Environment

You connect to the UAT (user acceptance testing) sandbox environment to test and validate your integration. When you begin your application development and integration, you connect to the UAT instance of our service. 

The UAT environment includes emulators that simulate the payment processing activities that occur in production. In this environment, you test with dummy data that is never sent to the payment processor. You should use test card numbers (for example, 4111 1111 1111 1111 or 4444 3333 2222 1111) and physical test cards. 

<!-- theme: warning -->
> See [Testing With Amount-Driven Response Codes](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md#Testing-with-Amount-Driven-Response-Codes) for additional information on using the emulator to test specific response cases.

CardPointe merchants receive test Merchant ID (MID) accounts for use in the UAT environment. If necessary, you can request a merchant record with a valid MID to test in the UAT environment.

## PROD Environment

You connect to the PROD (production) environment to run your live application. After you successfully test and validate your integration, you will receive a production MID and access to the PROD environment. 

In this environment you use valid merchant and payment data. Test card numbers that work in UAT will decline in this environment. All request traffic is inspected by the CardPointe Gateway and, if valid, is sent to the production host for authorization and settlement. 

For smoke testing, you must direct traffic to a valid MID with production credentials that have been provided by CardPointe Support.

<!-- theme: danger -->
> Tokens generated in one environment (UAT or PROD) can not be used to make authorization requests in the other environment. For example, an authorization request in the PROD environment using a token generated in the UAT environment will fail with the error "invalid token."

# Web Service URLs

Each service has one or more URLs that your application uses to connect and send requests to the service.

The following table provides an overview of the URLs and the components that they are comprised of.

| API | URL Schema |
| --- | ---------- |
| **CardPointe Gateway API** | PROD: `https://<site>.cardconnect.com/cardconnect/rest/<endpoint>` <br /> UAT: `https://<site>-uat.cardconnect.com/cardconnect/rest/<endpoint>` |
| **CardPointe Integrated Terminal API** | PROD: `https://<site>.cardpointe.com/api/<version>/<endpoint>` <br /> UAT: `https://<site>-uat.cardpointe.com/api/<version>/<endpoint>` |
| **CardSecure API (JSON)** | PROD: `https://<site>.cardconnect.com/cardsecure/api/v1/ccn/<endpoint>` <br /> UAT: `https://<site>-uat.cardconnect.com/cardsecure/api/v1/ccn/<endpoint>` |
| **CardSecure API (Legacy)** | PROD: `https://<site>.cardconnect.com/cardsecure/cs?action=<action code>&data=<data string>` <br /> UAT: `https://<site>-uat.cardconnect.com/cardsecure/cs?action=<action code>&data=<data string>` |
| **CoPilot API** | `https://api-uat.cardconnect.com/`

## Sites

In the context of CardPointe products and services, a "site" is a partner-level grouping of one or more merchants and the specific business settings that apply to all merchants under the partner. Some business capabilities and transaction processing settings are configured for the site.

In general, most merchants are boarded to a default site configuration. Depending on the scale and specific business needs of an integration (for example, to allow or disallow specific processing capabilities, or to support a large number of terminal devices), a partner can be granted a unique site for its merchants. 

During the onboarding process, you will be provided the exact site name and assistance with specific configuration details.

> CardSecure tokens are site-specific.
>
> Tokens generated in one site can be used in authorization requests made by any merchant ID within the same site.
>
> Tokens generated in one site are **not** valid for use in an authorization request made to another site. In this case, the response will return an "invalid token" error.

# Authorization Methods

With the exception of CardSecure, each service authorizes requests sent by the client application.

The following table describes the authorization methods used by each service:

| Service | Authorization Method | Description |
| ------- | -------------------- | ----------- |
| **CardPointe Gateway API** | Basic Authorization with username and password	| Your client application is authorized using a username and password. The CardPointe Gateway uses a username and password pair for each merchant ID or for all merchant accounts on a given site, depending on the needs of the integration. <br /> <br /> Base64 encode the "username:password" value and pass it in the Authorization header of all CardPointe Gateway API requests. <br /> <br /> See the Connecting to the Server topic in the CardPointe Gateway API documentation for more information. | 
| **CardPointe Terminal API** | Basic Authorization with secret API key	| Your client application is authorized using a unique API key. This value is valid for use with all CardPointe Integrated Terminal devices associated with a merchant ID. <br /> <br /> Include the API key in the Authorization header of all Terminal API requests. <br /> <br /> See the Connecting a Terminal topic in the CardPointe Integrated Terminal API Developer Guide for more information. |
| **CoPilot API** | Bearer Token Authentication	| Your client application is authorized using a JSON Web Token, which is retrieved in an authentication request to the CoPilot service. A unique client secret value is used to generate the bearer token. <br /> <br /> Include the token in the Authorization field in the header of all CoPilot API requests. <br /> <br /> See the Authentication topic in the CoPilot API documentation for more information. |
