# Gateway Response Codes

This page describes the responses you may receive when sending CardPointe Gateway API requests, including HTTP errors and a detailed reference for authorization response codes. 

# HTTP Errors

HTTP errors are returned when the client (for example, the browser) encounters an issue communicating with the host server. 

| Error | Description |
| --- | --- 
| 400	| The server was unable to decipher the request because of invalid syntax. Update the request before repeating.
| 401	| The credentials passed in the request were not accepted by the server. Update the request before repeating.
| 404 | The endpoint url path specified in the request was incorrect.
| 500	| The host server encountered an issue and was unable to send a response.

# Response Codes

The CardPointe Gateway validates various parameters of a request prior to sending it to a payment card processor. For this internal validation, the card processor field (`respproc`) contains **PPS**.

If the request passes the internal validation, the request is forwarded to the appropriate payment processor based on the merchant ID, and the payment processor supplies the response values.

Each payment processor has a unique set of response codes. Generally, a processor response code (`respcode`) beginning with "00" or "000" is a successful authorization request. The `respstat` response indicates one of the following:

- A - Approval
- B - Temporary processing issue, such as a network error
- C - Rejection

## Gateway (PPS) Response Codes

The CardPointe Gateway validates various parameters of an authorization request prior to sending it to the payment card processor. The following response codes describe the approval and decline responses returned by the CardPointe Gateway. 

> The CardPointe Gateway does **not** validate the expiration date or automatically decline expired cards. Some issuers accept authorizations for expired cards; however, the issuer may decline an expired card based on its specific policies.

#### Gateway Response Codes (PPS)

| respproc	| respcode	| respstat	| resptext
| --- | --- | --- | ---
| PPS	| 00	| A	| "Approval"
| PPS	| 01	| A	| "VoiceAuth Approved"
| PPS	| 08	| A	| "Profile Deleted"
| PPS	| 09	| A	| "Profile Saved"
| PPS	| 11	| C	| "Invalid card"
| PPS	| 12	| C	| "Invalid track"
| PPS	| 13	| C	| "Bad card check digit"
| PPS	| 14	| C	| "Non-numeric CVV"
| PPS	| 15	| C	| "Non-numeric expiry"
| PPS	| 16	| C	| "Card expired"
| PPS	| 17	| C	| "Invalid zip"
| PPS	| 18	| C	| "CardDefense Review"
| PPS	| 19	| C	| "CardDefense Decline"
| PPS	| 21	| C	| "Invalid merchant"
| PPS	| 22	| C	| "No auth route"
| PPS	| 23	| C	| "No auth queue"
| PPS	| 24	| C	| "Reversal not supported"
| PPS	| 25	| C	| "No matching auth for reversal"
| PPS	| 26	| A	| "Txn Settled"
| PPS	| 27	| C	| "Txn Batched"
| PPS	| 28	| C	| "Txn not settled"
| PPS	| 29	| C	| "Txn not found"
| PPS	| 31	| C	| "Invalid currency"
| PPS	| 32	| C	| "Wrong currency for merch"
| PPS	| 33	| C	| "Unknown card type"
| PPS	| 34	| C	| "Invalid field"
| PPS	| 35	| C	| "No postal code"
| PPS	| 36	| C	| "Duplicate sequence"
| PPS	| 37	| C	| "CVV mismatch"
| PPS	| 38	| C	| "CVV is required"
| PPS	| 39	| C	| "Void not permitted after 30 days"
| PPS	| 41	| C	| "Below min amount"
| PPS	| 42	| C	| "Above max amount"
| PPS	| 43	| C	| "Invalid amount"
| PPS	| 44	| C	| "Prepaid not supported"
| PPS	| 45	| C	| "Refunds without reference not supported"
| PPS	| 46	| C	| "Partial refunds not supported"
| PPS	| 48	| C	| "Echeck not supported"
| PPS	| 61	| B	| "Line down"
| PPS	| 62	| B	| "Timed out" <br> <br> **Note**: See Handling Timed-Out Transactions for more information.
| PPS	| 63 | C	| "Bad resp format"
| PPS	| 64	| C	| "Bad HTTP header"
| PPS	| 65	| C	| "Socket close error"
| PPS	| 66	| C	| "Response mismatch"
| PPS	| 70	| C	| "Voice authorization cannot be voided"
| PPS	| 71	| C	| "EMV data not authorized"
| PPS	| 91	| C	| "No TokenSecure"
| PPS	| 92	| C	| "No Merchant table"
| PPS	| 93	| C	| "No Database"
| PPS	| 94	| C	| "No action"
| PPS	| 95	| C	| "Missing config"
| PPS	| 96	| C	| "Profile not found"
| PPS	| 97	| C	| "Merchant disabled"
| PPS	| 98	| C	| "Invalid token"
| PPS	| 99	| C	| "Invalid card"
| PPS	| 101	| C	| “AVS Mismatch”
| PPS	| 102	| C	| “Service Fee Declined”
| PPS	| 103	| C	| “Service Fee Txn not found”
| PPS	| 104	| C	| “Surcharge Not Supported”
| PPS	| 105	| C	| "Invalid EMV"
| PPS	| 106	| B	| "Invalid method. Please insert card"

## Processing Host Response Codes

#### American Express Response Codes (AMEX)

| respproc	| respcode	| respstat	| resptext
| --- | --- | --- | ---
| AMEX	| 000	| A	| "Approval"	
| AMEX	| 001	| A	| "Approval with ID"
| AMEX	| 002	| A	| "Partial Approval"
| AMEX	| 003	| A	| "Approval VIP"
| AMEX	| 100	| C	| "Decline"
| AMEX	| 101	| C	| "Expired card"
| AMEX	| 103	| C	| "CID failed"
| AMEX	| 105	| C	| "Card cancelled"
| AMEX	| 107	| C	| "Call issuer"
| AMEX	| 109	| C	| "Invalid merchant"
| AMEX	| 110	| C	| "Invalid amount"
| AMEX	| 111 | C	| "Invalid card"
| AMEX	| 115	| C	| "Function not supported"
| AMEX	| 116	| C	| "Insufficient funds"
| AMEX	| 121	| C	| "Limit exceeded"
| AMEX	| 122	| C	| "Invalid CID"
| AMEX	| 181	| C	| "Invalid feature"
| AMEX	| 182	| B	| "Try later"
| AMEX	| 187	| C	| "Deny - new card issued"
| AMEX	| 188	| C	| "Deny - canceled"
| AMEX	| 200	| C	| "Pick up card"
| AMEX	| 400	| A	| "Reversal"

#### First Data North Response Codes (FNOR)

| respproc	| respcode	| respstat	| resptext
| --- | --- | --- | ---
| FNOR	| 00	| A	| "Approval"
| FNOR	| 10	| A	| "Partial Approval"
| FNOR	| 76	| A	| "Reversed"
| FNOR	| 85	| A	| "Validated"
| FNOR	| 01	| C	| "Refer to issuer"
| FNOR	| 03	| C	| "Invalid merchant"
| FNOR	| 05	| C	| "Do not honor"
| FNOR	| 12	| C	| "Invalid transaction"
| FNOR	| 13	| C	| "Invalid amount"
| FNOR	| 14	| C	| "Invalid card number"
| FNOR	| 25	| C	| "Invalid terminal"
| FNOR	| 28	| B	| "Please retry"
| FNOR	| 51	| C	| "Declined"
| FNOR	| 54	| C	| "Wrong expiration"
| FNOR	| 55	| C	| "Incorrect pin"
| FNOR	| 57	| C	| "Invalid txn for card"
| FNOR	| 60	| C	| "Declined" # Capture card
| FNOR	| 61	| C	| "Exceeds withdrawal limit"
| FNOR	| 63	| C	| "Service not allowed"
| FNOR	| 69	| C	| "Host key error"
| FNOR	| 75	| C	| "Pin try exceeded"
| FNOR	| 89	| C	| "Invalid Term ID"
| FNOR	| 91	| B	| "System error"
| FNOR	| 94	| C	| "Duplicate tran"
| FNOR	| 1A	| C	| "Need addl authentication"
| FNOR	| 1B	| C	| "Need PIN data"
| FNOR	| C2	| C	| "CVV decline"
| FNOR	| CE	| C	| "System problem"
| FNOR	| N3	| C	| "Invalid Account"
| FNOR	| NG	| C	| "Reversal rejected"
| FNOR	| NH	| C	| "Enter lesser amt"
| FNOR	| NI	| C	| "Pin decrypt error"
| FNOR	| NK	| C	| "Crypto box offline"
| FNOR	| NL	| B	| "Debit switch down"
| FNOR	| NM	| C	| "Issuer unavailable" # Debit cards
| FNOR	| NN	| C	| "Bad debit card"
| FNOR	| NP	| C	| "Bad debit merchant"
| FNOR	| NQ	| C	| "Tran limit exceeded"
| FNOR	| NR	| C	| "Tran freq exceeded"
| FNOR	| NS	| C	| "Error decrypting pin"
| FNOR	| NU	| C	| "Insufficient funds"
| FNOR	| RW	| C	| "Reversal outside window"

