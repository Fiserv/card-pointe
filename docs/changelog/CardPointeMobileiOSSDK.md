# CardPointe Mobile iOS SDK Changelog

The following entries describe changes to the [CardPointe Mobile iOS SDK](?path=docs/documentation/CardPointeMobileiOSSDKDeveloperGuide.md) and documentation.

## Version 5.2 

This version of the iOS SDK includes the following updates:

- The SDK is now an XCFramework. To integrate this version of the SDK, you must migrate your project to the new framework.

> See the _XCFramework Migration Guide_ included in the SDK package for information on migrating your project to the new framework.

- Updated card brand support for the VP3300 to resolve Amex corporate purchase card declines.

- Improved EMV fallback handling.

- The SDK now includes a Bluetooth permission check on device connection. If the required permissions have not been granted, the error code `BMSSwiperErrorPermissionsNotGranted` is returned with the message "Required permissions denied".

- New contactless status messages `swiper:displayMessage:canCancel:` (for example, "Try Again with Chip").

- Updated EMV card read interaction now requires the user to remove the card to continue the transaction.

<!-- theme: warning -->
> Display the messages from `swiper:displayMessage:canCancel:` to notify the user to remove the card. You can also use `BMSSwiperController.beepSetting` to enable the VP3300 to produce a tone while waiting for card removal.

## Version 5.1 

This version includes the following changes from this and previous builds:

### Support for Contactless (NFC) Payments

The SDK now supports contactless (NFC) payments using the ID TECH VP3300 card reader.

To integrate contactless support with your application, add the new `BMSSwiperController:` card read mode `BMSCardReadModeSwipeDipTap` or `BMSCardReadModeSwipeTap` to enable MSR/EMV/NFC or MSR/NFC card read methods as needed.

### Additional Fixes and Enhancements 

This update also includes the following fixes and enhancements:

- Added the `BMSSwiper` error `cardNotSupported`.

- Fixed an issue handling chip cards when no application identifier (AID) is present.

- Added a new force configure method, `connectToDevice:mode:forceConfig`, which skips the configuration check and forces a device configuration on startup. This method should only be used for debugging purposes.

## Version 4.3.4 

This version of the iOS SDK includes the following updates:

### New Required Properties for Apple Pay Requests 

The `CCCPaymentRequest` class now includes the following properties for use in Apple Pay requests:

- **companyName** - Your company name is required for all Apple Pay requests. Use the `companyName` property to provide your company name in the "PAY TOTAL" label (for example, "PAY MY COMPANY"). Alternatively, you can use the `paymentSummaryItems` property to provide your company name.
- _**Note**: If you do not provide your company name in the payment request, the Apple Pay option is not displayed by_ `CCCAccountListViewController`.

- **paymentSummaryItems** - You can use the `paymentSummaryItems` property to provide the company name and total values within the PKPaymentSummaryItem array instead of including the `total` and `companyName` properties separately.

### Enhanced Error Reporting 

The NSError object returned by `swiper:didFailWithError:completion:` now provides the `frameworkVersion` in the `userInfo`, if available.

## Version 4.3.3 

This version of the iOS SDK includes a fix to resolve the application hanging and requiring a restart in the event that the mobile payment reader is disconnected while establishing a connection.

## Version 4.3.2 

This version of the iOS SDK includes the following updates:

### Mobile Payment Reader Enhancements 

The SDK includes the following updates to improve the the mobile payment reader connection handling:

- **CCCSwiperErrorOtherAudioPlaying** - This new error message notifies the user that the mobile payment reader cannot connect because another application running on the device is using the audio connection.
- **CCCSwiperConnectionStateSearching** - This new connection state is set when the SDK is finding devices or attempting to connect to a device. Once the connection state changes to "connecting" you should not send any commands to the SDK until it has connected or disconnected.
- **swiperType** - This new `CCCSwiperController` property retrieves the `CCCSwiperType` of the current controller.

### Improved CVV Handling 

If your application captures the cardholder verification value (CVV), you must use the `CCCCardInfo` object to capture and tokenize the CVV with the card number. You should **not** send the CVV value separately to the CardPointe Gateway.

Improved Logging
Logs captured by the SDK are now prefixed with `CCCSDK:`.

Additionally, logs and connection state updates have been improved to reduce duplicate calls and more clearly indicate the status.

## Version 4.3.1 

This version of the iOS SDK includes a fix for VP3300 devices encountering errors when using languages other than English.

## Version 4.3 

This version of the iOS SDK includes the following updates:

### Support for iOS 13  

This version of the iOS SDK includes support for iOS 13. Note the following new requirements:

You must use xcode 11 to develop and build your application.
You must add the "Privacy - Bluetooth Always Usage Description" key to your project.

### VP3600 Support for MSR and EMV 

The `CCCSwiperType.VP3600` swiper type now supports the `CCCCardReadMode.SwipeDip` read mode.

