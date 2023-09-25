# CardPointe Mobile Android SDK Changelog

The following entries describe changes to the [CardPointe Mobile Android SDK](?path=docs/documentation/CardPointeMobileAndroidSDKDeveloperGuide.md) and documentation.

## Version 3.0.86 

This version includes the following changes from this and previous builds:

- To improve BBPOS Chipper Mini 2 connectivity, added a check for isConnected in the BBPOS swiper listener callback. The SDK will check that the device is connected and, if so, call `onConnected` to notify the app.
- Other minor fixes and improvements.

## Version 3.0.80 

This version includes the following changes from this and previous builds:

- Added `onBusy()` to the device listener. When attempting to connect to the swiper, if it is in a “busy” state and unable to initiate a new connection, the SDK will call `onBusy()`.
- The SDK will call `onError()`, if the transaction is cancelled while processing (for example, if the card is removed in the middle of an EMV transaction). This presents an error to the calling application, which can then retry the transaction.

## Version 3.0.74 

This version includes the following changes from this and previous builds:

- Lowered the minimum required Android SDK version to 7.0 (API version 24). 
- Updated the swipe transaction flow to ignore `0x0003` responses.
- Updated the data format transmitted for fallback to MSR (swipe).
- Improved device connection stability.

## Version 3.0.71 

This version includes the following changes from this and previous builds:

- Updated the `setEndPoint` method to parse extra characters from the endpoint name.
- Replaced deprecated Bluetooth Adapter methods `startLEScan` and `stopLEScan` with `startDiscovery`.
- Added the ability to control the filtering of bluetooth device search results.
- Updated to Universal_SDK_1.00.135.jar.
- Added support for fallback transactions.
- Fixed a bug in which the device would crash when attempting to release a device that has not completed registration.

## Version 3.0.65 

This version of the SDK includes the following updates:

- The BBPOS controller has been updated to notify the `onTimeout` callback when the device times out.
- Fix added to prevent crashes encountered when calling `cancelTransaction` without the object being fully initialized.

## Version 3.0.63 

This version of the SDK includes an additional `BeepCommandType` value, `BMS_BEEP_COMMAND_TYPE_NONE`, which disables the beep sound on the VP3300.

## Version 3.0.62 

This version includes the following enhancement:

The SDK now includes a `setBeepSetting` command, which allows you to configure the beep settings for the VP3300. You can use the `setBeepSetting` command to specify one of the following `BeepCommandType` values:

- `BMS_BEEP_COMMAND_TYPE_200MSL` - One 200 millisecond long beep
- `BMS_BEEP_COMMAND_TYPE_400MSL` - One 400 millisecond long beep
- `BMS_BEEP_COMMAND_TYPE_600MSL` - One 400 millisecond long beep
- `BMS_BEEP_COMMAND_TYPE_800MSL` - One 400 millisecond long beep
- `BMS_BEEP_COMMAND_TYPE_SINGLE` - One short beep
- `BMS_BEEP_COMMAND_TYPE_DOUBLE` - Two short beeps
- `BMS_BEEP_COMMAND_TYPE_TRIPLE` - Three short beeps
- `BMS_BEEP_COMMAND_TYPE_QUADRU` - Four short beeps

See the API reference documentation in the SDK package for more information. Browse to **/docs/reference api** and double-click **help-doc.html** to launch the API help.

## Version 3.0.61 

This version includes the following changes from this and previous builds:

- The `CCConsumer.getInstance().getApi().setEndPoint()` method now only requires the base url (for example, `https://fts-uat.cardconnect.com`) instead of the full URL (for example, `https://fts-uat.cardconnect.com/cardsecure/cs`). Additional paths and parameters are appended to the URL.
- Updated `getEndPoint()` to only return the base URL.
- Added device firmware details to error messages in log.
- Changed the default transaction from $0 to $1.
- Optimized the reconnection logic to allow devices to reconnect more quickly.
- Added ability to programmatically set `CCConsumerCreditCardNumberEditText` using the `setText()` method.
- Added a check for success or failure when requesting device version information at the start of the auto config process.
- Added callbacks `onRemoveCardRequested`  and `CardRemoved`.
- Added the ability to auto reconnect the device after timeout or disconnect.
- Updated the EMV transaction flow to use the `GO_ONLINE` callback.
- Updated the auto config file.
- Added `LCD_Display` support.
- Changed the auto configuration to allow 0 as a valid version number.
- Added the `onTimeout` callback for notification of device transaction timeout.
- Updated the order of operations for device configuration updates.
- Changed the byte code sent after a `emv_completeTransaction()` call.
- Blocked usage of swiper modes (`SWIPE_TAP` and `SWIPE_TAP_INSERT`).
- Updated the auto config code to correctly check for the existing version information.
- Removed unnecessary `start_reader()` requests that might have caused swipers connection issues in some situations.
- Numerous bug fixes, including:
    - Fixed the `DFEE25` tag.
    - Fixed a bug preventing `onSwiperReadyForCard()` from being called.
    - Fixed a bug blocking functionality of `CCSwiperController.release()`.
    - Fixed a bug that caused issues reading the expiration date for American Express cards.
    - Fixed a bug that prevented connecting to a device that was turned off when the initial connection is requested.
    - Fixed the `“unexpected result”` error message.
    - Fixed bugs associated with BBPOS functionality and communication.
    - Fixed issues with auto reconnect after device goes to sleep mode.
    - Fixed a bug with auto config update.
    - Fixed a bug causing a main thread lock during device initialization.
    - Fixed a bug in the auto config update that prevented terminal data from being updated.
    - Fixed a bug in the auto config when reading factory reset devices.
    - Fixed a bug that blocked a `startReader` request once one had already been sent
    - Fixed a race condition that prevented the swiper from being initialized due to commands being sent while a device configuration version check is in progress.

## Version 3.0.33 

This version includes the following changes:

- Automatic device configuration for VP3300.
- MSR card swipes return encrypted card data.
- Added the `source=AndroidSDK` parameter to HTTP requests.
- Changed the base URL for requests to cardconnect.com.
- Added an enumerated connection status.
- Bug fixes and general enhancements.

## Version 3.0.28 

This version includes the following changes:

- The Android SDK has been rebranded and includes renamed project classes and related folders to align with the product change.
- Added sample code to the demo project for using autofill to populate credit card field information.
- Removed unnecessary files and folders from the SDK package.

## Version 3.0.22 

This version includes the following changes to the CardSecure tokenization request:

- Added support for the cardholder verification value (CVV). 
- Changed the HTTP request method from GET to POST.

## Version 3.0.16 

This version of the Android SDK fixed a bug that caused the main thread UI to hang.

## Version 3.0.15 

This version of the Android SDK added support for auto-filling cardholder information.

## Version 3.0.14 

This version of the Android SDK includes an updated gradle version to resolve application compiling issues.

## Version 3.0.13 

This version of the Android SDK fixed a build issue that was introduced in version 3.0.12.

## Version 3.0.12 

This version of the Android SDK fixed a bug that caused the application to crash when the mobile payment reader is released during a connection attempt.

## Version 3.0.11 

This version of the Android SDK added the ability to return the cardholder name and expiration date to the application.

## Version 3.0.10 

This version of the Android SDK fixed a bug that bricked some ID TECH VP3300 devices while attempting to make a transaction.

## Version 3.0.9 

This version of the Android SDK fixed a bug that caused the application to hang if a new transaction is started while completing a previous transaction.

## Version 3.0.8 

This version of the Android SDK removes the default card removal beep.

## Version 3.0.6 
This version of the Android SDK includes updated ID TECH libraries used by the SDK for the ID TECH VP3300.

## Version 3.0.5 

This version of the Android SDK includes the following updates:

- Updated targetSdkVersion to version 9. 
- Updated targetApiVersion to version 28.

## Version 3.0.4 

This version of the Android SDK includes the following updates:

- Fixed memory leaks.
- Improved ease of debugging and testing.
- Reordered regex checks.
- Reduced 8 second delay when SDK is initialized.
- For the ID TECH VP3300, added a message to prompt the user to remove the card after an EMV transaction is complete.
- Resolved an issue causing device connections to hang in the “processing” phase.
- Fixed app crashes related to releasing a handle to the bluetooth device.
- Removed extraneous classes from the SDK package.
- Removed deprecated legacy app USB hardware requirements.
- Added a version info printout to the log when the SDK initializes.

## Version 3.0 

This version of the Android SDK includes support for ID TECH VP3300 mobile payment readers. See [Supported Devices](?path=docs/documentation/CardPointeMobileAndroidSDKDeveloperGuide.md#supported-devices) for more information.