#### First Data Nashville Response Codes (NASH)

| respproc | respcode | respstat | resptext                           |
|----------|----------|----------|------------------------------------|
| NASH     | 00       | A        | "Approval"                         |
| NASH     | 10       | A        | "Partial Approval"                 |
| NASH     | 76       | A        | "Reversed"                         |
| NASH     | 85       | A        | "Validated"                        |
| NASH     | 01       | C        | "Refer to issuer"                  |
| NASH     | 03       | C        | "Invalid merchant"                 |
| NASH     | 05       | C        | "Do not honor"                     |
| NASH     | 12       | C        | "Invalid transaction"              |
| NASH     | 13       | C        | "Invalid amount"                   |
| NASH     | 14       | C        | "Invalid card number"              |
| NASH     | 25       | C        | "Invalid terminal"                 |
| NASH     | 28       | B        | "Please retry"                     |
| NASH     | 51       | C        | "Declined"                         |
| NASH     | 54       | C        | "Wrong expiration" # Expired card  |
| NASH     | 55       | C        | "Incorrect pin"                    |
| NASH     | 57       | C        | "Invalid txn for card"             |
| NASH     | 60       | C        | "Declined" # Capture card          |
| NASH     | 61       | C        | "Exceeds withdrawal limit"         |
| NASH     | 63       | C        | "Service not allowed"              |
| NASH     | 69       | C        | "Host key error"                   |
| NASH     | 75       | C        | "Pin try exceeded"                 |
| NASH     | 89       | C        | "Invalid Term ID"                  |
| NASH     | 91       | B        | "System error"                     |
| NASH     | 94       | C        | "Duplicate tran"                   |
| NASH     | C2       | C        | "CVV decline"                      |
| NASH     | CE       | C        | "System problem"                   |
| NASH     | N3       | C        | "Invalid Account"                  |
| NASH     | NG       | C        | "Reversal rejected"                |
| NASH     | NH       | C        | "Enter lesser amt"                 |
| NASH     | NI       | C        | "Pin decrypt error"                |
| NASH     | NK       | C        | "Crypto box offline"               |
| NASH     | NL       | B        | "Debit switch down"                |
| NASH     | NM       | C        | "Issuer unavailable" # Debit cards |
| NASH     | NN       | C        | "Bad debit card"                   |
| NASH     | NP       | C        | "Bad debit merchant"               |
| NASH     | NQ       | C        | "Tran limit exceeded"              |
| NASH     | NR       | C        | "Tran freq exceeded"               |
| NASH     | NS       | C        | "Error decrypting pin"             |
| NASH     | NU       | C        | "Insufficient funds"               |
| NASH     | RW       | C        | "Reversal outside window"          |

#### First Data Omaha Response Codes (FOMA)

| respproc | respcode | respstat | resptext            |
|----------|----------|----------|---------------------|
| FOMA     | 0        | A        | "Approval"          |
| FOMA     | 00       | A        | "Approval"          |
| FOMA     | 01       | C        | "Call"              |
| FOMA     | 02       | C        | "Invalid amount"    |
| FOMA     | 03       | C        | "Invalid card"      |
| FOMA     | 04       | C        | "Invalid merchant"  |
| FOMA     | 05       | C        | "Decline"           |
| FOMA     | 06       | C        | "Wrong expiry"      |
| FOMA     | 07       | C        | "Invalid expiry"    |
| FOMA     | 08       | C        | "Transmit error"    |
| FOMA     | 09       | B        | "Timeout - retry"   |
| FOMA     | 10       | A        | "Approval"          |
| FOMA     | 11       | B        | "Invalid batch"     |
| FOMA     | 12       | C        | "Unmatched void"    |
| FOMA     | 13       | C        | "Invalid tran type" |
| FOMA     | 14       | C        | "Invalid CVV2"      |
| FOMA     | 76       | A        | "Reversed"          |
| FOMA     | 99       | C        | "Decline"           |

#### First Data South Response Codes (FDMS)

| respproc | respcode | respstat | resptext    |
|----------|----------|----------|-------------|
| FDMS     | 0        | A        | "Approval"  |
| FDMS     | 3        | C        | "Not valid" |
| FDMS     | 4        | C        | "Referral"  |
| FDMS     | 6        | C        | "Reenter"   |
| FDMS     | 7        | C        | "Decline"   |
| FDMS     | A00      | A        | "Approval"  |

#### Rapid Connect Response Codes (RPCT)

