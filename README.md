<h1>
  <img src="https://axeptio.imgix.net/2024/07/e444a7b2-ea3d-4471-a91c-6be23e0c3cbb.png" alt="Descrizione immagine" width="80" style="vertical-align: middle; margin-right: 10px;" />
  Axeptio React Native SDK Integration
</h1>

This repository demonstrates how to implement the **Axeptio React Native SDK** into your mobile applications to handle user consent for GDPR compliance and other privacy regulations.

The SDK is customizable for both [brands](https://support.axeptio.eu/hc/en-gb/articles/30431504788753-Geolocation) and [publishers](https://support.axeptio.eu/hc/en-gb/articles/23671149708945-1-Create-the-project), depending on your use case and requirements.

# 📑 Table of Contents
1. [GitHub Access Token Setup](#github-access-token-setup)
2. [Setup](#setup)
   - [Installation](#installation)
   - [Android Setup](#android-setup)
   - [iOS Setup](#ios-setup)
3. [Initialize the SDK on App Startup](#initialize-the-sdk-on-app-startup)
4. [ATT (App Tracking Transparency) Integration Note](#att-app-tracking-transparency-integration-note)




# GitHub Access Token Setup
When setting up your project or accessing certain GitHub services, you may be prompted to create a GitHub Access Token. Please note that generating this token requires:
- A valid **GitHub account**.
- **Two-factor authentication (2FA)** enabled.

To avoid authentication issues during setup, we recommend reviewing the [official GitHub Access Token Documentation](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) or detailed instructions on how to create your token and ensure 2FA is properly configured.
Following these steps will help you generate a token smoothly and reduce onboarding friction.

# 🔧Setup
## Installation
To install the **Axeptio React Native SDK**, run one of the following commands:
### Using npm:
```bash
npm install --save @axeptio/react-native-sdk
```
### Using yarn:
```bash
yarn add @axeptio/react-native-sdk
```
## Android Setup
- **Minimum SDK version**: 26.
- Add the **Maven GitHub repository** and **credentials** to your **app's** `android/build.gradle` (located at the root level of your `.gradle` file).
The snippet below is added to your `android/build.gradle` file, in the repositories block. It configures Gradle to pull the Axeptio Android SDK from a private GitHub repository.
```gradle
repositories {
    maven {
        url = uri("https://maven.pkg.github.com/axeptio/axeptio-android-sdk")
        credentials {
            username = "[GITHUB_USERNAME]"
            password = "[GITHUB_TOKEN]"
        }
    }
}
```

## iOS Setup
- We support **iOS versions >= 15**.
- Run the following command to install the dependencies:
```bash
npx pod-install
```
You can find a basic usage of the Axeptio SDK in the [example folder](https://github.com/axeptio/react-native-sdk/tree/master/example). Please check the folder for more detailed implementation examples.

# 🚀Initialize the SDK on App Startup
To initialize the **Axeptio React Native SDK** in your app, you need to set it up when your application starts. The SDK can be configured for different use cases, such as brands or publishers, using the `AxeptioService` enum. This allows you to customize the SDK according to your specific needs (e.g., handling GDPR consent for brands or publishers).

Here’s a step-by-step guide on how to initialize the SDK:
```javascript
import { AxeptioSDK, AxeptioService } from '@axeptio/react-native-sdk';

async function init() {
  try {
    // Initialize the SDK with the desired service (brands or publishers)
    await AxeptioSDK.initialize(
      AxeptioService.brands, // Choose between AxeptioService.brands or AxeptioService.tcfPublishers
      [your_client_id],      // Replace with your client ID (as provided by Axeptio)
      [your_cookies_version], // Replace with your current cookies version (as defined by your platform)
      [optional_consent_token] // Optional: If available, provide an existing consent token to restore previous consent choices
    );
  
    // Setup the user interface for consent management
    await AxeptioSDK.setupUI();

    console.log("Axeptio SDK initialized successfully!");
  } catch (error) {
    console.error("Error initializing Axeptio SDK:", error);
  }
}
```
## Parameters:
- `AxeptioService`: This enum defines the type of service you’re using:
   - `AxeptioService.brands`: Use this for brands and advertisers. This setup is typically used for compliance with GDPR and other privacy regulations for tracking user consent.
   - `AxeptioService.tcfPublishers`: Use this for publishers. It helps with managing user consent under the **Transparency and Consent Framework (TCF)** , which is common for handling GDPR consent in the context of digital publishing.
 - `your_client_id`: Replace this with your unique **client ID** provided by Axeptio when you set up your account. This ID identifies your organization or application in the system.
 - `your_cookies_version`: This is a version identifier for your cookies. It helps manage different cookie policies and consent flows over time. This value should be incremented when your cookie policy changes.
 - `optional_consent_token`: This token (if available) allows you to pass a previously stored consent token to restore the user's previous consent choices. This is optional, and you can skip it if it's not required.


# ATT (App Tracking Transparency) Integration Note:

The Axeptio SDK does not handle the user permission for tracking in the **App Tracking Transparency (ATT)** framework. It is the responsibility of your app to manage this permission and decide how the **Axeptio Consent Management Platform (CMP)** and the **ATT permission** should coexist.

## Apple Guidelines
Your app must comply with [Apple's guidelines](https://developer.apple.com/app-store/review/guidelines/#5.1.2) for disclosing the data collected by your app and requesting the user's permission for tracking.

To manage ATT, you can use the [react-native-tracking-transparency](https://www.npmjs.com/package/react-native-tracking-transparency) widget.

## Steps to Integrate ATT:

1. **Install the library**:
   You need to install the `react-native-tracking-transparency` library to handle ATT requests.

   Using npm:
   ```bash
   npm install --save react-native-tracking-transparency
 ```
Or using yarn:
```yarn
yarn add react-native-tracking-transparency
```
2. **Update `Info.plist`**:
Add the following key to your `Info.plist` file to explain why your app requires user tracking:
```xml
<key>NSUserTrackingUsageDescription</key>
<string>Explain why you need user tracking</string>
```
3. **Manage the ATT Popup**:
Before displaying the Axeptio CMP UI, you need to request ATT permission and handle the user's response. Here's how you can manage the ATT popup:
```javascript
import { getTrackingStatus, requestTrackingPermission } from 'react-native-tracking-transparency';

async function handleATT() {
  // Check the current tracking status
  let trackingStatus = await getTrackingStatus();

  // If status is not determined, request permission
  if (trackingStatus === 'not-determined') {
    trackingStatus = await requestTrackingPermission();
  }

  // If tracking is denied, update the Axeptio SDK with the user's decision
  if (trackingStatus === 'denied') {
    await AxeptioSDK.setUserDeniedTracking();
  } else {
    // If tracking is allowed, initialize the Axeptio SDK UI
    await AxeptioSDK.setupUI();
  }
}
```


