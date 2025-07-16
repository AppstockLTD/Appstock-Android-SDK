# Appstock Android SDK - Integration

## Integration using dependency

In order to integrate Appstock SDK into your application, you should add the following dependency to
the `app/build.gradle` file and sync Gradle:

```groovy
dependencies {
    implementation("com.appstock:appstock-sdk:1.1.0")
}
```

Add this custom maven repository URL into the `project/settings.gradle` file:

```groovy
dependencyResolutionManagement {
    repositories {
        maven {
            setUrl("https://public-sdk.al-ad.com/android/")
        }
    }
}
```

Manual integration using AAR files

Copy AAR files to your Android module libs folder (f.e. app/libs/).

- [Core SDK](https://public-sdk.al-ad.com/android/com/appstock/appstock-sdk/1.1.0/appstock-sdk-1.1.0.aar)
- [Open Measurement SDK](https://public-sdk.al-ad.com/android/com/appstock/omsdk/1.5.0/omsdk-1.5.0.aar)
- [AdMob adapters](https://public-sdk.al-ad.com/android/com/appstock/appstock-sdk-google-mobile-ads-adapters/1.1.0/appstock-sdk-google-mobile-ads-adapters-1.1.0.aar)
- [AppLovin adapters](https://public-sdk.al-ad.com/android/com/appstock/appstock-sdk-applovin-adapters/1.1.0/appstock-sdk-applovin-adapters-1.1.0.aar)

Add dependencies to build.gradle file.
```groovy
implementation(files("libs/core-release.aar"))
implementation(files("libs/omsdk.aar"))

// Only for AdMob integration
implementation(files("libs/admob-adapters-release.aar"))

// Only for AppLovin integration
implementation(files("libs/applovin-adapters-release.aar"))
```

Integration using ARR files requires additional dependencies.  You should add ExoPlayer dependency for video ads and Google ads identifier dependency for better targeting.
```groovy

implementation 'com.google.android.exoplayer:exoplayer-core:2.15.1'
implementation 'com.google.android.exoplayer:exoplayer-ui:2.15.1'

implementation 'com.google.android.gms:play-services-base:18.1.0'
implementation 'com.google.android.gms:play-services-ads-identifier:18.0.1'

implementation "androidx.localbroadcastmanager:localbroadcastmanager:1.0.0"
```


## Initialization

Import the Appstock SDK core class in the main application class:

Kotlin:
```kotlin
import com.appstock.sdk.api.Appstock
```

Java:
```java
import com.appstock.sdk.api.Appstock;
```

Initialize Appstock SDK in the  `.onCreate()` method by calling `Appstock.initializeSdk()`.

Kotlin: 
```kotlin
class DemoApplication : Application() {
    override fun onCreate() {
        super.onCreate()

        // Initialize Appstock SDK
        Appstock.initializeSdk(this, PARTNER_KEY)
    }
}
```

Java: 
```java
public class DemoApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        // Initialize Appstock SDK
        Appstock.initializeSdk(this, PARTNER_KEY);
    }
}
```

The `Appstock.initializeSdk()` method has two parameters:

- **context** - the reference to the Application subclass instance

- **partnerKey** - determine the Appstock server URL. The Appstock account manager should provide you with this key.

It is recommended that contextual information be provided after initialization to enrich the ad requests. For this
purpose, use [SDK parametrization properties](6-sdk-android-parametrisation).

Once SDK is initialized and all needed parameters are provided, it is ready to request the ads.

If you want to see all requests made by the SDK and verbose logs, you should enable debug mode before the initialization. 

Kotlin:
```kotlin
Appstock.setDebugRequests(true)
Appstock.setLogLevel(Appstock.LogLevel.DEBUG)
Appstock.initializeSdk(this, PARTNER_KEY)
```

Java:
```java
Appstock.setDebugRequests(true);
Appstock.setLogLevel(Appstock.LogLevel.DEBUG);
Appstock.initializeSdk(this, PARTNER_KEY);
```