| respproc | respcode | respstat | resptext                                                                                                                              |
|----------|----------|----------|---------------------------------------------------------------------------------------------------------------------------------------|
| RPCT     | 000      | A        | "Approve"                                                                                                                             |
| RPCT     | 001      | C        | "Schema Validation Error"                                                                                                             |
| RPCT     | 002      | A        | "Approve for partial amount"                                                                                                          |
| RPCT     | 003      | A        | "Approve VIP"                                                                                                                         |
| RPCT     | 100      | C        | "Do not honor"                                                                                                                        |
| RPCT     | 101      | C        | "Expired card"                                                                                                                        |
| RPCT     | 102      | C        | "Suspected fraud"                                                                                                                     |
| RPCT     | 104      | C        | "Restricted card"                                                                                                                     |
| RPCT     | 105      | C        | "Call acquirer security department"                                                                                                   |
| RPCT     | 106      | C        | "Allowable PIN tries exceeded"                                                                                                        |
| RPCT     | 107      | C        | "Call for authorization"                                                                                                              |
| RPCT     | 108      | C        | "Refer to issuer special conditions"                                                                                                  |
| RPCT     | 109      | C        | "Invalid merchant"                                                                                                                    |
| RPCT     | 110      | C        | "Invalid amount"                                                                                                                      |
| RPCT     | 114      | C        | "Invalid account type"                                                                                                                |
| RPCT     | 116      | C        | "Not sufficient funds"                                                                                                                |
| RPCT     | 117      | C        | "Incorrect PIN"                                                                                                                       |
| RPCT     | 118      | C        | "No card record"                                                                                                                      |
| RPCT     | 119      | C        | "Transaction not permitted to cardholder"                                                                                             |
| RPCT     | 120      | C        | "Transaction not permitted to terminal"                                                                                               |
| RPCT     | 121      | C        | "Exceeds withdrawal amount"                                                                                                           |
| RPCT     | 122      | C        | "Security violation"                                                                                                                  |
| RPCT     | 123      | C        | "Exceeds withdrawal frequency limit"                                                                                                  |
| RPCT     | 124      | C        | "Violation of law"                                                                                                                    |
| RPCT     | 129      | C        | "Suspected counterfeit card"                                                                                                          |
| RPCT     | 130      | C        | "Invalid terminal"                                                                                                                    |
| RPCT     | 131      | C        | "Invalid account number"                                                                                                              |
| RPCT     | 132      | C        | "Unmatched card expiry date"                                                                                                          |
| RPCT     | 150      | C        | "Invalid merchant set up"                                                                                                             |
| RPCT     | 151      | C        | "Activation failed"                                                                                                                   |
| RPCT     | 152      | C        | "Exceeds limit"                                                                                                                       |
| RPCT     | 153      | C        | "Already redeemed"                                                                                                                    |
| RPCT     | 154      | C        | "Over monthly limit"                                                                                                                  |
| RPCT     | 155      | C        | "Recharge amount exceeded"                                                                                                            |
| RPCT     | 156      | C        | "Max number of recharges exceeded"                                                                                                    |
| RPCT     | 157      | C        | "Invalid entry"                                                                                                                       |
| RPCT     | 208      | C        | "Lost card"                                                                                                                           |
| RPCT     | 209      | C        | "Stolen card"                                                                                                                         |
| RPCT     | 302      | C        | "Account closed"                                                                                                                      |
| RPCT     | 303      | C        | "Unknown account"                                                                                                                     |
| RPCT     | 304      | C        | "Inactive account"                                                                                                                    |
| RPCT     | 308      | C        | "Already active"                                                                                                                      |
| RPCT     | 311      | C        | "Not lost or stolen"                                                                                                                  |
| RPCT     | 315      | C        | "Bad mag stripe"                                                                                                                      |
| RPCT     | 316      | C        | "Incorrect location"                                                                                                                  |
| RPCT     | 317      | C        | "Max balance exceeded"                                                                                                                |
| RPCT     | 318      | C        | "Invalid amount"                                                                                                                      |
| RPCT     | 319      | C        | "Invalid clerk"                                                                                                                       |
| RPCT     | 320      | C        | "Invalid password"                                                                                                                    |
| RPCT     | 321      | C        | "Invalid new password"                                                                                                                |
| RPCT     | 322      | C        | "Exceeded account reloads"                                                                                                            |
| RPCT     | 323      | C        | "Password retry exceeded"                                                                                                             |
| RPCT     | 326      | C        | "Incorrect transaction version or format number for POS"                                                                              |
| RPCT     | 327      | C        | "Request not permitted by this account"                                                                                               |
| RPCT     | 328      | C        | "Request not permitted by this merchant location"                                                                                     |
| RPCT     | 329      | C        | "Bad repay date"                                                                                                                      |
| RPCT     | 330      | C        | "Bad checksum"                                                                                                                        |
| RPCT     | 331      | C        | "Balance not available (denial)"                                                                                                      |
| RPCT     | 332      | C        | "Account locked"                                                                                                                      |
| RPCT     | 333      | C        | "No previous transaction"                                                                                                             |
| RPCT     | 334      | C        | "Already reversed"                                                                                                                    |
| RPCT     | 336      | C        | "Bad Authorization ID"                                                                                                                |
| RPCT     | 337      | C        | "Too many transactions requested"                                                                                                     |
| RPCT     | 338      | C        | "No transactions available/no more transactions available"                                                                            |
| RPCT     | 339      | C        | "Transaction history not available"                                                                                                   |
| RPCT     | 340      | C        | "New password required"                                                                                                               |
| RPCT     | 341      | C        | "Invalid status change"                                                                                                               |
| RPCT     | 342      | C        | "Void of activation after account activity"                                                                                           |
| RPCT     | 343      | C        | "No phone service"                                                                                                                    |
| RPCT     | 344      | C        | "Internet access disabled"                                                                                                            |
| RPCT     | 351      | C        | "PIN authentication required"                                                                                                         |
| RPCT     | 355      | C        | "Invalid currency"                                                                                                                    |
| RPCT     | 357      | C        | "Currency conversion error"                                                                                                           |
| RPCT     | 359      | C        | "The terminal transaction number did not match"                                                                                       |
| RPCT     | 367      | C        | "Target embossed card entered and Transaction count entered do not match"                                                             |
| RPCT     | 368      | C        | "No account link"                                                                                                                     |
| RPCT     | 369      | C        | "Invalid time zone"                                                                                                                   |
| RPCT     | 370      | C        | "Account on hold"                                                                                                                     |
| RPCT     | 372      | C        | "Promo location restricted"                                                                                                           |
| RPCT     | 373      | C        | "Invalid Card Account"                                                                                                                |
| RPCT     | 374      | C        | "Product code(s) restricted"                                                                                                          |
| RPCT     | 375      | C        | "Bad Post Date. The Post Date is not a valid date"                                                                                    |
| RPCT     | 376      | C        | "Account status is void lock"                                                                                                         |
| RPCT     | 377      | C        | "Already active and reloadable"                                                                                                       |
| RPCT     | 378      | C        | "Account is Purged"                                                                                                                   |
| RPCT     | 380      | C        | "Bulk activation error"                                                                                                               |
| RPCT     | 381      | C        | "Bulk activation un-attempted error"                                                                                                  |
| RPCT     | 382      | C        | "Bulk activation package amount error"                                                                                                |
| RPCT     | 383      | C        | "Store location zero not allowed"                                                                                                     |
| RPCT     | 384      | C        | "Account row locked"                                                                                                                  |
| RPCT     | 385      | C        | "Accepted but not yet processed"                                                                                                      |
| RPCT     | 401      | C        | "Offer Processing Error"                                                                                                              |
| RPCT     | 402      | C        | "TransArmor Service Unavailable"                                                                                                      |
| RPCT     | 403      | C        | "TransArmor Invalid Token or Account Number"                                                                                          |
| RPCT     | 404      | C        | "TransArmor Key Error"                                                                                                                |
| RPCT     | 500      | C        | "Decline"                                                                                                                             |
| RPCT     | 501      | C        | "Date of Birth Error for Check Processing"                                                                                            |
| RPCT     | 502      | C        | "Invalid State Code"                                                                                                                  |
| RPCT     | 503      | C        | "New Account Information"                                                                                                             |
| RPCT     | 504      | C        | "Do not try again"                                                                                                                    |
| RPCT     | 505      | B        | "Please retry"                                                                                                                        |
| RPCT     | 506      | C        | "Invalid Checking Account Number"                                                                                                     |
| RPCT     | 507      | C        | "New Account Information available"                                                                                                   |
| RPCT     | 508      | B        | "Try again later - Declined"                                                                                                          |
| RPCT     | 509      | C        | "Do not try again - The card has expired"                                                                                             |
| RPCT     | 510      | C        | "New Account Information - The card has expired"                                                                                      |
| RPCT     | 511      | B        | "The card has expired. Get the new expiration date and try again."                                                                    |
| RPCT     | 512      | C        | "Service not allowed"                                                                                                                 |
| RPCT     | 513      | C        | "Decline. Transaction not permitted to acquirer or terminal."                                                                         |
| RPCT     | 514      | C        | "Do not try again - There was security violation"                                                                                     |
| RPCT     | 515      | C        | "Declined. No term record on First Data system."                                                                                      |
| RPCT     | 516      | B        | "Please retry"                                                                                                                        |
| RPCT     | 517      | C        | "CVV2 Declined"                                                                                                                       |
| RPCT     | 518      | C        | "Invalid account/date or sales date in future"                                                                                        |
| RPCT     | 519      | C        | "Invalid Effective Date"                                                                                                              |
| RPCT     | 520      | C        | "Reversal Rejected"                                                                                                                   |
| RPCT     | 521      | C        | "Enter lesser amount"                                                                                                                 |
| RPCT     | 522      | C        | "Cash Back greater than total Transaction amount"                                                                                     |
| RPCT     | 523      | C        | "Crypto box is offline"                                                                                                               |
| RPCT     | 524      | C        | "Debit Switch unavailable"                                                                                                            |
| RPCT     | 525      | C        | "Debit/EBT network gateway cannot get through to the ISSUER"                                                                          |
| RPCT     | 526      | C        | "Undefined Card"                                                                                                                      |
| RPCT     | 527      | C        | "Network Response indicates that Merchant ID / SE is invalid"                                                                         |
| RPCT     | 528      | C        | "Debit/EBT transaction count exceeds pre-determined limit in specified time/ Withdrawal limit exceeded"                               |
| RPCT     | 529      | C        | "Resubmission of transaction violates debit/EBT network frequency"                                                                    |
| RPCT     | 530      | C        | "The authorizing network has a problem decrypting the cryptogram in the request"                                                      |
| RPCT     | 531      | C        | "Retry with 3DS" <br> See our 3-D Secure 2.0 support article for information on integrating 3-D Secure if required for your business. |
| RPCT     | 532      | C        | "The DUKPT Base Derivation key is missing or incorrect in the PIN pad or PIN key synchronization error"                               |
| RPCT     | 540      | C        | "Edit Honor"                                                                                                                          |
| RPCT     | 541      | C        | "No Savings Account"                                                                                                                  |
| RPCT     | 542      | C        | "DUKPT: An error while processing the PIN block that is not related to the point- of sale equipment"                                  |
| RPCT     | 550      | C        | "Invalid Vehicle"                                                                                                                     |
| RPCT     | 551      | C        | "Invalid Driver"                                                                                                                      |
| RPCT     | 552      | C        | "Invalid Product"                                                                                                                     |
| RPCT     | 553      | C        | "Exceeds transaction total limit per product class"                                                                                   |
| RPCT     | 554      | C        | "Over daily limit"                                                                                                                    |
| RPCT     | 555      | C        | "Invalid Date/Time"                                                                                                                   |
| RPCT     | 556      | C        | "Exceeds quantity"                                                                                                                    |
| RPCT     | 557      | C        | "Invalid prompt entry"                                                                                                                |
| RPCT     | 558      | C        | "Invalid Track 2 data"                                                                                                                |
| RPCT     | 559      | C        | "Voyager ID problem"                                                                                                                  |
| RPCT     | 560      | C        | "Invalid Odometer"                                                                                                                    |
| RPCT     | 561      | C        | "Invalid Restriction Code"                                                                                                            |
| RPCT     | 562      | C        | "Pay at pump not allowed"                                                                                                             |
| RPCT     | 563      | C        | "Over fuel limit"                                                                                                                     |
| RPCT     | 564      | C        | "Over cash limit"                                                                                                                     |
| RPCT     | 565      | C        | "Fuel price error"                                                                                                                    |
| RPCT     | 566      | C        | "Y or N required"                                                                                                                     |
| RPCT     | 567      | C        | "Over repair limit"                                                                                                                   |
| RPCT     | 568      | C        | "Over additive limit"                                                                                                                 |
| RPCT     | 569      | C        | "Invalid user"                                                                                                                        |
| RPCT     | 701      | A        | "Approved EMV Key Load"                                                                                                               |
| RPCT     | 702      | C        | "EMV Key Download Error"                                                                                                              |
| RPCT     | 703      | A        | "Approved EMV Key Load-more key load data pending"                                                                                    |
| RPCT     | 704      | C        | "Pick Up Card"                                                                                                                        |
| RPCT     | 708      | A        | "Honor With Authentication"                                                                                                           |
| RPCT     | 721      | C        | "Invalid ZIP Code"                                                                                                                    |
| RPCT     | 722      | C        | "Invalid value in the field"                                                                                                          |
| RPCT     | 723      | C        | "Driver's License or ID is Required"                                                                                                  |
| RPCT     | 724      | C        | "Referred - Not Active"                                                                                                               |
| RPCT     | 726      | C        | "Unable to Locate Record On File"                                                                                                     |
| RPCT     | 727      | C        | "Refer - Call Authorization"                                                                                                          |
| RPCT     | 728      | C        | "Referred - Skip Trace Info"                                                                                                          |
| RPCT     | 729      | C        | "Hard Negative Info On File"                                                                                                          |
| RPCT     | 731      | C        | "Rejected Lost/Stolen Checks"                                                                                                         |
| RPCT     | 740      | C        | "Totals Unavailable"                                                                                                                  |
| RPCT     | 767      | C        | "Hard Capture; Pick Up"                                                                                                               |
| RPCT     | 771      | C        | "Amount Too Large"                                                                                                                    |
| RPCT     | 772      | C        | "Duplicate Return"                                                                                                                    |
| RPCT     | 773      | C        | "Unsuccessful"                                                                                                                        |
| RPCT     | 774      | C        | "Duplicate Reversal"                                                                                                                  |
| RPCT     | 775      | C        | "Subsystem Unavailable"                                                                                                               |
| RPCT     | 776      | C        | "Duplicate Completion"                                                                                                                |
| RPCT     | 782      | C        | "Count Exceeds Limit"                                                                                                                 |
| RPCT     | 785      | A        | "No reason to decline"                                                                                                                |
| RPCT     | 790      | C        | "Do not resubmit same transaction but continue billing process in subsequent billing period"                                          |
| RPCT     | 791      | C        | "Stop recurring payment requests"                                                                                                     |
| RPCT     | 792      | C        | "See attendant"                                                                                                                       |
| RPCT     | 801      | C        | "Over merchandise limit"                                                                                                              |
| RPCT     | 802      | C        | "Imprint card"                                                                                                                        |
| RPCT     | 803      | C        | "Not on file"                                                                                                                         |
| RPCT     | 804      | C        | "Fuel only"                                                                                                                           |
| RPCT     | 805      | C        | "Velocity exceeded"                                                                                                                   |
| RPCT     | 806      | C        | "Authorization ID needed"                                                                                                             |
| RPCT     | 807      | C        | "Over non-fuel limit"                                                                                                                 |
| RPCT     | 808      | C        | "Invalid location"                                                                                                                    |
| RPCT     | 809      | C        | "Over card velocity count"                                                                                                            |
| RPCT     | 810      | C        | "Over card velocity amount"                                                                                                           |
| RPCT     | 811      | C        | "Over issuer velocity count"                                                                                                          |
| RPCT     | 812      | C        | "Over issuer velocity amount"                                                                                                         |
| RPCT     | 813      | C        | "Over merchant daily velocity count"                                                                                                  |
| RPCT     | 814      | C        | "Over merchant daily velocity amount"                                                                                                 |
| RPCT     | 815      | C        | "Over merchant daily velocity both"                                                                                                   |
| RPCT     | 816      | C        | "Over merchant product velocity amount"                                                                                               |
| RPCT     | 817      | C        | "Over merchant product velocity count"                                                                                                |
| RPCT     | 818      | C        | "Over merchant product velocity both"                                                                                                 |
| RPCT     | 819      | C        | "Over chain daily velocity count"                                                                                                     |
| RPCT     | 820      | C        | "Over chain daily velocity amount"                                                                                                    |
| RPCT     | 821      | C        | "Over chain daily velocity both"                                                                                                      |
| RPCT     | 822      | C        | "Over chain product velocity count"                                                                                                   |
| RPCT     | 823      | C        | "Over chain product velocity both"                                                                                                    |
| RPCT     | 824      | C        | "Over chain product velocity amount"                                                                                                  |
| RPCT     | 825      | C        | "No chain ID for chain merchant"                                                                                                      |
| RPCT     | 826      | C        | "Signature required"                                                                                                                  |
| RPCT     | 902      | C        | "Invalid transaction"                                                                                                                 |
| RPCT     | 904      | C        | "Format error"                                                                                                                        |
| RPCT     | 906      | C        | "System Error"                                                                                                                        |
| RPCT     | 907      | C        | "Card issuer or switch inoperative"                                                                                                   |
| RPCT     | 908      | C        | "Transaction destination not found"                                                                                                   |
| RPCT     | 909      | C        | "System malfunction"                                                                                                                  |
| RPCT     | 911      | C        | "Card issuer timed out"                                                                                                               |
| RPCT     | 913      | C        | "Duplicate transaction"                                                                                                               |
| RPCT     | 914      | C        | "Original Authorization was not found"                                                                                                |
| RPCT     | 915      | C        | "Timeout Reversal not supported. Resend the original transaction"                                                                     |
| RPCT     | 920      | B        | "Security H/W or S/W error - try again"                                                                                               |
| RPCT     | 921      | C        | "Security H/W or S/W error"                                                                                                           |
| RPCT     | 923      | C        | "Request in progress"                                                                                                                 |
| RPCT     | 924      | C        | "Limit check failed"                                                                                                                  |
| RPCT     | 940      | C        | "Error"                                                                                                                               |
| RPCT     | 941      | C        | "Invalid issuer"                                                                                                                      |
| RPCT     | 942      | C        | "Customer cancellation"                                                                                                               |
| RPCT     | 944      | C        | "Invalid response"                                                                                                                    |
| RPCT     | 950      | C        | "Violation of business arrangement"                                                                                                   |
| RPCT     | 954      | C        | "CCV failed"                                                                                                                          |
| RPCT     | 958      | C        | "CCV2 failed"                                                                                                                         |
| RPCT     | 959      | C        | "CAV failed"                                                                                                                          |
| RPCT     | 963      | C        | "Acquirer channel unavailable"                                                                                                        |

