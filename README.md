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


