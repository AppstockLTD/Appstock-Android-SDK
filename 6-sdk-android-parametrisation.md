# Appstock Android SDK - Parametrisation

## Configuration via `AppstockTargeting` class

The `AppstockTargeting` class provided a set of properties that allow to enrich the ad request.

| Method                                  | Description                                                                                                                                                                                                                   | OpenRTB Field        |
|-----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| `AppstockTargeting.setPublisherName()`  | App's publisher name                                                                                                                                                                                                          | `app.publisher.name` |
| `AppstockTargeting.setDomain()`         | Domain of the app (e.g., `mygame.foo.com`).                                                                                                                                                                                   | `app.domain`         |
| `AppstockTargeting.setStoreUrl()`       | App store URL for an installed app.                                                                                                                                                                                           | `app.storeurl`       |
| `AppstockTargeting.setSubjectToCOPPA()` | Integer flag indicating if this request is subject to the COPPA regulations established by the USA FTC, where 0 = no, 1 = yes                                                                                                 | `regs.coppa`         |
| `AppstockTargeting.setExternalUserId()` | App store URL for an installed app.                                                                                                                                                                                           | `user.ext.eids[]`    |
| `AppstockTargeting.setUserLatLong()`    | Location of the user’s home base defined by a Geo object This is not necessarily their current location.                                                                                                                      | `user.geo.lat/lon`   |
| `AppstockTargeting.setUserKeywords()`   | Comma separated list of keywords, interests, or intent.                                                                                                                                                                       | `user.keywords`      |
| `AppstockTargeting.setUserCustomData()` | Optional feature to pass bidder data that was set in the exchange’s cookie. The string must be in base85 cookie safe characters and be in any format. Proper JSON encoding must be used to include “escaped” quotation marks. | `user.customdata`    |

Usage examples:

Kotlin:
```kotlin
    AppstockTargeting.setPublisherName("appstock")
    AppstockTargeting.setDomain("appstock.com")
    AppstockTargeting.setStoreUrl("https://google.play.url")
    AppstockTargeting.setSubjectToCOPPA(true)
    AppstockTargeting.setExternalUserId(ExternalUserId("adserver.org", "111111111111", null, mapOf("rtiPartner" to "TDID")))
    AppstockTargeting.setUserLatLong(35.82348f, 23.8243823f)
    AppstockTargeting.setUserKeywords(setOf("cats", "hobby", "sport"))
    AppstockTargeting.setUserCustomData("custom")
    Appstock.initializeSdk(context, PARTNER_KEY)
```

Java:
```java
    AppstockTargeting.setPublisherName("appstock");
    AppstockTargeting.setDomain("appstock.com");
    AppstockTargeting.setStoreUrl("https://google.play.url");
    AppstockTargeting.setSubjectToCOPPA(true);
    AppstockTargeting.setUserLatLong(35.82348f, 23.8243823f);
    AppstockTargeting.setUserCustomData("custom");

    HashMap<String, Object> externalUserIdExt = new HashMap<>();
    externalUserIdExt.put("rtiPartner", "TDID");
    AppstockTargeting.setExternalUserId(new ExternalUserId("adserver.org", "111111111111", null, externalUserIdExt));
    
    HashSet<String> keywords = new HashSet<>();
    keywords.add("cats");
    keywords.add("sport");
    AppstockTargeting.setUserKeywords(keywords);
    
    Appstock.initializeSdk(context, PARTNER_KEY);
```

## Configuration via `Appstock` class

Public methods:

- `initializeSdk`  - initializes the SDK.
- `setEndpointId` - a unique identifier generated on the platform's UI.
- `setExternalUserIds` - an array containing objects that hold external user ID parameters.
- `setAssignNativeAssetId` - determines whether the asset ID for native ads should be manually assigned.
- `setDebugRequests` - sets debug mode for verbose logging of requests and responses bodies (use with `setLogLevel(LogLevel.DEBUG)`)
- `setLogLevel` - sets the desired verbosity level for the SDK's logs.
- `setTimeoutMillis` - set network HTTP timeout for all requests. 
- `setCreativeFactoryTimeout` - timeout for parsing and render banner ads content (default: 6000).
- `setCreativeFactoryTimeoutPreRenderContent` - timeout for parsing and render video ads content (default: 30000).

Kotlin:
```kotlin
    Appstock.setEndpointId("endpoint_id")
    Appstock.getAssignNativeAssetId(true)
    Appstock.setDebugRequests(true)
    Appstock.setLogLevel(Appstock.LogLevel.DEBUG)
    
    Appstock.setTimeoutMillis(3000)
    Appstock.setCreativeFactoryTimeout(10000)
    Appstock.setCreativeFactoryTimeoutPreRenderContent(40000)
    
    val externalUserIdExt = HashMap<String, Any>()
    externalUserIdExt["rtiPartner"] = "TDID"
    val externalUserId = ExternalUserId("adserver.org", "111111111111", null, externalUserIdExt)
    Appstock.setExternalUserIds(List.of(externalUserId))
    
    Appstock.initializeSdk(this, PARTNER_KEY)
```

Java:
```java
    Appstock.setEndpointId(ENDPOINT_ID);
    Appstock.getAssignNativeAssetId(true);
    Appstock.setDebugRequests(true);
    Appstock.setLogLevel(Appstock.LogLevel.DEBUG);
    
    Appstock.setTimeoutMillis(3000);
    Appstock.setCreativeFactoryTimeout(10_000);
    Appstock.setCreativeFactoryTimeoutPreRenderContent(40_000);
    
    HashMap<String, Object> externalUserIdExt = new HashMap<>();
    externalUserIdExt.put("rtiPartner", "TDID");
    ExternalUserId externalUserId = new ExternalUserId("adserver.org", "111111111111", null, externalUserIdExt);
    Appstock.setExternalUserIds(List.of(externalUserId));
    
    Appstock.initializeSdk(this, PARTNER_KEY);
```