#### Elavon Response Codes (ELV)

| respproc | respcode | respstat | resptext              |
|----------|----------|----------|-----------------------|
| ELV      | 00       | A        | "Approval"            |
| ELV      | 01       | C        | "Referral"            |
| ELV      | 03       | C        | "Invalid card"        |
| ELV      | 05       | C        | "Decline"             |
| ELV      | 06       | C        | "Expired card"        |
| ELV      | 07       | C        | "Service not allowed" |
| ELV      | 08       | C        | "Invalid term id"     |
| ELV      | 09       | C        | "Please retry"        |

#### PayPal Response Codes (PPAL)

| respproc | respcode | respstat | resptext                    |
|----------|----------|----------|-----------------------------|
| PPAL     | 00       | A        | "Approval"                  |
| PPAL     | 0004     | C        | "Invalid Argument"          |
| PPAL     | 0601     | C        | "Authorization has Expired" |
| PPAL     | 0602     | C        | "Already completed"         |
| PPAL     | 0609     | C        | "Invalid TransactionID"     |
| PPAL     | 0617     | A        | "Auth still valid"          |

#### PT Tampa Response Codes (PTAM)

| respproc | respcode | respstat | resptext                          |
|----------|----------|----------|-----------------------------------|
| PTAM     | 00       | A        | "Approval"                        |
| PTAM     | 01       | C        | "Refer to issuer"                 |
| PTAM     | 02       | C        | "Refer to issuer - special"       |
| PTAM     | 03       | C        | "Invalid merchant"                |
| PTAM     | 04       | C        | "Pick up card"                    |
| PTAM     | 05       | C        | "Do not honor"                    |
| PTAM     | 06       | C        | "General error"                   |
| PTAM     | 07       | C        | "Suspected fraud"                 |
| PTAM     | 08       | A        | "Approval"                        |
| PTAM     | 09       | C        | "Velocity limit exceeded"         |
| PTAM     | 10       | A        | "Partial Approval"                |
| PTAM     | 11       | A        | "VIP Approval"                    |
| PTAM     | 12       | C        | "Invalid transaction"             |
| PTAM     | 13       | C        | "Invalid amount"                  |
| PTAM     | 14       | C        | "Invalid card number"             |
| PTAM     | 15       | C        | "No such card issuer"             |
| PTAM     | 16       | C        | "Account not active"              |
| PTAM     | 17       | C        | "Declined"                        |
| PTAM     | 18       | C        | "Already reversed"                |
| PTAM     | 19       | B        | "Re-enter transaction"            |
| PTAM     | 23       | C        | "Duplicate batch"                 |
| PTAM     | 30       | C        | "Format error"                    |
| PTAM     | 33       | C        | "Expired card"                    |
| PTAM     | 38       | C        | "Invalid PIN"                     |
| PTAM     | 40       | C        | "Function not supported"          |
| PTAM     | 43       | C        | "Lost/stolen card"                |
| PTAM     | 54       | C        | "Wrong expiration" # Expired card |
| PTAM     | 57       | C        | "Invalid txn for card"            |
| PTAM     | 58       | C        | "Terminal not permitted"          |
| PTAM     | 63       | C        | "Invalid EMV"                     |
| PTAM     | 91       | C        | "Issuer or switch not operative"  |
| PTAM     | 92       | C        | "Unable to route"                 |
| PTAM     | 93       | C        | "Busy, please retry"              |
| PTAM     | 97       | B        | "System error"                    |
| PTAM     | 98       | B        | "Database error"                  |
| PTAM     | 99       | B        | "Forwarding error"                |