## Version 4.2.2 

This version of the iOS SDK includes the following updates:

### New CCCSwiperError codes 

The `CCCSwiperError` constant includes the following new error codes:

| Error Code                       | Description |
|---------|----------------------------------|
| `CCCSwiperErrorConnectionError`    | Connection error  |
| `CCCSwiperErrorUnsupportedMode`    | Unsupported connection mode        |
| `CCCSwiperErrorBadCardRead`        | Swiped card was unable to be read. |
| `CCCSwiperErrorConfigurationError` | The device failed to configure.    |

See the `CCCSwiper` constant enumeration in the API reference for more information.

### New EMV Card Read Error Message 

Inserting an EMV card that cannot be read now returns the display message `“try ICC again”`

### New errorDetails  

The `userInfo` object now includes an `errorDetails` key/value pair that provides descriptive error messages for certain error codes.

### Expiration Date for EMV Card Reads 

Fixed a bug in which the expiration date was not returned for EMV cards.

## Version 4.2.1 

This version of the iOS SDK includes the following updates:

### Configurable Card Read Timeout 

You can use the `CCCSwiperController` setting  `cardReadTimeout` to configure the timeout duration for reading the card data. This value is in seconds and must be greater than "1." 

The maximum `cardReadTimeout` value depends on the device and the `currentReadMode` setting. For ID TECH devices, if the read mode is `swipe`, the max `cardReadTimeout` value is `0xFF`. All other devices and modes have a max value of `0xFFFF`. The default value is 60s. Attempting to set a higher value will result in the timeout not changing. Additionally, setting a read mode may reduce the timeout to the max value for that mode.

### Battery Level Warning for IDTECH Devices 

ID Tech devices now call `swiper:batteryLevelStatusHasChanged:` when the battery level falls below a set threshold. 

### IDTech.bundle Validation 

When initializing an IDTECH device, a warning message displays if the IDTech.bundle is not included in your application. Note that if the bundle is not included, display messages are not returned to your application.

### New currentReadMode Property 

The readonly currentReadMode property can be used to check the reader mode setting after the application connects to the device.

## Version 4.2 

This version of the iOS SDK includes the following update:

### Support for the ID TECH VP3600 (MSR Only) 

The iOS SDK now supports the ID TECH VP3600 mobile card reader device for MSR (swipe) only.

_**Note**: Support for EMV on the VP3600 is planned for a future update._ 

To connect to a VP3600 mobile card reader, use the swiper type `CCCSwiperTypeVP3600`.

## Version 4.1.2 

This release includes the following updates:

- Added a `beepSetting` property to the `CCCSwiperController` class to control the tone that the card reader device emits to prompt the user remove the card.

    The default setting is a single, 800ms long tone.

- A new enumeration has been added for the device beep setting called `CCCDeviceBeepSetting`.

## Version 4.1.1 

This release includes the following updates:

- The `connectToDevice:` method has been deprecated. Instead, use the new method, `connectToDevice:mode:`.
- A new enumeration has been added for device card read mode, `CCCCardReadMode`.
- The VP3300 can now support swipe-only input requests using `CCCCardReadModeSwipe`. 

    _**Note**: This also starts the contactless antenna which is not currently supported and will return an error. This will be supported in a future update._

## Version 4.1 

This version of the iOS SDK includes the following updates:

- New enumerations:
    - `CCCSwiperErrorConnectionError`
    - `CCCSwiperConnectionStateConfiguring`
- `swiperDidStartMSR:` has been renamed `swiperDidStartCardRead:`.
- The optional `CCCSwiperControllerDelegate` method `swiper:displayPrompt:options:completion:` has been removed.
- An optional `CCCSwiperControllerDelegate` method, `swiper:configurationProgress:` has been added to track the device configuration progress.

    The SDK will now check the configuration of IDTECH VP3300 devices when `connectToDevice:` is called. If the device needs to be configured it will be placed in the connection state `CCCSwiperConnectionStateConfiguring` and progress messages will be sent to swiper `swiper:configurationProgress`.

## Version 4.0 

This version of the iOS SDK includes the following updates:

- Removed the release framework; the SDK now includes a single universal framework.
- Added the strip-frameworks.sh script for stripping unnecessary framework components.
- Added support for ID TECH VP3300 mobile payment readers. See [Supported Devices](?path=docs/documentation/CardPointeMobileiOSSDKDeveloperGuide.md#supported-devices) for more information.
- Added the `CCCDevice` class to return a found Bluetooth device's name and identifier.
- The `CCCSwiperController` class includes the following updates:
    - Added the `CCCSwiperType` type definition enumeration that indicates the device type to be used.
    - Added new delegate methods for finding devices and displaying messages.
- The `CCCAccount` class includes a new `receiptData` property to return receipt data generated for EMV transactions.
