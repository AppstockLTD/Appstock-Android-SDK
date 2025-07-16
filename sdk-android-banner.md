# Appstock Android SDK - Banner

To load and show banner ads, you should initialize, configure, and add the `AppstockAdView` object to the app's layout and call the `loadAd()` method.

Kotlin:

```kotlin
private var adView: AppstockAdView? = null

private fun createAd() {
    // 1. Create AppstockAdView
    val adView = AppstockAdView(this).also { this.adView = it }

    // 2. Configure ad unit
    adView.setPlacementId(PLACEMENT_ID)
    adView.setAdSizes(AppstockAdSize(WIDTH, HEIGHT))
    adView.setAdViewListener(createListener())
    adView.autoRefreshDelay = 30

    // 3. Load ad
    adView.loadAd()

    // 4. Add AppstockAdView to the app UI
    containerForAd.addView(adView)
}
```

Java:
```java
private AppstockAdView adView;

private void createAd() {
    // 1. Create AppstockAdView
    adView = new AppstockAdView(this);

    // 2. Configure ad unit
    adView.setPlacementId(PLACEMENT_ID);
    adView.setAdSizes(new AppstockAdSize(WIDTH, HEIGHT));
    adView.setAutoRefreshDelay(30);
    adView.setAdViewListener(createListener());

    // 3. Load ad
    adView.loadAd();

    // 4. Add AppstockAdView to the app UI
    getContainerForAd().addView(adView);
}
```

The `AppstockAdView` should be provided with one of the required configuration properties:

- `setPlacementId()` - Unique placement identifier generated on the Appstock platform's UI.
- `setEndpointId()` - Unique endpoint identifier generated on the Appstock platform's UI.

Which one to use depends on your type of Appstock account.

**Important note**: `setAdSizes()` should provide standard advertisement sizes, not the sizes of the screen.  

It's important to destroy ad view after leaving the screen. It cleans the resources and stops auto refresh. Or you can just stop auto refresh using `stopAutoRefresh()`.

Kotlin:
```kotlin
override fun onDestroy() {
    adView?.destroy()
}
```

Java:
```java
@Override
public void onDestroy() {
    super.onDestroy();
    if (adView != null) {
        adView.destroy();
    }
}
```

If you need to integrate video ads, you can also use the `AppstockAdView` object in the same way as for banner ads. The single required change is you should explicitly set the ad format via the respective property:

Kotlin:
```kotlin
adView.setAdUnitFormat(AppstockAdUnitFormat.VIDEO)
```

Java:
```java
adView.setAdUnitFormat(AppstockAdUnitFormat.VIDEO);
```

Once it is done, the Appstock SDK will make ad requests for video placement and render the respective creatives.

Additionally, you can set more parameters for better advertisement targeting.

Kotlin:
```kotlin
adView.setAdPosition(AppstockBannerAdPosition.HEADER)
adView.setVideoPlacementType(AppstockVideoPlacementType.IN_BANNER) // Only for video ad unit format
```

Java:
```java
adView.setAdPosition(AppstockBannerAdPosition.HEADER);
adView.setVideoPlacementType(AppstockVideoPlacementType.IN_BANNER); // Only for video ad unit format
```

You can optionally subscribe to the ad’s lifecycle events by implementing the `AppstockAdViewListener` interface:

Kotlin:
```kotlin
private fun createListener(): AppstockAdViewListener {
    return object : AppstockAdViewListener {
        override fun onAdLoaded(adView: AppstockAdView, adInfo: AdInfo) {
            // Called when ad loaded
            Log.d(TAG, "Ad loaded successfully")
        }

        override fun onAdFailed(adView: AppstockAdView, e: AppstockAdException) {
            // Called when ad failed to load or parse
            Log.e(TAG, "Ad failed to load: " + e.message)
        }

        override fun onAdDisplayed(adView: AppstockAdView) {
            // Called when ad displayed
        }

        override fun onAdClicked(adView: AppstockAdView) {
            // Called when ad clicked
        }

        override fun onAdClosed(adView: AppstockAdView) {
            // Called when ad closed
        }
    }
}
```

Java:
```java
private static AppstockAdViewListener createListener() {
    return new AppstockAdViewListener() {
        @Override
        public void onAdLoaded(AppstockAdView AppstockAdView, AdInfo adInfo) {
            // Called when ad loaded
            Log.d(TAG, "Ad loaded successfully");
        }

        @Override
        public void onAdFailed(AppstockAdView AppstockAdView, AppstockAdException e) {
            // Called when ad failed to load
            Log.e(TAG, "Ad failed to load: " + e.getMessage());
        }

        @Override
        public void onAdDisplayed(AppstockAdView AppstockAdView) {
            // Called when ad displayed on screen
        }

        @Override
        public void onAdClicked(AppstockAdView AppstockAdView) {
            // Called when ad clicked
        }

        @Override
        public void onAdClosed(AppstockAdView AppstockAdView) {
            // Called when ad hidden
        }
    };
}
```

Once the ad is loaded you can utilize it's basic properties inspecting [AdInfo](6-sdk-android-utils.md) structure.

Or you can subscribe to the video ad events (only for video ad unit format).

Kotlin:
```kotlin
private fun createListener(): AppstockAdViewVideoListener {
    return object : AppstockAdViewVideoListener {
        override fun onVideoCompleted(bannerView: AppstockAdView?) {
            Log.d(TAG, "Video completed")
        }

        override fun onVideoPaused(bannerView: AppstockAdView?) {
            Log.d(TAG, "Video paused")
        }

        override fun onVideoResumed(bannerView: AppstockAdView?) {
            Log.d(TAG, "Video resumed")
        }

        override fun onVideoUnMuted(bannerView: AppstockAdView?) {
            Log.d(TAG, "Video unmuted")
        }

        override fun onVideoMuted(bannerView: AppstockAdView?) {
            Log.d(TAG, "Video muted")
        }

    }
}
```

Java:
```java
private static AppstockAdViewVideoListener createListener() {
    return new AppstockAdViewVideoListener() {

        @Override
        public void onVideoCompleted(AppstockAdView teqBlazeAdView) {
            Log.d(TAG, "Video completed");
        }

        @Override
        public void onVideoPaused(AppstockAdView teqBlazeAdView) {
            Log.d(TAG, "Video paused");
        }

        @Override
        public void onVideoResumed(AppstockAdView teqBlazeAdView) {
            Log.d(TAG, "Video resumed");
        }

        @Override
        public void onVideoUnMuted(AppstockAdView teqBlazeAdView) {
            Log.d(TAG, "Video un muted");
        }

        @Override
        public void onVideoMuted(AppstockAdView teqBlazeAdView) {
            Log.d(TAG, "Video muted");
        }
    };
}
```