#### Paymentech Response Codes (PMT)

| respproc | respcode | respstat | resptext                                 |
|----------|----------|----------|------------------------------------------|
| PMT      | 000      | B        | "System Down" # 000 from Paymentech      |
| PMT      | 100      | A        | "Approval"                               |
| PMT      | 101      | A        | "Validated"                              |
| PMT      | 102      | A        | "Verified"                               |
| PMT      | 104      | A        | "Verified"                               |
| PMT      | 105      | A        | "Approved"                               |
| PMT      | 106      | A        | "Approved"                               |
| PMT      | 107      | A        | "Approved"                               |
| PMT      | 108      | A        | "Activated"                              |
| PMT      | 110      | A        | "Activated"                              |
| PMT      | 200      | B        | "Auth network down" # not documented     |
| PMT      | 201      | C        | "Invalid CC number"                      |
| PMT      | 202      | C        | "Bad amount"                             |
| PMT      | 203      | C        | "Zero amount"                            |
| PMT      | 204      | C        | "Other error"                            |
| PMT      | 220      | C        | "Invalid store number"                   |
| PMT      | 225      | C        | "Invalid field data"                     |
| PMT      | 227      | C        | "Missing companion data"                 |
| PMT      | 231      | C        | "Invalid merchant code"                  |
| PMT      | 233      | C        | "Card does not match type"               |
| PMT      | 234      | C        | "Duplicate order number"                 |
| PMT      | 236      | C        | "Auth Recycle host down"                 |
| PMT      | 238      | C        | "Invalid currency"                       |
| PMT      | 239      | C        | "Invalid card for merchant"              |
| PMT      | 241      | C        | "Illegal action"                         |
| PMT      | 243      | C        | "Invalid Level 3 field"                  |
| PMT      | 245      | C        | "Invalid or missing 3DS"                 |
| PMT      | 248      | C        | "Unused field not blank"                 |
| PMT      | 249      | C        | "Invalid MCC"                            |
| PMT      | 253      | C        | "Invalid Maestro Recurring"              |
| PMT      | 258      | C        | "Card type not allowed" # Canadian debit |
| PMT      | 260      | C        | "AVS mismatch"                           |
| PMT      | 275      | C        | "Ceiling limit"                          |
| PMT      | 299      | B        | "System Down" # 000 from Paymentech      |
| PMT      | 301      | C        | "Issuer unavailable"                     |
| PMT      | 302      | C        | "Insufficient funds"                     |
| PMT      | 303      | C        | "Processor decline"                      |
| PMT      | 304      | C        | "Invalid card"                           |
| PMT      | 305      | C        | "Already reversed"                       |
| PMT      | 306      | C        | "Reversal amount mismatch"               |
| PMT      | 307      | C        | "Reversal not found"                     |
| PMT      | 401      | C        | "Call"                                   |
| PMT      | 402      | C        | "Default Call"                           |
| PMT      | 501      | C        | "Pickup card"                            |
| PMT      | 502      | C        | "Card reported lost"                     |
| PMT      | 503      | C        | "Fraud" # Discover only                  |
| PMT      | 508      | C        | "Excessive PIN try"                      |
| PMT      | 509      | C        | "Over activity limit"                    |
| PMT      | 510      | C        | "Over frequency limit"                   |
| PMT      | 519      | C        | "On Negative File"                       |
| PMT      | 521      | C        | "Insufficient funds"                     |
| PMT      | 522      | C        | "Card expired"                           |
| PMT      | 530      | C        | "Do not honor"                           |
| PMT      | 531      | C        | "CVV mismatch"                           |
| PMT      | 532      | C        | "3DS exemption invalid"                  |
| PMT      | 570      | C        | "Recurring stopped"                      |
| PMT      | 571      | C        | "Recurring stopped"                      |
| PMT      | 591      | C        | "Invalid card number"                    |
| PMT      | 592      | C        | "Bad amount"                             |
| PMT      | 594      | C        | "Other error"                            |
| PMT      | 596      | C        | "Suspected fraud"                        |
| PMT      | 602      | C        | "Invalid issuer code"                    |
| PMT      | 603      | C        | "Invalid issuer code"                    |
| PMT      | 605      | C        | "Invalid expiry date"                    |
| PMT      | 606      | C        | "Invalid tran type"                      |
| PMT      | 607      | C        | "Invalid amount"                         |
| PMT      | 802      | C        | "Need ID"                                |
| PMT      | 806      | C        | "Restraint"                              |
| PMT      | 811      | C        | "Amex CID mismatch"                      |
| PMT      | 825      | C        | "No such account"                        |
| PMT      | 833      | C        | "Invalid Amex merchant"                  |
| PMT      | 902      | B        | "Issuer system error"                    |
| PMT      | 903      | C        | "Invalid expiry"                         |
| PMT      | 904      | C        | "Card not active"                        |

#### TSYS Response Codes (VPS)

