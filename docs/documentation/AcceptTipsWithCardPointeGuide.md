# Overview

This document provides an overview on how to best accept and process tips.

It is recommended to follow the below workflows and operations to ensure your and your customers' data remains secure.

> Some of the following workflows result in changing card-present transactions into card-not-present transactions. Due to this the rates and fees for accepting the transaction will differ. Workflows involving this change will be noted.
>
> Unless otherwise noted transactions will remain card-present.
>
> Learn more about the differences between these card-present and card-not-present transaction types by viewing our Card-Present and Card-Not-Present documentation.

# Tipping on Terminal

This workflow allows for seamlessly paying and tipping on a single terminal. This uses a single authorization on the card.

The workflow is as follows:

1. Call <code>connect</code> to establish a connection with the terminal.
2. Use <code>tip</code> to enable tipping on receipt.
3. Create an authCard request with <code>"capture" : "true"</code> to capture the transaction amount and finalize the transaction.
4. Terminate the connection with the terminal by using disconnect.

# Tipping on Receipt for Terminal Devices

The following methods result from the tip adjust setting being either enabled or disabled. Depending on this setting, the workflow will differ.

This workflow is to be used when accepting tips via a paper receipt after having already captured card information. 

## Tip Adjust Enabled

1. Call connect to establish a connection with the terminal.
2. Create an authCard request with "capture" : "true" to capture the transaction amount.
3. Terminate the connection with the terminal by using disconnect.
4. Allow the customer to fill out the tip line on the receipt
5. Use capture to add the tip amount if present otherwise original amount settles.

## Tip Adjust Disabled

> Transactions without tips will need to be captured for the original amount. These transactions will not settle by default.

1. Call connect to establish a connection with the terminal.
2. Create an authCard request with "capture" : "false" to capture the transaction amount.
3. Terminate the connection with the terminal by using disconnect.
4. Allow the customer to fill out the tip line on the receipt
5. Use capture to capture the added tipped or original amount.

# Closed Tab Tipping for CardPointe Web and Mobile Applications 

## CardPointe Mobile Procedure

> The following method results in changing the card-present transaction into a card-not-present transaction. Due to this the rates and fees for accepting the transaction will differ from card-present transactions. Learn more about card-present and card-not-present transaction types by viewing our Card-Present and Card-Not-Present documentation.

To accept immediate tips, you must first enable the **Tip At Time of Sale** setting located in the Virtual Terminal section under the **Administration** tab in the CardPointe Web Application. This will allow for both the CardPointe Mobile and CardPointe Web Applications to accept tips during a Virtual Terminal transaction.

> This procedure is only available for merchants who have the **Tip Adjust** ability. This ability can be granted to a merchant by enabling the **Tip Adjustment** Billpay Flag in CoPilot.

You must also enable the tipping ability in the CardPointe Mobile Application. To do this, navigate to the **Application** page under the **Settings** tab and enable **Restaurant-Style Gratuity**. This will result in printed receipts including two lines for the tip and the total amount after tipping for the customer to fill out.

<img src="../../assets/images/TipGuideMobileSettings.jpg" alt="CardPointe Mobile Settings Tab" width = "350"/>

After completing the above, the procedure for accepting tips is as follows.

1. Accept the customer's payment information via the CardPointe Mobile Device or CardPointe Mobile Application.
2. Print and provide the receipt to the customer.
3. Once the customer has filled out the tip and amount line, use the CardPointe Mobile Application to search for the transaction. To do this, navigate to the Transactions page under the Reporting tab. Then enter the retrieval reference number in the search bar and select your transaction.
4. Once your transaction is open, you can enter the amount into the Tip Adjust line and complete the transaction.

## CardPointe Web Application Procedure

> The following method results in changing the card-present transaction into a card-not-present transaction. Due to this the rates and fees for accepting the transaction will differ from card-present transactions. Learn more about the differences between these card-present and card-not-present transaction types by viewing our Card-Present and Card-Not-Present documentation.

To accept immediate tips, you must first enable the Tip At Time of Sale setting located in the Virtual Terminal section under the Administration tab in the CardPointe Web Application. This will allow for both the CardPointe Mobile and CardPointe Web Applications to accept tips during a Virtual Terminal transaction.

> This procedure is only available for merchants who have the Tip Adjust ability. This ability can be granted to a merchant by enabling the Tip Adjustment Billpay Flag in CoPilot.

To add tip lines to your receipts, navigate to the Receipts page under the Administration tab in the CardPointe Web Application and enable the Tip Lines option.
