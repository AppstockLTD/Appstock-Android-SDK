# Appstock Android SDK - Interstitial

To load and show interstitial ads, you should initialize, configure, and add the `AppstockInterstitialAdUnit` object to the app's layout and call the `loadAd()` method. Once the ad is loaded, you can invoke the `show()` method at any appropriate point of the app flow to present the fullscreen ad.

Kotlin:
```kotlin
private var adUnit: AppstockInterstitialAdUnit? = null

private fun createAd() {
    // 1. Create AppstockInterstitialAdUnit
    adUnit = AppstockInterstitialAdUnit(this)

    // 2. Configure ad unit
    adUnit?.setPlacementId(PLACEMENT_ID)
    adUnit?.setInterstitialAdUnitListener(createListener())
    adUnit?.setAdSizes(AppstockAdSize(320, 480))

    // 3. Load ad
    adUnit?.loadAd()
}
```

Java:
```java
private AppstockInterstitialAdUnit adUnit;

private void createAd() {
    // 1. Create AppstockInterstitialAdUnit
    adUnit = new AppstockInterstitialAdUnit(this);

    // 2. Configure ad unit
    adUnit.setPlacementId(PLACEMENT_ID);
    adUnit.setInterstitialAdUnitListener(createListener());

    // 3. Load ad
    adUnit.loadAd();
}
```

**Important note**: `setAdSizes()` should provide standard advertisement sizes, not the sizes of the screen.

It's important to destroy ad unit after leaving the screen. It cleans the resources and stops auto refresh.

Kotlin:
```kotlin
override fun onDestroy() {
    adUnit?.destroy()
}
```

Java:
```java
@Override
public void onDestroy() {
    super.onDestroy();
    if (adUnit != null) {
        adUnit.destroy();
    }
}
```

If you need to integrate video ads or multiformat ads, you should set the adFormats property to the respective value:

Kotlin:
```kotlin
// Make ad request for video ad
adUnit.setAdUnitFormats(EnumSet.of(AppstockAdUnitFormat.VIDEO))

// Make ad request for both video and banner ads (default behaviour)
adUnit.setAdUnitFormats(EnumSet.of(AppstockAdUnitFormat.BANNER, AppstockAdUnitFormat.VIDEO))

// Make ad request for banner ad
adUnit.setAdUnitFormats(EnumSet.of(AppstockAdUnitFormat.BANNER))
```

Java:
```java
// Make ad request for video ad
adUnit.setAdUnitFormats(EnumSet.of(AppstockAdUnitFormat.VIDEO));

// Make ad request for both video and banner ads (default behaviour)
adUnit.setAdUnitFormats(EnumSet.of(AppstockAdUnitFormat.BANNER, AppstockAdUnitFormat.VIDEO));

// Make ad request for banner ad
adUnit.setAdUnitFormats(EnumSet.of(AppstockAdUnitFormat.BANNER));
```

Once the ad is loaded, you can invoke the `show()` method at any appropriate point of the app flow to present the full-screen ad. To know when the ad is loaded, you should implement `AppstockInterstitialAdUnitListener` interface and subscribe to the ad events in its methods.

When the delegate’s method `onAdLoaded` is called, it means that the SDK has successfully loaded the ad. Starting from this point, you can call the `show()` method to display the full-screen ad.

Kotlin:
```kotlin
private fun createListener(): AppstockInterstitialAdUnitListener {
    return object : AppstockInterstitialAdUnitListener {
        override fun onAdLoaded(adUnit: AppstockInterstitialAdUnit, adInfo: AdInfo) {
            // Called when ad loaded
            Log.d(TAG, "Ad loaded successfully")

            // 4. Show ad
            adUnit.show()
        }

        override fun onAdDisplayed(adUnit: AppstockInterstitialAdUnit) {
            // Called when ad displayed full screen
            Log.d(TAG, "Ad displayed")
        }

        override fun onAdFailed(
            adUnit: AppstockInterstitialAdUnit,
            e: AppstockAdException,
        ) {
            // Called when ad failed to load or parse
            Log.e(TAG, "Ad failed to load: " + e.message, e)
        }

        override fun onAdClicked(adUnit: AppstockInterstitialAdUnit) {
            // Called when ad clicked
        }

        override fun onAdClosed(adUnit: AppstockInterstitialAdUnit) {
            // Called when ad closed
        }
    }
}
```

Java:
```java
private static AppstockInterstitialAdUnitListener createListener() {
    return new AppstockInterstitialAdUnitListener() {
        @Override
        public void onAdLoaded(AppstockInterstitialAdUnit adUnit, AdInfo adInfo) {
            // Called when ad loaded
            Log.d(TAG, "Ad loaded successfully");

            // 4. Show ad
            adUnit.show();
        }

        @Override
        public void onAdDisplayed(AppstockInterstitialAdUnit adUnit) {
            // Called when ad displayed full screen
            Log.d(TAG, "Ad displayed");
        }

        @Override
        public void onAdFailed(AppstockInterstitialAdUnit adUnit, AppstockAdException e) {
            // Called when ad failed to load
            Log.e(TAG, "Ad failed to load: " + e.getMessage());
        }

        @Override
        public void onAdClicked(AppstockInterstitialAdUnit adUnit) {
            // Called when ad clicked
        }

        @Override
        public void onAdClosed(AppstockInterstitialAdUnit adUnit) {
            // Called when ad closed
        }
    };
}
```

Once the ad is loaded you can utilize it's basic properties inspecting [AdInfo](6-sdk-android-utils.md) structure.

### Rendering Controls

The following properties enable rendering customization of video interstitial ads.

| Setter                  | Description                                                                                                                                                     |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| setIsMuted              | This option lets you switch the sound on or off during playback. Default is `false`.                                                                            |
| setCloseButtonArea      | This setting determines the percentage of the device screen that the close button should cover. Allowed range - `0...1`. Default value is `0.1`.                |
| setSkipButtonPosition   | This setting controls where the close button appears on the screen. Allowed values: `topLeft`, `topRight`. Other values will be ignored. Default is `topRight`. |
| setSkipButtonArea       | This setting determines the percentage of the device screen that the skip button should cover. Allowed range - `0...1`. Default value is `0.1`.                 |
| setSkipButtonPosition   | This control sets the position of the skip button. Allowed values: `topLeft`, `topRight`. Other values will be ignored. Default is `topLeft`.                   |
| setSkipDelay            | This setting determines the number of seconds after the start of playback before the skip or close button should appear. Default value is `10.0`.               |
| setIsSoundButtonVisible | This option switches on or off the visibility of the sound/mute button for users. Default value is `false`.                                                     |


Usage examples:

Kotlin:
```kotlin
adUnit.setSkipDelay(10)
adUnit.setSkipButtonPosition(AppstockPosition.TOP_RIGHT)
adUnit.setSkipButtonArea(0.2) 

adUnit.setCloseButtonPosition(AppstockPosition.TOP_RIGHT)
adUnit.setCloseButtonArea(0.2)

adUnit.setIsMuted(true) 
adUnit.setIsSoundButtonVisible(true)
```

Java:
```java
adUnit.setSkipDelay(10);
adUnit.setSkipButtonPosition(AppstockPosition.TOP_RIGHT);
adUnit.setSkipButtonArea(0.2);

adUnit.setCloseButtonPosition(AppstockPosition.TOP_RIGHT);
adUnit.setCloseButtonArea(0.2);

adUnit.setIsMuted(true); 
adUnit.setIsSoundButtonVisible(true);
```