| respproc | respcode | respstat | resptext                           |
|----------|----------|----------|------------------------------------|
| VPS      | 00       | A        | "Approval"                         |
| VPS      | 01       | C        | "Refer to issuer"                  |
| VPS      | 02       | C        | "Refer to issuer - special"        |
| VPS      | 03       | C        | "Invalid merchant"                 |
| VPS      | 04       | C        | "Pick up card"                     |
| VPS      | 05       | C        | "Do not honor"                     |
| VPS      | 06       | C        | "General error"                    |
| VPS      | 07       | C        | "Suspected fraud"                  |
| VPS      | 08       | A        | "Approval"                         |
| VPS      | 10       | A        | "Partial Approval"                 |
| VPS      | 11       | A        | "VIP Approval"                     |
| VPS      | 12       | C        | "Invalid transaction"              |
| VPS      | 13       | B        | "Invalid amount"                   |
| VPS      | 14       | B        | "Invalid card number"              |
| VPS      | 15       | C        | "No such card issuer"              |
| VPS      | 19       | B        | "Re-enter transaction"             |
| VPS      | 21       | B        | "Unable to back out"               |
| VPS      | 23       | C        | "Bad fee amount"                   |
| VPS      | 28       | B        | "File temporarily unavailable"     |
| VPS      | 33       | C        | "Wrong expiration date"            |
| VPS      | 34       | C        | "Suspected fraud"                  |
| VPS      | 36       | C        | "Restricted card"                  |
| VPS      | 39       | C        | "No credit account"                |
| VPS      | 41       | C        | "Card reported lost"               |
| VPS      | 43       | C        | "Card reported stolen"             |
| VPS      | 51       | C        | "Insufficient funds"               |
| VPS      | 54       | C        | "Wrong expiration" // Expired card |
| VPS      | 55       | C        | "Incorrect PIN"                    |
| VPS      | 57       | C        | "Invalid txn for card"             |
| VPS      | 58       | C        | "Terminal not permitted"           |
| VPS      | 59       | C        | "Service not allowed"              |
| VPS      | 61       | C        | "Exceeds withdrawal limit"         |
| VPS      | 62       | C        | "Invalid service code"             |
| VPS      | 63       | C        | "Security violation"               |
| VPS      | 65       | C        | "Activity limit exceeded"          |
| VPS      | 75       | C        | "PIN tries exceeded"               |
| VPS      | 76       | C        | "Unable to match"                  |
| VPS      | 78       | C        | "No account"                       |
| VPS      | 80       | C        | "Invalid date"                     |
| VPS      | 81       | C        | "Cryptographic error"              |
| VPS      | 82       | C        | "CVV incorrect"                    |
| VPS      | 85       | A        | "Validated"                        |
| VPS      | 91       | C        | "Issuer unavailable"               |
| VPS      | 92       | C        | "Destination not found"            |
| VPS      | 93       | C        | "Violation"                        |
| VPS      | 96       | B        | "System malfunction"               |
| VPS      | CV       | C        | "Card type error"                  |
| VPS      | EA       | C        | "Acct length error"                |
| VPS      | EB       | C        | "Check digit error"                |
| VPS      | H1       | C        | "Daily threshold exceeded"         |
| VPS      | HV       | C        | "Hierarchy validation"             |
| VPS      | N7       | C        | "CVV mismatch"                     |
| VPS      | V1       | C        | "Failure VM"                       |

#### Moneris Response Codes (MNS)

| respproc | respcode | respstat | resptext                           |
|----------|----------|----------|------------------------------------|
| MNS      | 00       | A        | "Approval"                         |
| MNS      | 01       | A        | "Approval"                         |
| MNS      | 02       | C        | "Refer to issuer - special"        |
| MNS      | 03       | C        | "Invalid merchant"                 |
| MNS      | 04       | C        | "Pick up card"                     |
| MNS      | 05       | C        | "Do not honor"                     |
| MNS      | 06       | C        | "General error"                    |
| MNS      | 07       | C        | "Suspected fraud"                  |
| MNS      | 12       | C        | "Invalid transaction"              |
| MNS      | 13       | C        | "Invalid amount"                   |
| MNS      | 14       | C        | "Invalid card number"              |
| MNS      | 15       | C        | "No such card issuer"              |
| MNS      | 19       | B        | "Re-enter transaction"             |
| MNS      | 21       | B        | "Unable to back out"               |
| MNS      | 23       | C        | "Bad fee amount"                   |
| MNS      | 28       | B        | "File temporarily unavailable"     |
| MNS      | 34       | C        | "Suspected fraud"                  |
| MNS      | 39       | C        | "No credit account"                |
| MNS      | 40       | C        | "Decline"                          |
| MNS      | 41       | C        | "Card reported lost"               |
| MNS      | 43       | C        | "Card reported stolen"             |
| MNS      | 51       | C        | "Insufficient funds"               |
| MNS      | 54       | C        | "Wrong expiration" // Expired card |
| MNS      | 55       | C        | "Incorrect PIN"                    |
| MNS      | 57       | C        | "Invalid txn for card"             |
| MNS      | 58       | C        | "Terminal not permitted"           |
| MNS      | 59       | C        | "Service not allowed"              |
| MNS      | 61       | C        | "Exceeds withdrawal limit"         |
| MNS      | 62       | C        | "Invalid service code"             |
| MNS      | 63       | C        | "Security violation"               |
| MNS      | 65       | C        | "Activity limit exceeded"          |
| MNS      | 75       | C        | "PIN tries exceeded"               |
| MNS      | 76       | C        | "Unable to match"                  |
| MNS      | 78       | C        | "No account"                       |
| MNS      | 80       | C        | "Invalid date"                     |
| MNS      | 81       | C        | "Cryptographic error"              |
| MNS      | 82       | C        | "CVV incorrect"                    |
| MNS      | 85       | A        | "Validated"                        |
| MNS      | 91       | C        | "Issuer unavailable"               |
| MNS      | 92       | C        | "Destination not found"            |
| MNS      | 93       | C        | "Violation"                        |
| MNS      | 96       | B        | "System malfunction"               |
| MNS      | 98       | C        | "Duplicate"                        |
| MNS      | 99       | C        | "Decline"                          |
| MNS      | N7       | C        | "Call"                             |

#### DPS Payment Express Response Codes (DPS)

| respproc | respcode | respstat | resptext                       |
|----------|----------|----------|--------------------------------|
| DPS      | 00       | A        | "Approval"                     |
| DPS      | 01       | C        | "Refer to issuer"              |
| DPS      | 02       | C        | "Refer to issuer - special"    |
| DPS      | 03       | C        | "Invalid merchant"             |
| DPS      | 04       | C        | "Pick up card"                 |
| DPS      | 05       | C        | "Do not honor"                 |
| DPS      | 06       | C        | "Do not honor" //NZ            |
| DPS      | 07       | C        | "Pick up card - special"       |
| DPS      | 08       | A        | "Accept with sig"              |
| DPS      | 10       | A        | "Approved" //NZ                |
| DPS      | 11       | A        | "Approved" //NZ                |
| DPS      | 12       | C        | "Invalid transaction"          |
| DPS      | 13       | C        | "Invalid amount"               |
| DPS      | 14       | C        | "Invalid card number"          |
| DPS      | 15       | C        | "No such issuer"               |
| DPS      | 16       | A        | "Approved" //NZ                |
| DPS      | 19       | B        | "Re-enter transaction"         |
| DPS      | 20       | C        | "Invalid response" //NZ        |
| DPS      | 30       | C        | "Format error"                 |
| DPS      | 31       | B        | "Card not supported"           |
| DPS      | 33       | C        | "Expired card"                 |
| DPS      | 34       | C        | "Expected Fraud"               |
| DPS      | 35       | C        | "Restricted Card"              |
| DPS      | 39       | C        | "No credit account"            |
| DPS      | 41       | C        | "Card reported lost"           |
| DPS      | 43       | C        | "Card reported stolen"         |
| DPS      | 51       | C        | "Insufficient funds"           |
| DPS      | 52       | C        | "Account error"                |
| DPS      | 54       | C        | "Expired card"                 |
| DPS      | 56       | C        | "Invalid card"                 |
| DPS      | 57       | C        | "Card not permitted"           |
| DPS      | 58       | C        | "Invalid txn"                  |
| DPS      | 59       | C        | "Suspected Fraud"              |
| DPS      | 60       | C        | "Call acquirer"                |
| DPS      | 61       | C        | "Above set limit"              |
| DPS      | 62       | C        | "Restricted Card"              |
| DPS      | 65       | C        | "Activity limit exceeded"      |
| DPS      | 67       | C        | "Declined(67)"                 |
| DPS      | 76       | C        | "Declined"                     |
| DPS      | 77       | A        | "Approved" //NZ                |
| DPS      | 91       | C        | "Issuer unavailable"           |
| DPS      | A5       | C        | "Declined(A5)"                 |
| DPS      | AF       | B        | "Transmission error"           |
| DPS      | AP       | B        | "Busy - try again"             |
| DPS      | AQ       | C        | "Amex not enabled"             |
| DPS      | BH       | C        | "Wrong currency"               |
| DPS      | BN       | C        | "Decline (BN)"                 |
| DPS      | BY       | B        | "Terminal busy"                |
| DPS      | CC       | C        | "Invalid DPSTxnRef"            |
| DPS      | D2       | C        | "No such user"                 |
| DPS      | D4       | C        | "User blocked"                 |
| DPS      | D5       | C        | "AU-NZ processor down"         |
| DPS      | D9       | C        | "Invalid merchant or password" |
| DPS      | J1       | C        | "Refund Ref mismatch"          |
| DPS      | J2       | C        | "No match for refund"          |
| DPS      | J4       | C        | "Already settled"              |
| DPS      | J5       | C        | "Already refunded"             |
| DPS      | J8       | C        | "No match for refund"          |
| DPS      | J9       | C        | "No matching debit"            |
| DPS      | NU       | C        | "Invalid card"                 |
| DPS      | O4       | C        | "No matching auth"             |
| DPS      | QG       | C        | "Invalid txn type"             |
| DPS      | QH       | C        | "Invalid expiry"               |
| DPS      | U9       | B        | "Timeout"                      |
| DPS      | Y2       | B        | "CBA network"                  |
| DPS      | YG       | C        | "Declined(YG)"                 |

