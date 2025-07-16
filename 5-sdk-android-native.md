# Appstock Android SDK - Native

To load a native ad, you should initialize and configure `AppstockNativeAdUnit` object and call the `loadAd()` method.

Kotlin:

```kotlin
private var adUnit: AppstockNativeAdUnit? = null

private fun createAd() {
    // 1. Create AppstockNativeAdUnit
    adUnit = AppstockNativeAdUnit()

    // 2. Configure ad unit with native config
    adUnit?.setPlacementId(PLACEMENT_ID)
    adUnit?.setNativeAdConfig(createNativeConfig())

    // 3. Load ad
    adUnit?.loadAd { result: AppstockNativeResult ->
        val nativeAd = result.nativeAd
        if (nativeAd == null) {
            Log.e("AdExample", "Native ad is null: " + result.status)
            return@loadAd
        }

        Log.d(TAG, "Native ad loaded successfully")
        // 4. Create native view
        createNativeView(nativeAd)
    }
}

private fun createNativeConfig(): AppstockNativeAdConfig {
    val eventTrackingMethods = ArrayList(
        Arrays.asList(
            NativeEventTracker.EventTrackingMethod.IMAGE,
            NativeEventTracker.EventTrackingMethod.JS
        )
    )
    val eventTracker = NativeEventTracker(
        NativeEventTracker.EventType.IMPRESSION,
        eventTrackingMethods
    )

    val title = NativeTitleAsset()
    title.setLength(90)
    title.isRequired = true

    val icon = NativeImageAsset(20, 20, 20, 20)
    icon.imageType = NativeImageAsset.ImageType.ICON
    icon.isRequired = true

    val mainImage = NativeImageAsset(200, 200, 200, 200)
    mainImage.imageType = NativeImageAsset.ImageType.MAIN
    mainImage.isRequired = true

    val sponsored = NativeDataAsset()
    sponsored.len = 90
    sponsored.dataType = NativeDataAsset.DataType.SPONSORED
    sponsored.isRequired = true


    val description = NativeDataAsset()
    description.dataType = NativeDataAsset.DataType.DESC
    description.isRequired = true

    val ctaText = NativeDataAsset()
    ctaText.dataType = NativeDataAsset.DataType.CTATEXT
    ctaText.isRequired = true

    val assets = Arrays.asList(
        title,
        icon,
        mainImage,
        sponsored,
        description,
        ctaText
    )

    return AppstockNativeAdConfig.Builder()
        .setContextType(NativeContextType.SOCIAL_CENTRIC)
        .setPlacementType(NativePlacementType.CONTENT_FEED)
        .setContextSubType(NativeContextSubtype.GENERAL_SOCIAL)
        .setNativeEventTrackers(listOf(eventTracker))
        .setNativeAssets(assets)
        .build()
}
```

Java:

```java
private AppstockNativeAdUnit adUnit;

private void createAd() {
    // 1. Create AppstockNativeAdUnit
    adUnit = new AppstockNativeAdUnit();

    // 2. Configure ad unit with native config
    adUnit.setPlacementId(PLACEMENT_ID);
    adUnit.setNativeAdConfig(createNativeConfig());

    // 3. Load ad
    adUnit.loadAd((result) -> {
        AppstockNativeAd nativeAd = result.getNativeAd();
        if (nativeAd == null) {
            Log.e("AdExample", "Native ad is null: " + result.getStatus());
            return;
        }

        Log.d(TAG, "Native ad loaded successfully");
        // 4. Create native view
        createNativeView(nativeAd);
    });
}

private AppstockNativeAdConfig createNativeConfig() {
    ArrayList<NativeEventTracker.EventTrackingMethod> eventTrackingMethods = new ArrayList<>(
            Arrays.asList(
                    NativeEventTracker.EventTrackingMethod.IMAGE,
                    NativeEventTracker.EventTrackingMethod.JS
            )
    );
    NativeEventTracker eventTracker = new NativeEventTracker(
            NativeEventTracker.EventType.IMPRESSION,
            eventTrackingMethods
    );

    NativeTitleAsset title = new NativeTitleAsset();
    title.setLength(90);
    title.setRequired(true);

    NativeImageAsset icon = new NativeImageAsset(20, 20, 20, 20);
    icon.setImageType(NativeImageAsset.ImageType.ICON);
    icon.setRequired(true);

    NativeImageAsset mainImage = new NativeImageAsset(200, 200, 200, 200);
    mainImage.setImageType(NativeImageAsset.ImageType.MAIN);
    mainImage.setRequired(true);

    NativeDataAsset sponsored = new NativeDataAsset();
    sponsored.setLen(90);
    sponsored.setDataType(NativeDataAsset.DataType.SPONSORED);
    sponsored.setRequired(true);


    NativeDataAsset description = new NativeDataAsset();
    description.setDataType(NativeDataAsset.DataType.DESC);
    description.setRequired(true);

    NativeDataAsset ctaText = new NativeDataAsset();
    ctaText.setDataType(NativeDataAsset.DataType.CTATEXT);
    ctaText.setRequired(true);

    List<NativeAsset> assets = Arrays.asList(
            title,
            icon,
            mainImage,
            sponsored,
            description,
            ctaText
    );

    return new AppstockNativeAdConfig.Builder()
            .setContextType(NativeContextType.SOCIAL_CENTRIC)
            .setPlacementType(NativePlacementType.CONTENT_FEED)
            .setContextSubType(NativeContextSubtype.GENERAL_SOCIAL)
            .setNativeEventTrackers(Collections.singletonList(eventTracker))
            .setNativeAssets(assets)
            .build();
}
```

