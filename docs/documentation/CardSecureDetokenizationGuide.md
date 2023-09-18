# CardSecure Detokenization Guide

CardSecure is our sensitive data encryption and tokenization service. CardSecure allows you to securely accept and tokenize payment account and other sensitive data to ensure the safety of your customers' information. Additionally, CardSecure provides a detokenization method to allow privileged integrators to submit a token to CardSecure to retrieve the unencrypted data from CardSecure's vault.

This guide provides supplemental information for integrators who need to incorporate detokenization requests into their workflows.

<!-- theme: warning -->
> See the [CardSecure API documentation] for detailed information for integrating the CardSecure API with your application. See the [CardSecure Developer Guides](?path=docs/documentation/CardSecure.md) for additional helpful information.

## Requirements

To use CardSecure to detokenize sensitive data, you must first meet the following requirements:

- You must obtain CardSecure detokenization credentials.
- You must provide the IP addresses or range that you will use to send detokenization requests to CardSecure. Your IP address will be used to authenticate and log your detokenization requests.

## Restrictions and Limitations

Before you begin, you should understand the following restrictions and limitations:

- You must not use the CardSecure API to detokenize payment account data that is tokenized using a P2PE solution (Bolt Terminal, PanPad, or another integrated P2PE device).
- You accept all liability for the security of your customers' data. We assume no liability for your business' handling of unencrypted, cleartext data.

<!-- theme: danger -->
> You should only use CardSecure to detokenize and handle cleartext payment account numbers (PANs) if your business is a PCI Level 1 or Level 2 certified merchant. If you are not already certified for compliance with the PCI Security Standards Council's standards and guidelines for handling sensitive account data, see https://www.pcisecuritystandards.org for more information.

# Authentication

Unlike tokenize requests, which do not require authentication, the `detokenize` request requires your client application to authenticate using the following methods:

- API credentials, included in the Authorization header in each request. You receive a unique username and password, which you must encode and include in the Authorization header of each request.
- A whitelisted IP address or range, used by your application to make requests to CardSecure. You must provide this address or range for us to configure your account. Requests from any IP addresses that are not whitelisted will be denied.

# Service Endpoint

The CardSecure API includes the detokenize endpoint to retrieve the unencrypted account data associated with a single token.

> The detokenize request only returns the cleartext account number associated with a token. The response does **not** include any additional data stored with the token (for example `cvv` or `expiry` values).

## detokenize

A request to the detokenize endpoint locates the token in the CardSecure database, decrypts the ciphertext, and returns the original value to the application or returns an error if the token is not found. 

The detokenize request requires the `site` that your merchant account is boarded to, and the `token`  that you want to detokenize.

The detokenize response includes the `account`, which is the cleartext string used to generate the token.

#### Endpoint URL

The URL for the tokenize endpoint is:

`https://<site>.cardconnect.com/cardsecure/api/v1/<domain>/detokenize`

Where:

- `<site>` is the site that your merchant account is configured to use.
- `<domain>` is the CardSecure domain that was used to generate the token. `<domain>` can be one of the following values:
    - `ccn` - The domain used to tokenize payment account numbers.
    - `ssn` - The domain used to tokenize Social Security numbers.

#### Request Header

The following key/value pairs are required in the request header:

| Key | Value |
| --- | --- |
| Content-Type | application/json
| Authorization	| Basic

#### Request Parameters

Fields in bold are required.

| Field | Description |
| --- | --- |
| **site** | The site that your merchant account is configured to use.
| **token**	| The token to detokenize and retrieve account data from.

#### Example Detokenize Request

```json
{
  "site" : "pipeline",
  "token" : "9417119164771111"
}
```

#### Response Parameters

A successful response includes the following data:

| Field	| Description
| --- | --- |
| message	| The status of the request. <br> <br> Returns "No Error" if the request was successful. <br> <br> Returns a descriptive error message if the request failed. <br> <br> See Error Codes and Messages for a complete list of possible error codes and messages.
| errorcode	| An error code, if the request encountered an error. <br> <br> Returns "0" if the request was successful. <br> <br> See Error Codes and Messages for a complete list of possible error codes and messages.
| account	| The unencrypted string associated with the token provided in the request.

#### Example Successful Detokenize Response

```json
{
  "message" : "No Error",
  "errorcode" : 0,
  "account" : "4111111111111111"
}
```
