# CardPointe Mobile Android SDK Developer Guide

The CardPointe Mobile Android SDK seamlessly connects your Android<sup>tm</sup> applications to CardSecure for tokenization of customer card data. Tokens and other associated payment details are then retrieved by your server and securely transmitted to the CardPointe Gateway for authorization, using a server-side REST client.

# What's New?

<!-- theme: warning -->
> See [Getting Started](#getting-started), below, to download the latest version of the CardPointe Mobile Android SDK.

## Version 3.0.86

This version includes the following changes from this and previous builds:

- To improve BBPOS Chipper Mini 2 connectivity, added a check for isConnected in the BBPOS swiper listener callback. The SDK will check that the device is connected and, if so, call `onConnected` to notify the app.
- Other minor fixes and improvements.

## Version 3.0.80

This version includes the following changes from this and previous builds:

- Added `onBusy()` to the device listener. When attempting to connect to the swiper, if it is in a “busy” state and unable to initiate a new connection, the SDK will call `onBusy()`.
- The SDK will call `onError()`, if the transaction is cancelled while processing (for example, if the card is removed in the middle of an EMV transaction). This presents an error to the calling application, which can then retry the transaction.

<!-- type: row -->

<!-- type: card
title: CardPointe Mobile Andriod SDK Changelog
description: Visit the Changelog for more information on recent updates to the CardPointe Mobile Android SDK and documentation
link:
-->

<!-- type: row-end -->

# Overview

A complete mobile payment integration consists of two components:

- Tokenization is handled by the CardPointe Mobile SDK integrated with your mobile application.
- Authorization is handled by host scripts integrated with your server application.

See the [CardPointe Mobile SDKs Developer Guide](?path=docs/documentation/CardPointeMobileSDKs.md) for detailed information on the overall solution, as well as example host scripts to help you get started with your server-side implementation.

## Supported Devices

If you are developing an application to accept card present payments, you must integrate a mobile payment reader (swiper) device with your solution. Currently, the CardPointe Mobile Android SDK includes support for the ID TECH VP3300.

The VP3300 is a Bluetooth-enabled mobile payment reader device that supports MSR (swipe) and EMV (chip) transactions. The VP3300 connects to your phone or tablet using Bluetooth 4.0, which supports Bluetooth Low Energy and automatic pairing. 

See the CardPointe Mobile Device User's Guide for more information on the ID TECH VP3300.

<!-- theme: danger -->
> The ID TECH VP3300 is only available for merchants processing on the First Data Rapid Connect platform.

> Support for NFC (contactless) transactions on the VP3300 is planned for a future update of the SDK.

# Getting Started

Download the latest version of the SDK to get started:

> [CardPointe Mobile Android SDK v3.0.86](../../assets/files/cardpointe-mobile-android-v3.0.86.zip)

For new integrations, see the [Android SDK Integration Guide](#android-sdk-integration-guide), below for detailed information on integrating the SDK with your application.

For migrating an existing app to the latest version of the SDK, see the [Android SDK Migration Guide](#andriod-sdk-migration-guide), below.

<!-- theme: warning -->
> The CardPointe Mobile Android SDK includes an API reference help file. Browse to **/docs/reference api** and double-click **help-doc.html** to launch the API help.

## Requirements

Before you begin, ensure that you have the following:

- **Android App Requirements** - The CardPointe Mobile Android SDK requires the following minimum software levels:
    - Android OS version 4.4 (KitKat) or later
    - Android SDK version 7 (API level 24) or later
- **Android Studio** - The Android SDK and related documentation support the Android Studio integrated development environment.
- **CardPointe Mobile Android SDK** - See [Getting Started](#getting-started) to download the SDK.
- **Sample App** - Review the included sample applications to better understand the SDK integration.
- **CardPointe Merchant ID and API credentials** - Contact integrationdelivery@fiserv.com if you do not already have an account.
- **CardSecure Tokenization URL** - Your merchant ID must be associated with a CardSecure URL to send requests to CardSecure.
- **(Optional) Mobile Payment Reader Device** - If you are integrating a mobile payment reader (swiper) device, you must us a device that has been pre-provisioned and provided for use with your merchant account.

<!-- theme: danger -->
> If you are using the ID TECH VP3300, your merchant account must be processing on the First Data Rapid Connect platform.

- **(Optional) Android Device** - If you are using a mobile payment reader (swiper) device, you will need a physical Android device to test the integration.

## Customer and Merchant-Facing Applications

The CardPointe Mobile SDK supports both merchant and customer-facing applications. Depending on the type of application you are developing, you can include specific modules tailored to your specific audience and integration needs.

### Customer-Facing Applications

A customer user downloads and interacts with your application directly, without interacting with a merchant or participating in a card-present transaction.

### Merchant-Facing Applications

A merchant user interacts with the application as an extension of the integrated point-of-sale (POS) system, and might accept both card present and card not present payments.

### Andriod SDK Features

The following table provides an overview of the features that you might want to integrate with your application, depending on your audience:

| Functionality | User Type | Component | Description |
| --- | --- | --- | --- |
| Stored Customer Profiles | Customer | [Integrated Customer Profile UI](#integrating-the-customer-profile-ui-optional) | Integrates the CardPointe Gateway Profile service, allowing customers to securely store and manage their payment accounts. <br> <br> See the CardPointe Gateway Profile service documentation for more information on the profile service. <br> <br> See [Integrating the Customer Profile UI](#integrating-the-customer-profile-ui-optional) for information on adding this feature to your application. |
| Mobile Payment Reader Devices	| Merchant | Android Swiper Implementation | An integrated mobile payment reader (swiper) device used to securely capture and encrypt card data prior to tokenization. <br> <br> See [Supported Devices](#supported-devices) for information on the devices that are currently supported. <br> <br> See the [Android SDK Integration Guide](#android-sdk-integration-guide) for instructions for integrating mobile payment reader devices. |

Additionally, you can integrate the Google Pay Android API to add Google Paytm support to your app. See the [Google Pay Developer Guide](?path=docs/documentation/GooglePayDeveloperGuide.md) for more information.

# Android SDK Sample APP

Before you begin your integration, you should review the sample app, which includes demos of the features provided by the SDK.

The sample app includes the following demos:

- **Configure Swiper** - Connect to an ID TECH VP3300 device.
- **Tokenization Methods** - Tokenize card data obtained from the swiper device, or manual entry.
- **Modal Profile Flow** - Create and manage stored customer profiles using the CardPointe Gateway Profile service.
- **Stack Profile Flow** - Create and manage stored customer profiles using the CardPointe Gateway Profile service.
- **Theming** - Customize the colors used throughout the app.
- **Signature** - Capture a signature in the format required for the CardPointe Gateway Signature Capture service.

## Tokenization URL

The sample app includes an editable URL field. This URL is used to connect to CardSecure to tokenize the payment card data. Ensure that you enter the URL provided for your merchant account.

# Android SDK Integration Guide

This guide provides information for integrating the CardPointe Mobile Android SDK with your application.

<!-- theme: warning -->
> See the API reference documentation for detailed information on using the classes and methods described in this guide. Browse to **/docs/reference api** and double-click **help-doc.html** to launch the API help.

> If you already integrated an older version of the Android SDK and want to migrate to the latest version, see the [Android SDK Migration Guide](#android-sdk-migration-guide), below.

## Importing the SDK to Your Project

Do the following to import the SDK with your project:

**1.** Download and unzip the SDK package.

**2.** Browse to the /lib folder and copy **boltsdk-release.aar**.

**3.** Paste the .aar file into the /app/libs folder in your project directory.

**4.** Launch Android Studio and open your project.

**5.** Do the following to import the SDK:
    - Click **File**, select **New**, then select **New Module**.
    - On the Create New Module dialog select **Import .JAR/.AAR Package** and click **Next**.
    - Browse to the **boltsdk-release.aar** file, then click **Finish**.

        A build.gradle file is created for the newly added module. Additionally, the settings.gradle file has an entry for both the main project and the newly added module. Each submodule has an entry in the settings.gradle file to tell the build system that the module is now available.

**6.** Do the following to make the module available to your project:
    - In the Project pane, select the **build.gradle** file for your app.
    - Add the following line to the dependencies section:

        `implementation files('libs/boltsdk-release.aar')`

        Additionally, the SDK requires the GSON and Android support libraries. The following example illustrates a complete list of dependencies :

        ```json
        dependencies {
            implementation fileTree(dir: 'libs', include: ['*.jar'])
            implementation 'com.android.support:appcompat-v7:28.0.0'
            testImplementation 'junit:junit:4.12'
            androidTestImplementation 'com.android.support.test:runner:1.0.2'
            androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
            implementation 'com.android.support:appcompat-v7:27.1.0'
            implementation 'com.android.support:design:27.1.0'
            implementation 'com.google.code.gson:gson:2.8.2'
            implementation files('libs/boltsdk-release.aar')
        }
        ```

**7.** Add the following to the manifest file:

```json
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.BLUETOOTH" />
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION />
```

<!-- theme: warning -->
> If you are using ProGuard with the R8 compiler to shrink and obfuscate your app, add the following line to your ProGuard rules file to keep the SDK:
>
> `-keep class com.bolt.consumersdk.** { *; }`

## Integrating the Customer Profile UI (optional)

The SDK supports an integrated user interface for managing customer account profiles. The profile UI integrates the CardPointe Gateway's profile service and allows your app to:

- List all stored accounts
- Delete specified accounts
- Update specified accounts
- Create an account when you generate a token using either manual entry or a mobile payment reader

The integrated UI also includes a custom theming class that allows you to modify the look and feel.

To integrate the Customer Profile UI, do the following:

**1.** Declare the following activities to allow the application to support custom themes.

```java
<!--Declare Consumer SDK Activities in order to be able to change themes-->
<activity android:name="com.bolt.consumersdk.views.payment.accounts.PaymentAccountsActivity"
    android:theme="@style/ConsumerAppImplementer.Theme" />
<activity android:name="com.bolt.consumersdk.views.payment.createaccount.CreateAccountActivity"
    android:theme="@style/ConsumerAppImplementer.Theme" />
<activity android:name="com.bolt.consumersdk.views.payment.editaccount.EditAccountActivity"
    android:theme="@style/ConsumerAppImplementer.Theme" />
```

**2.** Declare a specific theme that inherits from AppConsumerTheme.NoActionBar.

    See the Theme Guide included in the SDK package for details on specific theme configuration attributes.

```java
<style name="ConsumerAppImplementer.Theme" parent="AppConsumerTheme.NoActionBar">
    <!--//TODO Override Attributes-->
</style>
```

**3.** Implement the CCConsumerApiBridge interface class to populate the Payments UI flow and perform operations with accounts.

```java
public class ApiBridgeImpl implements CCConsumerApiBridge, Parcelable {
    //Parcelable implementation required for passing this object to Consumer SDK
    public static final Creator<ApiBridgeImpl> CREATOR = new Creator<ApiBridgeImpl>() {
        @Override
        public ApiBridgeImpl createFromParcel(Parcel in) {
            return new ApiBridgeImpl(in);
        }
        @Override
        public ApiBridgeImpl[] newArray(int size) {
            return new ApiBridgeImpl[size];
        }
    };
    public ApiBridgeImpl() {
    }
    protected ApiBridgeImpl(Parcel in) {
        //unused
    }
    @Override
    public void getAccounts(@NonNull final CCConsumerApiBridgeCallbacks apiBridgeCallbacks) {
        final CCConsumerApiBridgeGetAccountsResponse response = new CCConsumerApiBridgeGetAccountsResponse();
        //TODO Implement get Accounts from Third party server here
        //TODO provide result through apiBridgeCallbacks object
    }
    @Override
    public void saveAccountToCustomer(@NonNull final CCConsumerAccount account,
            @NonNull final CCConsumerApiBridgeCallbacks apiBridgeCallbacks) {
        final CCConsumerApiBridgeSaveAccountResponse response = new CCConsumerApiBridgeSaveAccountResponse();
        //TODO Implement add Account to Profile from Third party server here
        //TODO provide result through apiBridgeCallbacks object
    }
    @Override
    public void deleteCustomerAccount(@NonNull CCConsumerAccount accountToDelete,
            @NonNull final CCConsumerApiBridgeCallbacks apiBridgeCallbacks) {
        final CCConsumerApiBridgeDeleteAccountResponse response = new CCConsumerApiBridgeDeleteAccountResponse();
        //TODO Implement remove Account to Profile from Third party server here                //TODO provide result through apiBridgeCallbacks object
    }
    @Override
    public void updateAccount(@NonNull CCConsumerAccount account,
            @NonNull final CCConsumerApiBridgeCallbacks apiBridgeCallbacks) {
       //TODO Implement update Account to Profile from Third party server here
       //TODO provide result through apiBridgeCallbacks object
    }
    @Override
    public int describeContents() {
        return 0;
    }
    @Override
    public void writeToParcel(Parcel dest, int flags) {
        //unused
    }
}
```

**4.** Start PaymentsActivity and pass the CCConsumerApiBridge implementation object.

```java
ApiBridgeImpl apiBridgeImpl = new ApiBridgeImpl();
Intent intent = new Intent(this, PaymentAccountsActivity.class);
intent.putExtra(PaymentAccountsActivity.API_BRIDGE_IMPL_KEY, apiBridgeImpl);
startActivityForResult(intent, PaymentAccountsActivity.PAYMENT_ACTIVITY_REQUEST_CODE);
```

**5.** In addition to theme attributes, you can specify mask options using the CCConsumerCardFormatter together the CCConsumerApiBridge implementation class.

```java
ConsumerCardFormatter formatter = new CCConsumerCardFormatter();
formatter.setCCConsumerMaskFormat(CCConsumerMaskFormat.CARD_MASK_LAST_FOUR);
formatter.setMaskCharacter('*');
formatter.setCCConsumerExpirationDateSeparator(CCConsumerExpirationDateSeparator.DASH);
formatter.setCCConsumerCardMaskSpacing(CCConsumerCardMaskSpacing.CARD_MASK_SPACING_EVERY_CHARACTER);
intent.putExtra(PaymentAccountsActivity.CARD_FORMAT_OPTIONS_KEY,formatter);
startActivityForResult(intent, PaymentAccountsActivity.PAYMENT_ACTIVITY_REQUEST_CODE);
```

**6.** Receive the Account from the Payments UI if selected.

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    //Get Account selected by integrated UI flow
    if (requestCode == PaymentAccountsActivity.PAYMENT_ACTIVITY_REQUEST_CODE && resultCode == RESULT_OK) {
        //Example of displaying account selected
        CCConsumerAccount selectedAccount =
                data.getParcelableExtra(PaymentAccountsActivity.PAYMENT_ACTIVITY_ACCOUNT_SELECTED);
        Log.d(TAG, "Selected Account: " + selectedAccount.getAccountType().toString() + ", " +
                selectedAccount.getLast4());
    }
}
```

## Generating a Token using a Custom UI

The Android SDK enables your application to capture payment information and generate a token in either a customer-facing UI that supports manual entry, or a merchant-facing UI that supports manual entry and card reader devices.

**1.** Set the CardSecure URL you were provided.

    The SDK requires access to CardSecure to tokenize card information. 

    To set this URL in your application, call the `CCConsumer` class which provides a Singleton to facilitate communication with the API:

    `CCConsumer._getInstance_().getApi().setEndPoint(""https://url/to/tokenize/data"" );`
   
**2.** Implement `CCConsumerTokenCallback` to listen for events from the API.

```java
public class MainActivity extends AppCompatActivity implements CCConsumerTokenCallback {
    @Override
     protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout. activity\_main);
    }
    @Override
     public void onCCConsumerTokenResponseError(@NonNull CCConsumerError ccConsumerError) {
        //An error occurred trying to tokenize the data
        //Notify user of error
     }
    @Override
     public void onCCConsumerTokenResponse(@NonNull CCConsumerAccount ccConsumerAccount) {
        //Populate third party server request and send api call to proceed with the authorization.
     }}
    @Override
     protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout. **activity\_main** );
    }
```

**3.** Populate information to the `CCConsumerCardInfo` object using one of the following methods:

    - Use the following UI components to retrieve card data:
        - `CCConsumerCreditCardNumberEditText`
        - `CCConsumerCvvEditText`
        - `CCConsumerExpirationDateEditText`
          
        These components provide configurable format options. See the API reference in the **/docs/reference api** folder in the SDK package for details. 

        Card information through these components is not accessible from the application side, therefore you must pass the `CCConsumerCardInfo` object to every component to populate internally. Review the sample application for an example of these components in use.

    - Populate the `CCConsumerCardInfo` object manually.
      
        In this case, real card information should be set in the object.

**4.** Call `generateTokenWithCard` to generate a token using the data in the `CCConsumerCardInfo` object, as follows:

```java
CCConsumer._getInstance_().getApi().generateAccountForCard( **mCCConsumerCardInfo**** this**);
```

## Integrating a Mobile Payment Reader

The SDK supports integrated mobile payment reader devices, allowing your app to retrieve payment card data directly from device, and to pass that data to CardSecure for tokenization.

See [Supported Devices](#supported-devices) for information on currently supported mobile payment reader devices.

### Establishing a Persistent Connection for VP3300 Devices 

The SDK allows you to provides numerous connection methods, including establishing a persistent connection to prevent the device from disconnecting from the phone or tablet.

<!-- theme: danger -->
> Using a persistent connection will increase battery use for both the VP3300 and your Android device.

To establish a persistent connection, you can create a `SwiperControllerManager` singleton class to manage the swiper controller for the duration of the application's use. This allows your application to call a single instance of the class to connect to the device in the background.

The following example shows one possible implementation:

#### Example SwiperControllerManager Singleton Class

```java
package com.bolt.consumer.demoapp;

import android.content.Context;
import android.os.Handler;
import android.text.TextUtils;
import android.util.Log;

import com.bolt.consumersdk.CCConsumer;
import com.bolt.consumersdk.domain.CCConsumerAccount;
import com.bolt.consumersdk.domain.CCConsumerError;
import com.bolt.consumersdk.swiper.CCSwiperControllerFactory;
import com.bolt.consumersdk.swiper.SwiperController;
import com.bolt.consumersdk.swiper.SwiperControllerListener;
import com.bolt.consumersdk.swiper.enums.BatteryState;
import com.bolt.consumersdk.swiper.enums.SwiperCaptureMode;
import com.bolt.consumersdk.swiper.enums.SwiperError;
import com.bolt.consumersdk.swiper.enums.SwiperType;

public class SwiperControllerManager {
    public static String TAG = SwiperControllerManager.class.getSimpleName();
    private static final SwiperControllerManager mInstance = new SwiperControllerManager();
    private String mDeviceMACAddress = null;
    private SwiperController mSwiperController;
    private SwiperControllerListener mSwiperControllerListener = null;
    private SwiperCaptureMode mSwiperCaptureMode = SwiperCaptureMode.SWIPE_INSERT;
    private SwiperType mSwiperType = SwiperType.ID_TECH VP3300;
    private Context mContext = null;
    private boolean bConnected = false;

    public static SwiperControllerManager getInstance() {
        return mInstance;
    }

    private SwiperControllerManager() {

    }

    public void setContext(Context context) {
        mContext = context;
    }

    /***
     * Set bluetooth MAC Address of IDTECH Device
     * @param strMAC
     */
    public void setMACAddress(String strMAC) {
        boolean bReset = false;

        if (strMAC == null || !strMAC.equals(mDeviceMACAddress)) {
            bReset = true;
        }

        mDeviceMACAddress = strMAC;

        if (bReset) {
            connectToDevice();
        }
    }

    public String getMACAddr() {
        return mDeviceMACAddress;
    }

    /***
     *
     * @param swiperCaptureMode
     */
    public void setSwiperCaptureMode(SwiperCaptureMode swiperCaptureMode) {
        mSwiperCaptureMode = swiperCaptureMode;
    }

    /***
     *
     * @return
     */
    public SwiperCaptureMode getSwiperCaptureMode() {
        return mSwiperCaptureMode;
    }

    /***
     *
     */
    private void connectToDevice() {
        if (mSwiperType == SwiperType.IDTech && TextUtils.isEmpty(mDeviceMACAddress)) {
            return;
        }

        if (mContext == null || mDeviceMACAddress == null) {
            return;
        }

        if (mSwiperController != null) {
            disconnectFromDevice();

            new Handler().postDelayed(new Runnable() {
                @Override
                public void run() {
                    createSwiperController();
                }
            }, 5000);
        } else {
            createSwiperController();
        }
    }

    /***
     * Create a swiper controller based on the defined swiper type
     */
    private void createSwiperController() {
        SwiperController swiperController = null;

        swiperController = new CCSwiperControllerFactory().create(mContext, mSwiperType, new SwiperControllerListener() {
            @Override
            public void onTokenGenerated(CCConsumerAccount account, CCConsumerError error) {
                if (mSwiperControllerListener != null) {
                    mSwiperControllerListener.onTokenGenerated(account, error);
                }
            }

            @Override
            public void onError(SwiperError swipeError) {
                if (mSwiperControllerListener != null) {
                    mSwiperControllerListener.onError(swipeError);
                }
            }

            @Override
            public void onSwiperReadyForCard() {
                if (mSwiperControllerListener != null) {
                    mSwiperControllerListener.onSwiperReadyForCard();
                }
            }

            @Override
            public void onSwiperConnected() {
                bConnected = true;
                if (mSwiperControllerListener != null) {
                    mSwiperControllerListener.onSwiperConnected();
                }
            }

            @Override
            public void onSwiperDisconnected() {
                bConnected = false;
                if (mSwiperControllerListener != null) {
                    mSwiperControllerListener.onSwiperDisconnected();
                }
            }

            @Override
            public void onBatteryState(BatteryState batteryState) {
                if (mSwiperControllerListener != null) {
                    mSwiperControllerListener.onBatteryState(batteryState);
                }
            }

            @Override
            public void onStartTokenGeneration() {
                if (mSwiperControllerListener != null) {
                    mSwiperControllerListener.onStartTokenGeneration();
                }
            }

            @Override
            public void onLogUpdate(String strLogUpdate) {
                if (mSwiperControllerListener != null) {
                    mSwiperControllerListener.onLogUpdate(strLogUpdate);
                }
            }

            @Override
            public void onDeviceConfigurationUpdate(String s) {
                if (mSwiperControllerListener != null) {
                    mSwiperControllerListener.onDeviceConfigurationUpdate(s);
                }
                Log.d(TAG, "onDeviceConfigurationUpdate: " + s);
            }

            @Override
            public void onConfigurationProgressUpdate(double v) {

            }

            @Override
            public void onConfigurationComplete(boolean b) {

            }

            @Override
            public void onTimeout() {
                if (mSwiperControllerListener != null) {
                    mSwiperControllerListener.onTimeout();
                }
            }

            @Override
            public void onLCDDisplayUpdate(String str) {
                if (mSwiperControllerListener != null) {
                    mSwiperControllerListener.onLogUpdate(str);
                }
            }

            @Override
            public void onRemoveCardRequested() {
                if (mSwiperControllerListener != null) {
                    mSwiperControllerListener.onRemoveCardRequested();
                }
            }

            @Override
            public void onCardRemoved() {
                if (mSwiperControllerListener != null) {
                    mSwiperControllerListener.onCardRemoved();
                }
            }
        }, mDeviceMACAddress, false);

        if (swiperController == null) {
            //Connection to device failed.  Device may be busy, wait and try again.
            Handler handler = new Handler();
            handler.postDelayed(new Runnable() {
                @Override
                public void run() {
                    createSwiperController();
                }
            }, 5000);
        } else {
            mSwiperController = swiperController;
        }

        return;
    }

    /***
     * Disconnect from swiper device
     */
    public void disconnectFromDevice() {
        mSwiperController.release();
        mSwiperController = null;
    }

    /***
     *
     * @param swiperControllerListener
     */
    private void setSwiperControllerListener(SwiperControllerListener swiperControllerListener) {
        mSwiperControllerListener = swiperControllerListener;
    }

    /***
     *
     * @return true if swiper is connected.
     */
    public boolean isSwiperConnected() {
        return bConnected;
    }

    /***
     *
     * @return SwiperController Object
     */
    public SwiperController getSwiperController() {
        return mSwiperController;
    }

    /**
     * Provide a listener for the swiper contoller events
     * @param listener
     */
    public void setSwipeContolllerListener(SwiperControllerListener listener) {
        mSwiperControllerListener = listener;
    }

    /***
     *
     * @return the type of swiper supported by the current controller
     */
    public SwiperType getSwiperType() {
        return mSwiperType;
    }

    /***
     * Used to set define the type of swiper to create a controller for.  ID_TECH VP3300
     * @param swiperType
     */
    public void setSwiperType(SwiperType swiperType) {
        boolean bReset = false;

        if (mSwiperType != swiperType) {
            bReset = true;
        }

        mSwiperType = swiperType;
        setupConsumerApi();

        if (bReset || mSwiperController == null) {
            createSwiperController();
        }
    }

    /**
     * Initial Configuration for Consumer Api
     */
    private void setupConsumerApi() {
        switch (SwiperControllerManager.getInstance().getSwiperType()) {
            case ID_TECH VP3300:
                //CCConsumer.getInstance().getApi().setEndPoint("https://fts-uat.cardconnect.com");
                CCConsumer.getInstance().getApi().setEndPoint(mContext.getString(R.string.cardconnect_prod_post_url));
                break;
            case IDTech:
                //CCConsumer.getInstance().getApi().setEndPoint("https://fts-uat.cardconnect.com");
                CCConsumer.getInstance().getApi().setEndPoint(mContext.getString(R.string.cardconnect_qa_post_url));
                break;
        }
        CCConsumer.getInstance().getApi().setDebugEnabled(true);
    }
}
```

Before the application can connect to the device you must provide the following details:

- The singleton 
- the bluetooth MAC address for the device 
- the context for your application or activity 
- the swiper capture mode (DIP or SWIPE)

In the example above, the singleton has public methods (`setContext()`, `setMACAddress()` and `setSwiperCaptureMode()`) to allow you to set this information.

Additionally, the `IDTechSwiperController` class provides the following properties, which are useful in a singleton type implementation:

- The `isConnected` method specifies if the swiper is connected.
- The `timeout()` method controls the timeout length for card reads and has a maximum value of approximately 18 hours and 20 minutes. You can set this value to the desired threshold for fewer delays in waiting for the reader to be ready to accept a card.

### Connecting to a VP3300 Device

If you are using an ID TECH VP3300 mobile payment reader (swiper) device, do the following to find and connect to the device:

**1.** To start finding devices, call `CCConsumer.getInstance().getApi().startBluetoothDeviceSearch(BluetoothSearchResponseListener, Context);`.

    Found devices will be returned to `BluetoothSearchResponseListener.onDeviceFound`.
   
**2.** To connect to the device, call `CCSwiperControllerFactory().create(context, SwiperType, SwiperControllerListener, MacAddress)`

    Once connected, the swiper will begin waiting for a card and `SwiperControllerListener.onLogUpdate()` will be called.

### Getting a Token from a Mobile Payment Reader

To capture and tokenize card data using a mobile payment reader, use the `SwiperControllerListener` interface.

Do the following:

**1.** Instantiate an implementation of the SwiperControllerListener.

```
SwiperControllerListener swiperControllerListener = new SwiperControllerListener() {
        @Override
        public void onTokenGenerated(CCConsumerAccount ccConsumerAccount, CCConsumerError ccConsumerError) {
            //Dismiss loading indicator in case was shown from onStartTokenGeneration()
            //Check ccConsumerError object != null first in case there was an error.
            //Populate third party server request and send api call to proceed with the payment.
        }
        @Override
        public void onError(SwipeError s) {
            //SwiperControllerListener callback: A Toast or some sort of other visual indicator can be displayed in the
            //event of a reader error or other transaction-related error.
        }
        @Override
        public void onSwiperReadyForCard() {
            //SwiperControllerListener callback: It is recommended that a visual indicator be shown here
            //notifying that the reader is ready for a card swipe.
        }
        @Override
        public void onSwiperConnected() {
            //SwiperControllerListener callback: It is recommended that a visual indicator be shown here
        }
        @Override
        public void onSwiperDisconnected() {
            //SwiperControllerListener callback: It is recommended that a visual indicator be shown here
            // notifying that the reader was disconnected.
        }
        @Override
        public void onStartTokenGeneration() {
            //Should use this callback to show some sort of loading indicator.
        } 
        @Override
        public void onBatteryState(BatteryState batteryState) {
            //Should use this callback to notify the user swiper is running out of battery_
        }
    };
}
```

**2.** Instantiate a `SwiperController` object within the `onCreate()` method of the payment activity.

```
SwiperController mSwiper ;
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout. activity\_main );
     //Initialize the swiper using the CCSwiperControllerFactory.
     mSwiper = ( new CCSwiperControllerFactory()).create( this , SwiperType. ID_TECH VP3300 , mSwiperControllerListener );
     //Optional enable debugging to print debug info generated inside the swiper class
     mSwiper.setDebugEnabled( true );
}
```

**3.** When the activity corresponding to the payment screen is destroyed, the card reader resources must be cleaned up.

    In the `onDestroy()` lifecycle callback method, clean up the resources as follows:

```
@Override
protected void onDestroy() {
     super.onDestroy();
     //release() must be called in the Activity life cycle onDestroy() method
     mSwiper.release();
}
```

# Android SDK Migration Guide

If you have an existing integration using a previous version of the Android SDK, do the following to upgrade to the latest version.

## Integrating the New Framework

**1.** Remove the existing SDK, **ccconsumersdk-consumerSwiper-release.aar**, file from your project's /app/libs folder.

**2.** Copy the new SDK file, **boltsdk-release.aar**, to the /app/libs folder.

**3.** Launch Android Studio and open your project.

**4.** In the Project pane, select the **build.gradle** file for your app.

**5.** In the build.gradle file, replace all existing **ccconsumersdk-consumerSwiper-release.aar** SDK references with references to the new **boltsdk-release.aar** SDK.

**6.** Click **Build** and select **Clean** to clean the project.

**7.** Click **Build** and select **Rebuild Project** to rebuild the project.

**8.** Note all compile errors and rename old **com.cardconnect.consumersdk** import references with **com.bolt.consumersdk**. 

**9.** Update abstract methods to implement updated event handlers.

**10.** Click **Build** and select **Rebuild Project** to rebuild the updated project.

# Troubleshooting

## Troubleshooting Device Configuration Issues

If you or your merchants integrated the VP3300 CardPointe Mobile device with a version of the Android SDK prior to version 3.0.40, and have upgraded to version 3.0.61, you may need to update your devices' firmware or configuration if you are experiencing connection or tokenization issues.

<!-- theme: warning -->
> As of SDK version 3.0.40, the VP3300 CardPointe Mobile device must be running firmware v1.01.129 or higher

> New devices should not require firmware or configuration updates. Future configuration updates will be applied automatically.

### Checking the Firmware Version

To check the firmware version, using version 3.0.61 of the SDK, use a `SwiperControllerListener` to listen for `onDeviceConfigurationUpdate:` during the connection progress. This will return the firmware version of the device (for example, “Device firmware version: VP3300 Bluetooth NEO v1.01.129”). If your device is not running v1.01.129 or higher, contact isvintegrations@fiserv.com to update your device.

### Forcing a Configuration Update

If your device is running firmware 1.01.129 or higher, and you are still experiencing connection or tokenization issues, you can force a configuration update to attempt to resolve the issues. The SDK provides a force configuration flag, `ForceConfigUpdate`, when creating a `SwiperController` that you can use to reconfigure the device. 

<!-- theme: danger -->
> You should only enable `ForceConfigUpdate` for devices that were connected using a version of the SDK prior to 3.0.40, and are now experiencing issues when using a newer version of the SDK; new devices will automatically receive all configuration updates. Setting `ForceConfigUpdate` to true will result in longer connection times.