Once the ad is loaded you can utilize it's basic properties inspecting [AppstockNativeAd.adInfo](6-sdk-android-utils.md)
structure.

## AppstockNativeAdConfig

The class responsible for configuration native ad parameters. Here is a brief description of parameters for the builder:

- **`setNativeAssets`** - an array of assets associated with the native ad.

- **`setNativeEventTrackers`** - an array of event trackers used for tracking native ad events.

- **`setContextType`** - the context type for the native ad (e.g., content, social).

- **`setContextSubType`** - a more detailed context in which the ad appears.

- **`setPlacementType`** - the design/format/layout of the ad unit being offered.

- **`setPlacementCount`** - the number of identical placements in this layout. Default is `1`.

- **`setSequence`** - the sequence number of the ad in a series. Default is `0`.

- **`setAUrlSupport`** - whether the supply source / impression impression supports returning an assetsurl instead of
  an asset object. Default is `0` (unsupported).

- **`setDUrlSupport`**  - whether the supply source / impression supports returning a dco url instead of an asset object.
  Default is `0` (unsupported).

- **`setPrivacy`**  - set to 1 when the native ad support buyer-specific privacy notice. Default is `0`.

- **`setExt`**  - a dictionary to hold any additional data as key-value pairs.

Once the ad is loaded, the SDK provides you with a `AppstockNativeAd` object in the callback of the `loadAd()` method.
This object contains ad assets that you should apply to the native ad layout.



## Assets configuration


### NativeTitleAsset

Request asset for the advertisement title. Parameters:

- `length` - the length of the title.
- `required` - flag whether the field is mandatory. 
- `ext` - additional json data. 

Kotlin:

```kotlin
    val title = NativeTitleAsset()
    title.setLength(90)
    title.isRequired = true
```

Java:

```java
    NativeTitleAsset title = new NativeTitleAsset();
    title.setLength(90);
    title.setRequired(true);
```


### NativeDataAsset

Request asset for any text data. Parameters:

- `length` - the length of the data.
- `type` - the type of data asset (e.g., sponsored, description).
- `required` - flag whether the field is mandatory.
- `ext` - additional json data.

Kotlin:

```kotlin
    val sponsored = NativeDataAsset()
    sponsored.len = 90
    sponsored.dataType = NativeDataAsset.DataType.SPONSORED
    sponsored.isRequired = true
```

Java:

```java
    NativeDataAsset sponsored = new NativeDataAsset();
    sponsored.setLen(90);
    sponsored.setDataType(NativeDataAsset.DataType.SPONSORED);
    sponsored.setRequired(true);
```

Available data types:

- `SPONSORED` - represents sponsored content.
- `DESC` - represents a description.
- `RATING` - represents a rating.
- `LIKES` - represents likes.
- `DOWNLOADS` - represents download count.
- `PRICE` - represents the price.
- `SALEPRICE` - represents a sale price.
- `PHONE` - represents a phone number.
- `ADDRESS` - represents an address.
- `DESC2` - represents a secondary description.
- `DESPLAYURL` - represents a display URL.
- `CTATEXT` - represents call-to-action text.
- `CUSTOM` - represents a custom data asset. You can set custom exchange id. 


### ImageDataAsset

Request asset for image. In the example below we request ad with desired size 200x200, and minimal size: 30x30. Parameters:

- `imageType` - the type of image asset (e.g., icon, main image).
- `width`, `height` - the desired size of the image.
- `minWidth`, `minHeight` - the minimum allowed size of the image. 
- `mimes` - an array of supported MIME types for the image.
- `required` - flag whether the field is mandatory.
- `ext` - additional json data.

Kotlin:

```kotlin
    val mainImage = NativeImageAsset(200, 200, 30, 30)
    mainImage.imageType = NativeImageAsset.ImageType.MAIN
    mainImage.isRequired = true
    mainImage.addMime("image/jpeg")
```

Java:

```java
    NativeImageAsset mainImage = new NativeImageAsset(200, 200, 200, 200);
    mainImage.setImageType(NativeImageAsset.ImageType.MAIN);
    mainImage.setRequired(true);
    mainImage.addMime("image/jpeg")
```