#### ProfitStars Response Codes (PSTR)

| respproc | respcode | respstat | resptext                                                                                                                                                                                                                                                                                                                                                                                                                           |
|----------|----------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PSTR     | 00       | A        | "Success"                                                                                                                                                                                                                                                                                                                                                                                                                          |
| PSTR     | 01       | C        | "Duplicate Transaction"                                                                                                                                                                                                                                                                                                                                                                                                            |
| PSTR     | 02       | C        | "Declined"                                                                                                                                                                                                                                                                                                                                                                                                                         |
| PSTR     | 03       | C        | "Data Not Valid"                                                                                                                                                                                                                                                                                                                                                                                                                   |
| PSTR     | 04       | C        | "Velocity Count"                                                                                                                                                                                                                                                                                                                                                                                                                   |
| PSTR     | 05       | C        | "Velocity Amount"                                                                                                                                                                                                                                                                                                                                                                                                                  |
| PSTR     | 06       | C        | "AVS Declined"                                                                                                                                                                                                                                                                                                                                                                                                                     |
| PSTR     | 07       | C        | "CCV Declined"                                                                                                                                                                                                                                                                                                                                                                                                                     |
| PSTR     | 08       | C        | "Expired"                                                                                                                                                                                                                                                                                                                                                                                                                          |
| PSTR     | 09       | C        | "Authorizer Suppressed Date"                                                                                                                                                                                                                                                                                                                                                                                                       |
| PSTR     | 10       | C        | "Error Invalid Format"                                                                                                                                                                                                                                                                                                                                                                                                             |
| PSTR     | 11       | C        | "Error Timeout"                                                                                                                                                                                                                                                                                                                                                                                                                    |
| PSTR     | 12       | C        | "Error Internal"                                                                                                                                                                                                                                                                                                                                                                                                                   |
| PSTR     | 13       | C        | "Velocity amount"                                                                                                                                                                                                                                                                                                                                                                                                                  |
| PSTR     | 14       | C        | "Error Not Supported"                                                                                                                                                                                                                                                                                                                                                                                                              |
| PSTR     | 15       | C        | "Error Not Subscribed"                                                                                                                                                                                                                                                                                                                                                                                                             |
| PSTR     | 16       | C        | "Error Batch Closed"                                                                                                                                                                                                                                                                                                                                                                                                               |
| PSTR     | 17       | C        | "Error Invalid Batch"                                                                                                                                                                                                                                                                                                                                                                                                              |
| PSTR     | 18       | C        | "Error Invalid Terminal"                                                                                                                                                                                                                                                                                                                                                                                                           |
| PSTR     | 19       | C        | "Error Transaction Not Found"                                                                                                                                                                                                                                                                                                                                                                                                      |
| PSTR     | 20       | C        | "Error Terminal Disabled"                                                                                                                                                                                                                                                                                                                                                                                                          |
| PSTR     | 21       | C        | "Error Unspecified"                                                                                                                                                                                                                                                                                                                                                                                                                |
| PSTR     | 28       | C        | "Routing Number Not Found"                                                                                                                                                                                                                                                                                                                                                                                                         |
| PSTR     | 37       | C        | "Above max amount"                                                                                                                                                                                                                                                                                                                                                                                                                 |
| PSTR     | 40       | C        | "Not Subscribed to use this service"                                                                                                                                                                                                                                                                                                                                                                                               |
| PSTR     | 46       | C        | "The original transaction cannot have this operation performed on it at this time"                                                                                                                                                                                                                                                                                                                                                 |
| PSTR     | 99       | C        | This response code indicates that the processor encountered an exception during the authorization process (for example, an invalid routing number, or invalid account number length). <br> <br> The `resptext` returns a truncated version of the exception message, for example: <br> <br> "The RoutingNumber (nnnnnnnnn) is not a valid Routing Number." <br> "The AccountNumber (nnnnnn) must be between 4 and 17 digits long." |

#### Credorax Response Codes (CRDX)

| respproc | respcode | respstat | resptext                                |
|----------|----------|----------|-----------------------------------------|
| CRDX     | -9       | C        | "Parameter is malformed"                |
| CRDX     | 0        | A        | "Success"                               |
| CRDX     | 00       | A        | "Approval"                              |
| CRDX     | 1        | C        | "Denied"                                |
| CRDX     | 2        | C        | "Fraud high risk"                       |
| CRDX     | 02       | C        | "Refer to card issuer"                  |
| CRDX     | 03       | C        | "AVS high risk"                         |
| CRDX     | 04       | C        | "Interchange timeout"                   |
| CRDX     | 05       | C        | "Declined"                              |
| CRDX     | 7        | C        | "Redirect URL issue"                    |
| CRDX     | 08       | C        | "Time-Out"                              |
| CRDX     | 9        | C        | "Invalid card check digit"              |
| CRDX     | 13       | C        | "Invalid amount"                        |
| CRDX     | 14       | C        | "Invalid card number"                   |
| CRDX     | 54       | C        | "Expired card"                          |
| CRDX     | 59       | C        | "Suspected Fraud"                       |
| CRDX     | 76       | C        | "Invalid Account"                       |
| CRDX     | 83       | A        | "Not declined (zero amount validation)" |
| CRDX     | 89       | C        | "Security Violation"                    |
| CRDX     | 91       | C        | "Issuer not available"                  |
| CRDX     | 99       | C        | "Duplicate Transaction"                 |

#### Vantiv Response Codes (VANT)

| respproc | respcode | respstat | resptext                             |
|----------|----------|----------|--------------------------------------|
| VANT     | 00       | A        | "Approval"                           |
| VANT     | 02       | C        | "Refer to issuer"                    |
| VANT     | 03       | C        | "Invalid merchant"                   |
| VANT     | 04       | C        | "Pick up card"                       |
| VANT     | 05       | C        | "Do not honor"                       |
| VANT     | 06       | C        | "General error"                      |
| VANT     | 07       | C        | "Suspected fraud"                    |
| VANT     | 08       | A        | "Approval with ID"                   |
| VANT     | 10       | A        | "Partial Approval"                   |
| VANT     | 11       | A        | "VIP Approval"                       |
| VANT     | 12       | C        | "Invalid transaction"                |
| VANT     | 13       | C        | "Invalid amount"                     |
| VANT     | 14       | C        | "Invalid card number"                |
| VANT     | 15       | C        | "No such card issuer"                |
| VANT     | 17       | C        | "Customer cancellation"              |
| VANT     | 19       | B        | "Retry transaction"                  |
| VANT     | 23       | C        | "Bad fee amount"                     |
| VANT     | 27       | C        | "File update field edit"             |
| VANT     | 28       | C        | Update file temporarily unavailable" |
| VANT     | 30       | C        | "Format error"                       |
| VANT     | 33       | C        | "Expired card"                       |
| VANT     | 38       | C        | "PIN try limit exceeded"             |
| VANT     | 39       | C        | "No Credit Account"                  |
| VANT     | 41       | C        | "Pick up card - Lost"                |
| VANT     | 43       | C        | "Pick up card - Stolen"              |
| VANT     | 51       | C        | "Insufficient funds"                 |
| VANT     | 52       | C        | "No Checking Account"                |
| VANT     | 53       | C        | "No Savings Account"                 |
| VANT     | 54       | C        | "Expired card"                       |
| VANT     | 55       | C        | "Incorrect PIN"                      |
| VANT     | 56       | C        | "Cannot process"                     |
| VANT     | 57       | C        | "Invalid txn for card"               |
| VANT     | 58       | C        | "Invalid txn for acquirer"           |
| VANT     | 61       | C        | "Exceeds withdrawal limit"           |
| VANT     | 62       | C        | "Restricted card"                    |
| VANT     | 63       | C        | "Security violation"                 |
| VANT     | 65       | C        | "Exceeds Withdrawal Frequency Limit" |
| VANT     | 67       | C        | "Pick up card"                       |
| VANT     | 75       | C        | "PIN try limit exceeded"             |
| VANT     | 78       | C        | "No -To- Account Specified"          |
| VANT     | 79       | C        | "No -From- Account Specified"        |
| VANT     | 81       | C        | "PIN Key sync error"                 |
| VANT     | 82       | C        | "Invalid CVV"                        |
| VANT     | 83       | C        | "Unable to Verify PIN"               |
| VANT     | 85       | A        | "Approval"                           |
| VANT     | 88       | C        | "Card record not available"          |
| VANT     | 91       | C        | "Issuer inoperative"                 |
| VANT     | 92       | B        | "Unable to route transaction"        |
| VANT     | 93       | C        | "Illegal transaction"                |
| VANT     | 94       | C        | "Duplicate transaction"              |
| VANT     | 95       | C        | "Reconciliation error"               |
| VANT     | 96       | B        | "System malfunction"                 |
| VANT     | 97       | A        | "Amex rewards Approval"              |
| VANT     | 98       | C        | "Duplicate transaction"              |
| VANT     | 99       | C        | "Cannot be performed as debit"       |

