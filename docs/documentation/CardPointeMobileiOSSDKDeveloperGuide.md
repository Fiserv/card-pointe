# Introduction

The CardPointe Mobile iOS SDK seamlessly connects your iOS applications to CardSecure for tokenization of customer card data. Tokens and other associated payment details are then retrieved by your server and securely transmitted to the CardPointe Gateway for authorization, using a server-side REST client. 

# What's New?

<!-- theme: warning -->
> See [Getting Started](#Getting-Started), below, to download the latest version of the CardPointe Mobile iOS SDK.

## Version 5.2

This version of the iOS SDK includes the following updates:

- The SDK is now an XCFramework. To integrate this version of the SDK, you must migrate your project to the new framework.

> See the XCFramework Migration Guide included in the SDK package for information on migrating your project to the new framework.

- Updated card brand support for the VP3300 to resolve Amex corporate purchase card declines.

- Improved EMV fallback handling.

- The SDK now includes a Bluetooth permission check on device connection. If the required permissions have not been granted, the error code `BMSSwiperErrorPermissionsNotGranted` is returned with the message "Required permissions denied".

- New contactless status messages `swiper:displayMessage:canCancel:` (for example, "Try Again with Chip").

- Updated EMV card read interaction now requires the user to remove the card to continue the transaction.

<!-- theme: warning -->
> Display the messages from `swiper:displayMessage:canCancel:` to notify the user to remove the card. You can also use `BMSSwiperController.beepSetting` to enable the VP3300 to produce a tone while waiting for card removal.

# Overview

A complete mobile payment integration consists of two components:

- Tokenization is handled by the CardPointe Mobile SDK integrated with your mobile application.
- Authorization is handled by host scripts integrated with your server application.

See the [CardPointe Mobile SDKs Developer Guide](?path=docs/documentation/CardPointeMobileSDKs.md) for detailed information on the overall solution, as well as example host scripts to help you get started with your server-side implementation.

## Supported Devices

If you are developing an application to accept card present payments, you must integrate a mobile payment reader (swiper) device with your solution. Currently, the CardPointe Mobile iOS SDK includes support for the ID TECH VP3300. 

The VP3300 is a Bluetooth-enabled mobile payment reader device that supports MSR (swipe) and EMV (chip) transactions. The VP3300 connects to your phone or tablet using Bluetooth 4.0, which supports Bluetooth Low Energy and automatic pairing.

For more information on using the VP3300, see the CardPointe Mobile Device User's Guide.

<!-- theme: danger -->
> The ID TECH VP3300 is only available for merchants processing on the First Data Rapid Connect platform.

# Getting Started

Download the latest version of the SDK to get started:

<!-- type: row -->

<!-- type: card
title: CardPoint Mobile iOS SDK v5.2 ZIP
-->

<!-- type: row-end -->

The CardPointe Mobile iOS SDK includes the following resources:

- **Docs** - Contains an API reference that provides a detailed description of each component of the SDK.
- **Framework** - Contains the SDK XCFramework and VP3300 .bundle file.
- **Integration Docs** - Contains additional SDK integration documentation.
- **SampleApp** - Contains an Objective-C sample application.
- **SwiftSampleApp** - Contains a Swift sample application.
- **ThemeGuide** - Contains a series of diagrams that illustrate the relationship of theme properties to UI elements in the sample application.

## Requirements

Before you begin, ensure that you have the following:

- **CardPointe Mobile iOS SDK** - See [Getting Started](#Getting-Started) to download the SDK.
- **Sample App** - Review the included sample applications to better understand the SDK integration.
- **CardPointe Merchant ID and API credentials** - Contact integrationdelivery@fiserv.com if you do not already have an account.
- **CardSecure Tokenization URL** - Your merchant ID must be associated with a CardSecure URL to send requests to CardSecure. 
- **Xcode IDE** - The CardPointe Mobile iOS SDK and related documentation support the Xcode integrated development environment.

> To support iOS 13, you must use [Xcode 11](https://developer.apple.com/documentation/xcode_release_notes/xcode_11_release_notes) and version 4.3 (or later) of the CardPointe Mobile iOS SDK.

- **(Optional) Mobile Payment Reader Device** - If you are integrating a mobile payment reader (swiper) device, you must us a device that has been pre-provisioned and provided for use with your merchant account.

<!-- theme: danger -->
> If you are using the ID TECH VP3300 device, your merchant account must be processing on the First Data Rapid Connect platform.

- **(Optional) iPhone or iPad Device** - If you are using a mobile payment reader (swiper) device, you will need a physical iPhone or iPad device to test the integration. The Xcode device emulator does not support these connected devices.

## Customer and Merchant-Facing Applications

The CardPointe Mobile iOS SDK supports both merchant and customer-facing applications. Depending on the type of application you are developing, you can include specific modules tailored to your specific audience and integration needs.

### Customer-Facing Applications

A customer user downloads and interacts with your application directly, without interacting with a merchant or participating in a card-present transaction.

### Merchant-Facing Applications

A merchant user interacts with the application as an extension of the integrated point-of-sale (POS) system, and might accept both card present and card not present payments.

### iOS SDK Features

The following table provides an overview of the features that you might want to integrate with your application, depending on your audience:

| Functionality | User Type | Component | Description |
| --- | --- | --- | --- |
| Stored Customer Profiles | Customer | [Integrated Customer Profile UI](#Integrating-The-Customer-Profile-UI) | Integrates the CardPointe Gateway Profile service, allowing customers to securely store and manage their payment accounts. <br> <br> See the CardPointe Gateway Profile service documentation for more information on the profile service. <br> <br> See [Integrating the Customer Profile UI](#Integrating-The-Customer-Profile-UI) for information on adding this feature to your application. |
| Apple Pay Wallet Support | Customer | [Apple Pay Integration](?path=docs/documentation/ApplePayDeveloperGuide.md) | Adds the option to use Apple Pay wallet credentials and stored payment methods. <br> <br> See the [Apple Pay Developer Guide](?path=docs/documentation/ApplePayDeveloperGuide.md) for more information. |
| Mobile Payment Reader Devices	| Merchant | [iOS Swiper Implementation](#Adding-Support-For-Mobile-Payment-Readers) | An integrated mobile payment reader (swiper) device used to securely capture and encrypt card data prior to tokenization. <br> <br> See [Supported Devices](#Supported-Devices) for information on the devices that are currently supported. <br> <br> See the [iOS SDK Integration Guide](#iOS-SDK-Integration-Guide) for instructions for integrating mobile payment reader devices. |

# iOS Sample App User's Guide

Before you begin your integration, you should review the sample app, which includes demos of the features provided by the SDK.

The sample app includes the following demos:

- **Configure Swiper** - Connect to an ID TECH VP3300 device.
- **Tokenization Methods** - Tokenize card data obtained from the swiper device, or manual entry.
- **Modal Profile Flow** - Create and manage stored customer profiles using the CardPointe Gateway Profile service.
- **Stack Profile Flow** - Create and manage stored customer profiles using the CardPointe Gateway Profile service.
- **Theming** - Customize the colors used throughout the app.
- **Signature** - Capture a signature in the format required for the CardPointe Gateway Signature Capture service.

## Tokenization URL

The sample app includes an editable URL field. This url is used to connect to CardSecure to tokenize the payment card data. Ensure that you enter the URL provided for your merchant account.

## Configure Swiper 

The configure swiper demo allows you to find and connect to a mobile payment reader (swiper) device that is properly configured and ready to connect to the app. 

> The phone or tablet cannot be connected to any other devices using Bluetooth or the 3.5mm headphone jack when attempting to use a swiper device.

To connect to a mobile payment reader, do the following:

**1.** Select IDTech VP3300 to connect to a VP3300 device.

    The app displays a list of available devices. 

**2.** Select a device to connect to it.

    The device is now connected and ready for use.

## Tokenization Methods

This demo allows you to manually enter or use a connected mobile payment reader device to capture and tokenize payment card data. 

### Using a Mobile Payment Reader

If a mobile payment reader device is connected, the app prompts you to insert or swipe a card. When you insert or swipe the payment card, the app captures the card data and immediately sends it to CardSecure. The app retrieves the generated token, and returns it in a confirmation message.

### Manually Entering Card Data

To manually enter card data, tap the input fields and enter the card information.

When finished, tap Generate Token to send the card data to CardSecure and generate a token. The app retrieves the generated token, and returns it in a confirmation message.

Additionally, you can select the following methods used to display the input in the Card Number field:

- **Masked Last4** - Masks each digit with the selected character (*, &, or -) except for the last four digits of the card number. For example, ************1111.
- **Last4** - hides each digit of the card number, displaying only the last four.  For example, 1111.
- **FirstandLast4** - Masks each digit with the selected character (*, &, or -), except for the first four digits and last four digits. For example, 4444********1111.

## Modal and Stack Profile Flows

The sample app includes two implementations of the Customer Profile UI, the Modal Profile View and the Stack Profile View. 

The **Modal Profile View** presents the Profile UI in a new modal object, which renders as a new window on top of the existing application window.

The **Stack Profile View** presents the Profile UI in a new stack in the existing application window.

Both versions of the demo include sample profile data, and the ability to add, edit, and delete profiles.

To add a new profile, do the following:

**1.** Tap **Add New Account** on the Accounts page.

**2.** Enter the Card Number and Expiration Date, and any optional details that you want to include in the profile. The Card Number and Expiration Date fields automatically format the input.

**3.** Tap **Done** then tap **Create Account** to save the new profile. 

To edit a profile, do the following:

**1.** Tap **Edit** on the Accounts page.

**2.** Tap the stored payment card that you want to edit.

    Alternatively, tap the delete icon to delete the stored payment card.

**3.** On the Edit Account page, you can edit the expiration date and set the stored card as the default payment method.  

**4.** Click **Save** to save your changes.

<!-- theme: warning -->
> When you integrate the SDK with your application, you can extend the Profile UI to edit the customer billing information and make an update profile request to the CardPointe Gateway.

## Theming

The sample app includes a theming demo, that allows you to change the colors used throughout the app to match your custom theme. 

The **ThemeGuide** folder, included in the SDK package, includes diagrams to help you map the theme parameters to their associated UI components.

## Signature

The sample app includes a demo interface for capturing signatures. 

The demo presents a touch input field that you can use to draw a signature. Click **Test** to capture the signature, or click **Clear** to clear the input. Note that the signature data is not stored in this demo.

# iOS SDK Integration Guide

This guide provides information for integrating the CardPointe Mobile iOS SDK with your application. 

<!-- theme: warning -->
> See the API reference documentation, included in the SDK package, for detailed information on using the classes and methods described in this guide.

## Integrating the SDK

The following topics provide information for integrating the CardPointe Mobile iOS SDK with your application, using the Xcode integrated development environment (IDE).

### Adding the Framework to your Project

Do the following to add the framework to your project.

**1.** Open your project in Xcode. 

**2.** In the left pane, select the **Project Navigator** tab, then click your project to open the Project Editor.

**3.** In the left pane of the Project Editor, select the target for your project.

**4.** Select the **General** tab, scroll down to the **Frameworks, Libraries, and Embedded Content** section and click the plus sign (+).

**5.** In the choose items to add window, click **Add Other…** and browse to **BoltMobileSDK.xcframework**. 

**6.** Select the framework, click **Open**, and then click **Finish**.

**7.** Ensure that the framework is listed as Embed & Sign.

### Configuring your Project

Do the following to configure your project:

**1.** Import the SDK into your prefix or source files:

    `#import <BoltMobileSDK/BoltMobileSDK.h>`

**2.** Set your endpoint on BMSAPI (for example, <host>.cardconnect.com)

    `[BMSAPI instance].endpoint = @”<endpoint provided for your account>”;`

## Adding Support for Mobile Payment Readers

If you are integrating support for a mobile payment reader (swiper) device, do the following to configure your project:

**1.** Select the **Info** tab on the Project Editor

**2.** Under **Custom iOS Target Properties**, hover over the table and click the plus sign (+) to add a new key.

**3.** Select **Privacy - Microphone Usage Description** key.

**4.** In the **Value** column, enter a description to display to users (for example, "Required for swiper usage").

### Adding Support for the VP3300 Device

To integrate an ID TECH VP3300, do the following to add the required bundle to your project:

**1.** In the **Project Navigator** (left) pane, right-click your project name and select **Add files to "<app name>"**.

**2.** Browse to the directory that includes the framework and add the **IDTech.bundle** file to your project at the path **<project directory>/BoltMobileSDK.xcframework/IDTech.bundle**.

**3.** Select the **Info** tab on the Project Editor

**4.** Under **Custom iOS Target Properties**, hover over the table and click the plus sign (+) to add a new key.

**5.** Select both the **Privacy - Bluetooth Always Usage Description** key and the **Privacy - Bluetooth Peripheral Usage Description** key.

**6.** In the **Value** column, enter a description to display to users (for example, "Required for the VP3300 swiper").

## Integrating the Mobile Payment Reader

To use a mobile payment reader (swiper) device, perform the following steps to integrate the device with your application:

**1.** Add the `BMSSwiperControllerDelegate` protocol to your view controller.

**2.** Add the required methods from `BMSSwiperControllerDelegate` and its super protocol `BMSSwiperDelegate` to your view:

    - `swiper:didGenerateTokenWithAccount:completion:`
    - `swiper:didFailWithError:completion:`
    - `swiper:foundDevices:`
    - `swiper:displayMessage:canCancel:`

**3.** Create a `BMSSwiperController` property in your view and initialize it in `viewWillAppear:` as follows:

    `self.swiper = [BMSSwiperController alloc] initWithDelegate:self swiper:{BMSSwiperType} loggingEnabled:YES];`

**4.** Release the swiper and set it to nil in `viewWillDisappear:` as follows:

    `[self.swiper releaseDevice];`

    `self.swiper = nil;`

    The swiper should initialize when the view appears. You can get the connection status using the optional methods in `BMSSwiperDelegate`.

**5.** If you are using a VP3300 device, continue to [Integrating a VP3300 Device](#Integrating-a-VP3300-Device).

## Integrating a VP3300 Device

The following topics provide information for integrating the ID TECH VP3300.

### Validating and Updating the Device Configuration

When the application connects to the VP3300, by calling `connectToDevice:mode:`, it validates the device's configuration. If this is the first time the device is connected, or if a new device configuration is available, the VP3300 automatically enters configuration mode. In configuration mode, the application updates the VP3300 with the current valid configuration. When the process completes, the device automatically begins waiting for card input.

<!-- theme: warning -->
> You can use the `swiper:configurationProgress` delegate callback to monitor the configuration process.

To save time in the payment flow, it is a best practice to configure a view that finds and connects to the device prior to accepting a payment, to ensure that the device is updated and ready for use. 

In this view, once the device is found using `[swiper findDevices];`, connect using `connectToDevice:mode:`. You can suppress the `displayMessage` callback. Additionally, when `swiperDidStartCardRead` is called you can cancel the transaction and check the error code for "canceled" in `swiper:didFailWithError:` to finish your connection flow. 

Note that you should delay the call to cancel the transaction to allow adequate time for the device time to initialize.  See `SwiperConfigurationViewController.m` or the .swift class in the sample application for an example of this workflow. 

### Selecting Card Read Methods

The SDK provides several modes to connect to a mobile payment reader device, depending on the types of card read methods you want to support.

- `BMSCardReadModeSwipeDipTap` - Enables the reader to accept swipe (MSR), dip/insert (EMV), and tap (contactless/NFC) card read methods.

- `BMSCardReadModeSwipeDip` to accept only swipe (MSR) and dip/insert (EMV) card read methods.

- `BMSCardReadModeSwipeTap` to accept only swipe (MSR) and tap (contactless/NFC) card read methods.

- `BMSCardReadModeSwipe` to force the reader to only accept the swipe (MSR) card read method. In this mode, attempting to insert a card will result in an error.

### Connecting the Device

Do the following to find and connect to the VP3300:

**1.** To start finding devices, call [self.swiper findDevices];.

    Found devices will be returned to swiper:foundDevices:.

**2.** Once you select a device, call [self.swiper cancelFindDevices];.

**3.** To connect to the device, call [self.swiper connectToDevice:device.uuid];.

    Once connected, the swiper will begin waiting for a card and swiper:displayMessage:canCancel will be called.

> All messages from `swiper:displayMessage:canCancel:` must be displayed. If the **cancelable** parameter is set to **true**, you can use `[self.swiper cancelTransaction];` to include a cancel function.

### Establishing a Persistent Connection

The SDK allows you to provides numerous connection methods, including establishing a persistent connection to prevent the device from disconnecting from the phone or tablet.

<!-- theme: danger -->
> Using a persistent connection will increase battery use for both the VP3300 and your iOS device.

To establish a persistent connection, you can create a `SwiperControllerManager` singleton class to manage `BMSSwiperController` for the duration of the application's use. This allows your application to call a single instance of the class to connect to the device in the background.

The `BMSSwiperController` class includes the following properties, which can be useful for implementing the `SwiperControllerManager` class as a singleton:

- `connectionState` - Tells your application if the swiper is connected.
- `cardReadTimeout` - Controls the timeout length for card reads. You can use this property to manage the delays in waiting for the card reader to initialize and accept a card. The maximum timeout value is 18 hours and 20 minutes.

#### Connecting to a Device

To connect to a device, `SwiperControllerManager` can use a stored UUID for a device that you previously found using the `BMSSwiperController findDevices` command. To connect using a stored UUID, call `swiperController.connect(ToDevice:uuid, mode:.swipeDip)`.

To connect to another device, call `swiperController.cancelFindDevices`, then call `swiperController.connect(toDevice:uuid, mode:.swipeDip)` using the UUID for the new device.

If the device is already waiting for card input, call `swiperController.cancelTransaction()` first. A card read is started on connect and continues when calling the completion block provided by the `BMSSwiperController` delegate methods.

> Your application can only use one instance of `BMSSwiperController` at a time.

#### Responding to Events

The `SwiperControllerManager` class should provide a delegate to forward events, such as card read data, tokens, and display messages, to the application.

For example:

`let swiperControllerManager = SwiperControllerManager.instanceswiperControllerManager.delegate = self`

When the swiper is used to read a card, `SwiperControllerManager` should respond to the following forwarded functions with their equivalent `BMSSwiperController` functions:

- `func swiperControllerManager(_ controller: SwiperControllerManager, displayMessage message: String){}`
- `func swiperControllerManager(_ controller: SwiperControllerManager, generatedTokenWith account: BMSAccount, completion: @escaping (() -> Void)){}`
- `func swiperControllerManager(_ controller: SwiperControllerManager, failedWithError error: NSError, completion: @escaping (() -> Void)){}){}`

## Integrating the Customer Profile UI

The SDK framework supports an integrated user interface for managing user accounts. The integrated UI allows you to use the CardPointe Gateway's profile service to add, delete, edit, and display a profile’s accounts with an easy to use interface. The integrated UI also includes a custom theming class that allows you to modify the look and feel.

To use the integrated UI, do the following:

**1.** Create a class that conforms to `BMSAPIBridgeProtocol`.

    This class calls the backend to perform the actions required by the UI.

**2.** Set up your root view controller.

    This will be the view that presents and responds to the integrated profile UI flow (for example, the screen that appears before the user selects a payment method).

**3.** In your root view controller, add strong properties for your class that conform to `BMSAPIBridgeProtocol` and `BMSPaymentController`. Additionally, if you want to use a custom theme, create a property for it.

**4.** In your root view controller, conform to the `BMSPaymentControllerDelegate` protocol and add `paymentController:finishedWithAccount:` to your controller.

**5.** In `viewDidLoad` Initialize the payment controller using either `initWithRootView:apiBridge:delegate:` or `initWithRootView:apiBridge:delegate:theme:`.

**6.** The payment controller supports two display methods: integrated with your current stack, or in a separate modal. Depending on which design you want to integrate, do the following:

    - To push the integrated profile UI flow onto your current navigation stack, call `[BMSPaymentController pushPaymentView]`.

        **Note**: This requires your root view to be contained within a navigation controller.

    - To present the integrated UI flow modally, call `[BMSPaymentController presentPaymentView]`.

    The integrated profile UI is now configured to display in the method you selected and use your API bridge class to supply data to the user. When the user finishes and selects a payment method, the controller is dismissed and the account is returned to your root view.

## Integrating Field Formatting and Validation

The SDK includes the following delegate classes, which provide functions for field formatting and validation:

- `BMSCardFunctions`
- `BMSCardFormatterDelegate`
- `BMSCVVFormatterDelegate`
- `BMSExpirationDateFormatterDelegate`

<!-- theme: warning -->
> See the API reference documentation, included in the SDK package, for detailed information on using these classes.

**1.** In the .xib file, where you collect card information, add an empty object to your controller and set its class to one of the delegates (for example, `BMSCardFormatterDelegate`).

**2.** Link your `UITextField` for card number to this class as its delegate.

    If you want callbacks for other `UITextFieldDelegate` calls sent to your view controller, link the `BMSCardFormatterDelegate` class `originalDelegate` property to your view controller.

**3.** If you want to modify the masking of the text field, set a reference outlet to your view controller as well.

    The text input field will now auto-format user input and provide validation using the `BMSCardFormatterDelegate` method `isValidCard`.

    To generate a token, use the `BMSCardFormatterDelegate` function `setCardNumberOnCardInfo:` to get the card number.

<!-- theme: warning -->
> When clearing text fields using the custom delegate classes, call `clearTextField` on the delegates themselves to clear internal information.

## Generating a Token

You use the `BMSCardInfo` and `BMSAPI` classes to send a payment card data to CardSecure in a tokenization request. See the API reference documentation, included in the SDK package, for detailed information on these classes.

**1.** Add a card number, expiration date, and CVV to your `BMSCardInfo` object.

**2.** Using your card info object call the `BMSAPI` function to generate a token as follows:

    `[BMSAPI instance] generateAccountForCard:<your card object> completion:^(BMSAccount *account, NSError *error){}];`

    A token is generated for the account and returned to your application.

**3.** Optionally, you can use `BMSAccount` to save an account for a profile on the CardPointe Gateway using the [iOS SDK Integrated Customer Profile UI](#Integrating-The-Customer-Profile-UI) or CardPointe Gateway API.
