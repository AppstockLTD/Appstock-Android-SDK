# Appstock Android SDK - Overview

## Overview

Appstock SDK is a native library that monetizes Android applications.

The latest SDK version is **1.0.0**.

The minimum supported Android version: **Android 5.0** (API level **21**)

[Demo applications (Kotlin, JAVA)](https://public-sdk.al-ad.com/android/appstock-demo/demo-app-1.0.0/).

## Integration and configuration

Follow the [integration instructions](sdk-android-integration.md) to add the SDK to your app. Once the SDK is
integrated, you can provide [configuration options](6-sdk-android-parametrisation.md) that will help increase your
revenue. Keep in mind that the SDK supports basic [consent providers](7-sdk-android-consents.md) according to industry
standards.

Appstock SDK supports the following ad formats:

- [Banner](sdk-android-banner.md) (HTML + Video)
- [Interstitial](sdk-android-interstitial.md) (HTML + Video)
- [Native](sdk-android-native.md)

The SDK can be integrated directly into your app or via supported Mediation Adapters:

- [AppLovin MAX](9-sdk-android-applovin.md)
- [GMA SDK](8-sdk-android-admob.md) (AdMob, GAM)

# Appstock Android SDK - Integration

## Integration using dependency

In order to integrate Appstock SDK into your application, you should add the following dependency to
the `app/build.gradle` file and sync Gradle:

```groovy
dependencies {
    implementation("com.appstock:appstock-sdk:1.0.0")
}