## Visa Decline Category Codes
To help merchants properly handle declined transactions and avoid excessive reattempt fees, Visa provides additional Decline Category Codes in the authorization response. These codes indicate whether or not the merchant should reattempt the transaction, and why. The CardPointe Gateway returns these values, when present in, the the `declineCategory` and `declineCategoryText` fields in the authorization response. 

The following table describes each code, its corresponding text description, and some example cases.

> See the [Visa Decline Rules and Responses support article](https://support.cardpointe.com/compliance/visa-decline-rules-and-responses/) for detailed information.

| declineCategory | declineCategoryText | Reattempt Handling | Examples |
| --- | --- | --- | --- |
| 1   | The issuer will never approve. | In this case, you should **not** reattempt the transaction using the same account information; it will never be approved by the issuer. <br><br>Reattempting this transaction will incur a fee.<br><br>Instead, advise the cardholder to contact their issuer or provide an alternate payment method. | Some possible reasons for this code include:<br><br>*   The account has been closed.<br>    <br>*   The account has not been activated.<br>    <br>*   There is a stop payment order on this account. |
| 2   | The issuer cannot approve at this time. | In this case, you may wait and retry this transaction at a later time; the issuer may approve the transaction on a future attempt.<br><br>_**Note**_: You may reattempt this transaction a maximum of 15 times in a 30-day period before incurring excessive reattempt fees, unless you receive an updated **declineCategory** of **1**, in which case you should no longer reattempt this transaction. | Some possible reasons for this code include:<br><br>*   The account has exceeded its daily transaction limit.<br>    <br>*   The cardholder exceeded the number of allowable PIN entry attempts.<br>    <br>*   The account currently has insufficient funds.<br>    <br>*   A system error occurred while authorizing the transaction. |
| 3   | The issuer cannot approve based on details provided. | In this case, you can check the account information for errors, correct any invalid data, and reattempt the transaction using the corrected data; the issuer might approve the transaction if all errors are corrected.<br><br>_**Note**:_ You may reattempt this transaction a maximum of 15 times in a 30-day period before incurring excessive reattempt fees, unless you receive an updated **declineCategory** of **1**, in which case you should no longer reattempt this transaction. | Some possible reasons for this code include:<br><br>*   The account number is invalid.<br>    <br>*   The card is expired.<br>    <br>*   The CVV entered is incorrect.<br>    <br>*   The PIN entered is incorrect. |

# Mastercard Merchant Advice Codes

For declined recurring payments using a Mastercard or Visa account, the issuer returns a Merchant Advice Code, which the merchant can use in conjunction with the processor response to determine the appropriate course of action for handling the decline.

The CardPointe Gateway returns these values and their text descriptions in the <code>merchAdviceCode</code> and <code>merchAdviceText</code> fields, respecticely, in the authorization response.

The following table describes each code and corresponding text description and issuer.

|     |     |     |
| --- | --- | --- |
| Card Brand | merchAdviceCode | merchAdviceText |
| Mastercard | 01  | New account information available |
|  | 02  | Retry after 3 days |
|  | 03  | Account closed |
|  | 04  | Token requirements not fulfilled for this token type |
|  | 05  | Card account closed or fraud |
|  | 06  | Cardholder canceled recurring payment |
|  | 07  | Cancel specific payment |
|  | 21  | Stop recurring for this merchant |
|  | 22  | Merchant does not qualify for product code |
|  | 24  | Retry after 1 hour |
|  | 25  | Retry after 24 hours |
|  | 26  | Retry after 2 days |
|  | 27  | Retry after 4 days |
|  | 28  | Retry after 6 days |
|  | 29  | Retry after 8 days |
|  | 30  | Retry after 10 days |
| Visa | 02  | Cardholder blocked this payment |
|  | 03  | Cardholder stopped all recurring for this merchant |
|  | 21  | All recurring payments cancelled for this card |
|  | R0  | Stop payment order |
|  | R1  | Revocation of authorization order |
|  | R3  | Revocation of all authorizations order |

## AVS Response Codes for First Data Platforms

The most common AVS responses (`avsresp`) shared among the Rapid Connect, North, and Omaha processing platforms are categorized below.

> The data below is not a complete representation of all possible AVS response codes, nor representative of AVS response codes returned by other processors. AVS responses vary by card brand and processor.

#### Successful Verification

The following response codes indicate that both the street address and postal code have been verified.

| avsresp | Meaning	| Card Brand | Cardholder Region | Address Match | Postal Code Match
| --- | --- | --- | --- | --- | ---
| Y	| Yes	| All	| US | Yes | Yes, 5 digit ZIP
| X	| Exact Match	| Visa, Mastercard, Discover | US | Yes	| Yes, 9 digit ZIP
| F	| N/A	| Visa | UK | Yes	| Yes
| D	| N/A	| Visa | Non-US	| Yes	| Yes

#### Partially Successful Verification

The following response codes indicate that only the street address, or the postal code has been verified.

| avsresp	| Meaning	| Card Brand | Cardholder Region | Address Match | Postal Code Match
| --- | --- | --- | --- | --- | ---
| A	| Address Matches	| All	| All	| Yes	| No
| Z	| ZIP Matches | Visa, AMEX | All | No | Yes, 5 or 9 digit ZIP
| Z	| ZIP Matches	| Mastercard, Discover | All | No	| Yes, 5 digit ZIP
| W | Whole ZIP Matches	| Mastercard, Discover | US | No | Yes, 9 digit ZIP
| P | Postal Code Matches	| Visa | Non-US	| No, incompatible address provided | Yes

#### Unsuccessful Verification

The following response codes indicate that neither the street address nor the postal code could be verified.

| avsresp	| Meaning	| Card Brand | Cardholder Region | Address Match | Postal Code Match
| --- | --- | --- | --- | --- | ---
| N	| Nothing Matches	| All	| All	| No	| No

#### Unattempted Verification

| avsresp	| Meaning	| Card Brand | Cardholder Region | Description
| --- | --- | --- | --- | ---
| R	| Retry	| All	| All	| Address Verification System unavailable. Try again.
| S | Service not Supported | Mastercard, Amex, Discover | All | Issuer does not support address verification.
| U	| Unavailable	| Mastercard, Amex, Discover | All | Address information is not available from card issuer or authorization system.
| U | Unavailable	| Visa | US | Address information is not available from card issuer or authorization system.
| G	| N/A	| Visa, Discover | Non-US	| Address Verification System not able to verify international address, or no attempt was made.
| blank	| N/A	| All	| All	| A blank response indicates address verification was not attempted.

# Testing with Amount-Driven Response Codes

When testing your CardPointe Gateway or CardPointe Integrated Terminal integration in the UAT environment, you can use amount-driven response codes to emulate processor-specific authorization responses that you might encounter in the production environment. This allows you to receive and handle response codes that you would not otherwise encounter in your test environment.

See [Test Cases](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md#test-cases) in the [CardPointe Gateway API Developer Guides](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md) for detailed information on testing with amount-driven response codes and other useful test cases.
