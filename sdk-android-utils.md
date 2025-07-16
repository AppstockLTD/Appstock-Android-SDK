# Appstock Android SDK - Utils

## Appstock Ad Info

The class AdInfo provides additional information about the received ad. Currently, it provides the ad price and later
this object will be extended.

This object is provided to the loading listeners for all ad unit types.

Kotlin:

```kotlin
object : AppstockAdViewListener {
    override fun onAdLoaded(adView: AppstockAdView, adInfo: AdInfo) {
        val adPrice = adInfo.getPrice()
    }
}
```

Java:

```java
AppstockAdViewListener listener = new AppstockAdViewListener() {
    @Override
    public void onAdLoaded(AppstockAdView adView, AdInfo adInfo) {
        Double adPrice = adInfo.getPrice();
    }
};
```