# CardPointe Integrated Terminal Application for Clover Terminals Changelog

The following entries describe changes to the [CardPointe Integrated Terminal application for Clover Terminals](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuideForCloverTerminals.md) and documentation, including updates to the Terminal API and to the Clover terminal application.

Visit status.cardconnect.com and click **subscribe to updates** to receive important release and status notifications.

## Date Updated: 10/13/2022 

Our Bolt family of integrated solutions is now **CardPointe Integrated Payments**.

On Thursday, October 13th, 2022, we released an update to the CardPointe Integrated Terminal Application for Clover terminals. This update includes the following changes:

- If you are using the default device wallpaper and theme, the Bolt wallpaper and logo will be updated to **CardPointe Integrated Terminal**.
- The Bolt App, Admin Menu and other UI elements will be updated to CardPointe.
- The **Bolted** and **Unbolted** statuses will be updated to **Connected** and **Disconnected**, respectively.

Terminals will automatically download and install this update during the nightly reboot window. No merchant action is required. 

For more information on using custom branding for your Clover terminals, see [Customizing the Clover Terminal](?path=docs/documentation/CardPointeIntegratedTerminalDeveloperGuideForCloverTerminals.md#customizing-the-clover-terminal).

## Date Updated: 4/12/2022 

An update to the CardPointe Integrated Terminal application for Clover terminals was released on April 14th, 2022.

This release includes internal enhancements and bug fixes.

## Date Updated: 2/5/2022 

An update to the CardPointe Integrated Terminal application for Clover terminals was released on February 5th, 2022, and pushed to terminals to update automatically.

This release includes internal enhancements and bug fixes.

## Date Updated: 4/17/2021 

A release of the CardPointe Integrated Terminal application for Clover terminals was deployed on 4/17/2021.

This release of the app includes the following updates:

- Support for the readCard request to tokenize customer cards on the terminal without running a transaction. 

- Support for printing duplicate receipts at the time of the transaction, using the new `printExtraReceipt` and `printDelay` parameters in the authCard or authManual request.

- Support for the terminalActivationCode request to retrieve the terminal activation code to display in your application when activating a new Clover terminal.

- Additional fixes and enhancements.

## Date Updated: 2/27/2021 

An update to the CardPointe Integrated Terminal application for Clover terminals was released to the UAT environment on 1/8/2021 and Production environment on 2/27/2021. This release includes numerous enhancements to the Terminal API. See [What's New? in the Terminal API](?path=docs/APIs/CardPointeIntegratedTerminalAPI.md#whats-new) for the release status and a complete list of updates.

This release includes the following updates:

- Support for reprinting receipts for previous transactions using the new printReceipt endpoint.
- Support for the `includeAVS` parameter in the authCard request, to capture the cardholder's postal code.