Available data types:

- `ICON` - represents an icon image asset.
- `MAIN` - represents a main image asset.
- `CUSTOM` - represents a custom image asset.


## Native event tracking

You can also specify what type of event tracking is supported. For that you need to set `setEventTrackers` setter.

Kotlin:
```kotlin
    val eventTrackingMethods = ArrayList(
        Arrays.asList(
            NativeEventTracker.EventTrackingMethod.IMAGE,
            NativeEventTracker.EventTrackingMethod.JS
        )
    )
    val eventTracker = NativeEventTracker(
        NativeEventTracker.EventType.IMPRESSION,
        eventTrackingMethods
    )
```

Java:
```java
    ArrayList<NativeEventTracker.EventTrackingMethod> eventTrackingMethods = new ArrayList<>(
            Arrays.asList(
                    NativeEventTracker.EventTrackingMethod.IMAGE,
                    NativeEventTracker.EventTrackingMethod.JS
            )
    );
    NativeEventTracker eventTracker = new NativeEventTracker(
            NativeEventTracker.EventType.IMPRESSION,
            eventTrackingMethods
    );
```

The event method configures desired tracking method:
- `Impression` - represents an impression event.                                                                   
- `ViewableImpression50` - represents a 50% viewable impression event.                                                    
- `ViewableImpression100` - represents a 100% viewable impression event.                                                 
- `ViewableVideoImpression50` - represents a 50% viewable video impression event.                                         
- `Custom` - represents a custom event type.         

The event type configures desired tracking type:
- `Image` - represents image-based event tracking.                                                            
- `JS` - represents JavaScript-based event tracking.                                                       
- `Custom` - represents a custom tracking method.


## Native view for the ad

Once the ad is loaded, the SDK provides you with a `AppstockNativeAd` object in the callback of the `loadAd()` method. This object contains ad assets that you should apply to the native ad layout.

Kotlin:

```kotlin
private fun createNativeView(ad: AppstockNativeAd) {
    val nativeContainer = View.inflate(this, R.layout.layout_native, null)

    val icon = nativeContainer.findViewById<ImageView>(R.id.imgIcon)
    ImageUtils.download(ad.iconUrl, icon)

    val title = nativeContainer.findViewById<TextView>(R.id.tvTitle)
    title.text = ad.title

    val image = nativeContainer.findViewById<ImageView>(R.id.imgImage)
    ImageUtils.download(ad.imageUrl, image)

    val description = nativeContainer.findViewById<TextView>(R.id.tvDesc)
    description.text = ad.description

    val cta = nativeContainer.findViewById<Button>(R.id.btnCta)
    cta.text = ad.callToAction

    containerForAd.addView(nativeContainer)

    ad.registerView(nativeContainer, Lists.newArrayList(icon, title, image, description, cta), createListener())
}
```

Java:

```java
private void createNativeView(AppstockNativeAd ad) {
    View nativeContainer = View.inflate(this, R.layout.layout_native, null);

    ImageView icon = nativeContainer.findViewById(R.id.imgIcon);
    ImageUtils.download(ad.getIconUrl(), icon);

    TextView title = nativeContainer.findViewById(R.id.tvTitle);
    title.setText(ad.getTitle());

    ImageView image = nativeContainer.findViewById(R.id.imgImage);
    ImageUtils.download(ad.getImageUrl(), image);

    TextView description = nativeContainer.findViewById(R.id.tvDesc);
    description.setText(ad.getDescription());

    Button cta = nativeContainer.findViewById(R.id.btnCta);
    cta.setText(ad.getCallToAction());
    getContainerForAd().addView(nativeContainer);

    ad.registerView(nativeContainer, Arrays.asList(icon, title, image, description, cta), createListener());
}
```

If you need to manage stages of the ad lifecycle you should implement the `AppstockNativeAdUnitEventListener` interface.

Kotlin:

```kotlin
private fun createListener(): AppstockNativeAdUnitEventListener {
    return object : AppstockNativeAdUnitEventListener {
        override fun onAdImpression() {
            // Called when ad displayed
            Log.d(TAG, "Ad displayed on the screen")
        }

        override fun onAdClicked() {
            // Called when ad clicked
            Log.d(TAG, "Ad clicked")
        }

        override fun onAdExpired() {
            // Called when ad expired
            Log.d(TAG, "Ad expired")
        }
    }
}
```

Java:

```java
private static AppstockNativeAdUnitEventListener createListener() {
    return new AppstockNativeAdUnitEventListener() {
        @Override
        public void onAdImpression() {
            // Called when ad displayed
            Log.d(TAG, "Ad displayed on the screen");
        }

        @Override
        public void onAdClicked() {
            // Called when ad clicked
            Log.d(TAG, "Ad clicked");
        }

        @Override
        public void onAdExpired() {
            // Called when ad expired
            Log.d(TAG, "Ad expired");
        }
    };
}
```
