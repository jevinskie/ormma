# Ad Controller Requirements: Level 3 #

### Offline Viewing ###
While rich media ads are built on web technologies for interoperability, Internet access is not always available in applications. For greatest reach, the SDK should provide a capability for off-line viewing of ads, including a mechanism to send tracking data once Internet access is restored.

Although HTML5 defines a standard for offline assets using a manifest file, this specification does not currently require it. Many ad creatives are built (correctly) as HTML snippets, not complete HTML pages as required for manifests. However, the HTML5 specification for offline viewing is still in progress and this may affect future recommendations for cross-platform rich media ads on mobile devices.

### Scenario for packaged ads (pre-loading) ###
Some application developers have a need to package ad content with their application, either directly or as part of content updates. In this case, it is desirable that all ad assets are downloaded to the device directly without a separate network call (like an ad-tag) when the user views the ad content.

While this specification identifies this need for pre-loading ads, it is outside the scope of the current version.

### Scenario for cached ads ###
The SDK should cache the last ad received during the rendering of each placement. Then in an offline mode, the cached ad is shown again.
In addition, the SDK should provide a method to capture outgoing web traffic from the ad and send it again once the application is started with Internet access, namely a store-and-forward strategy. All web responses for these stored requests would be ignored.

As an example
  1. user starts application online
  1. SDK requests ad from Internet
  1. SDK stores the ad assets identified by the ad developer and renders the ad
  1. the ad sends an outbound request for tracking impression data
  1. user quits application
  1. user starts application offline
  1. the SDK attempt to request ad from Internet fails
  1. SDK renders ad from local storage
  1. the ad sends an outbound request to track impression data
  1. SDK captures the outbound request identified by the ad developer
  1. user starts application online
  1. SDK sends all stored outbound requests
  1. SDK requests ad from Internet
  1. SDK stores the new ad assets to repeat process

#### offline assets ####
To fulfill the requirements of this scenario, the SDK must cache and then intercept all ad requests to assets identified by the ad designer using the Ad Controller. Requests to these URLs by the ad must be served by the SDK instead.

#### offline tracking ####
The SDK must intercept all requests identified by the ad designer using the Ad Controller. The SDK should then implement a store-and-forward strategy to resend these requests the next time the device is online.

### Asset Management ###
To enable offline viewing and performance optimization, the Controller also provides precise control over local cache in the Container. This cache allows the ad designer to define additional aliases for any kind of remote content - images, videos, even other ads - that can then be referenced in the ad when needed.

However, since a finite amount of local cache is available, ad designers may need direct asset management control to optimize the ad experience. The local cache uses a "Least Recently Used" algorithm to retire old assets as new assets are added. If an object is requested from the cache that has been retired, the SDK will attempt to reacquire the asset if online (otherwise a error is thrown -- see error handling).

_Note: Because of security restrictions in the OS Web browser control, only local base HTML documents can access locally stored objets. Therefore, a network served ad must first download a local copy of the same or a different HTML document to load into the ad as part of an expand call._

**addAsset** method

The addAsset method downloads, stores, and creates a new alias for an object in the local cache.

**addAssets method**

Use this method to create multiple aliases at once. Triggers multiple “assetReady” events.

**assetReady** event

The assetReady event fires once an asset has been successfully downloaded. Once the event fires, use getAssetURL to get the local path to the object.

**getAssetURL** method

The getAssetURL method translates the asset alias into a local file path that a local ad can access.

**removeAsset** method

The removeAsset method removes an asset from the local cache.

**removeAllAssets** method

Use this method to clear the local cache of all aliases. Triggers multiple “assetRemoved” events.

**assetRemoved** event

The assetRemoved event is thrown when an asset has successfully been removed.

**getCacheRemaining** method

The getCacheRemaining method returns the number of remaining bytes available in the local cache.

**assetRetired** event

This event is thrown when an asset has been retired from the local cache to make room for new assets.

### Special Media Assets ###

Rich Media Ads can access two special asset types that allow them to take a screenshot of the device's screen and take a picture with the device's camera. Calling the addAsset method with a URL of "ormma://screenshot" will take a screenshot and save it to the specified alias. Calling the addAsset method with a URL of "ormma://photo" will open the devices camera interface and save a photo it to the specified alias.

**storePicture** method

The storePicture method will place a picture in the device's photo album. The picture may be local or retrieved from the Internet.

### Offline Requests and Metrics ###

Rich Media Ads that can work while the device is without network connectivity need the ability to store and later forward metrics about how and when users interact with the ad.

While the following request method and response event are provided for greater flexibility in offline ads, their use is not confined to offline. An ad designer can use the request/response pair in an online state to provide Ajax style updates, for example.

**request** method

The request method makes an HTTP request when the device has network connectivity and caches the request for later transmission when the device is offline.

**response** event

The response event is fired when a request method completes and provides the response if desired.