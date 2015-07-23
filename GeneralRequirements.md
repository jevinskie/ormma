# General SDK Requirements for Supporting ORMMA API #
This section details the requirements on an in-app ad serving SDK that is ORMMA compatible.

It is expected that an implementation would be in two parts. The first part defines a native container for Rich Media Ads to display in Apps and the second part defines a JavaScript controller for Ad Creatives to interact with. The native container encapsulates an XHTML and JavaScript enabled Web browser view, such as iOS’s UIWebView, and the controller serves as a bridge that can integrate HTML-based ads with the native capabilities. Actual implementations may vary.

When planning, key design considerations are:
  * Access to the device’s native features (orientation, location, acceleration, etc.)
  * On and off-line Ad viewing and metrics
  * Industry standard Ad development (XHTML and JavaScript)
  * Progressive complexity (simple things are simple, complex things are possible but harder)

### Technical Audience ###
The specifications are technical by nature, but are not intended to limit innovation. This document is intended for Publishers or SDK vendors and addresses the needs of the Ad Designers.

#### Native Application Developer ####
There are no requirements in this specification for app developers. They should follow the instructions provided by their SDK developer for integrating ads into their application.

#### SDK Developer ####
SDK builders have a number of responsibilities outside this recommendation. (See “out-of-scope”.) As mentioned, it is expected that the SDK developer will provide two interfaces to implement these recommendations: a container for the native developer to integrate via the SDK and a controller for the ad developer to use directly.

This document outlines the requirements of the controller needed by the ad developer. It is the intention of the writers that these concepts can be managed with a facade layer for existing SDKs.

#### Ad Developer ####
There are no creative requirements in this document for ad designers and developers besides the use of web standards. Ad developers that use the methods in this specification can provide a rich media experience across platforms and publishers.

It is important for ad designers to recognize that calls to the native device must be asynchronous by design. For most web developers, this is analogous to AJAX programming.

### Out of Scope ###
Each SDK provides unique features sets to developers. This document outlines a minimum set of features for interoperability and does not define features that may also be part of an SDK such as
  * Retrieving the ad from Ad Server, Ad Network, or local resources
  * Reporting
  * IDE integrations
  * Security
  * Internationalization
  * Error reporting
  * Logging
  * Billing and payments

Of course, the SDK developer must implement the ability to render web content in the area intended for the ad unit. For most environments, this capability is already available as a web view component although the developer may have to develop additional functions to support these specifications.

It is the intent of the writers that SDK vendors are not limited to delivering only the features outlined in the API. They should continue to innovate and present features that differentiate them in the marketplace.

### Standard Web Technologies ###
For interoperability, only web compatible languages should be used for markup and scripting languages. This document assumes XHTML/JavaScript/CSS. The ad designer should be able to develop and test the ad unit in a web browser. If designers use tags, styles and functions which are compatible with only one browser (such as CSS3 on WebKit), then the ad should be targeted to compatible devices.

### Ad Server Requirements ###
The ad server used to traffic rich media ads should support XHTML ads with JavaScript.

### Requirements for Ad Rendering ###
#### Display of HTML Ads – Ad View Container ####
It is expected that an ORMMA-compatible SDK will display any HTML ad. Ad designers that are not concerned with rich media or accessing native features can simply provide simple HTML for display in the application.

The SDK should invoke an XHTML with JavaScript rendering engine for rendering ads. In this document, that engine will be called the "web viewer". As possible, the web viewer should incorporate the capabilities of the device web browser. For example, iOS developers may use UIWeb. A given App view can have one or more Ad View Containers that will all act independently of one another.

### Requirements for Ad Developer ###
#### Display Control for Rich Media Ads – Ad Controller ####
Additional creative requirements will register to use the ORMMA API on an as-needed basis. This supports the concept of progressive complexity.

So, the ad designer is in control of the ad display, but uses the ORMMA API when they need to communicate with the native layer. The internal interaction is hidden from both the Ad developer and the App developer.

An ad that does not utilize any device features does not need to use the ORMMA API at all. Some of the things an ad does use the API for are:
  * Query the SDK for supported features, such as:
    * Accelerometer
    * Compass
    * GPS
    * Specific gestures
    * Whether the device is currently on or off-line
    * Etc.
  * Register JavaScript Event Listener functions to be called by the Container for:
    * Accelerometer readings
    * Touch events
    * Gestures
    * Etc.
  * Download assets to the local file system for:
    * Caching large assets to improve performance
    * Installing assets for off-line use
  * Capturing user actions for:
    * Opening an embedded Web browser
    * Clicking within an Ad triggering an action
    * Storing metrics when off-line for transmission when the device is back on-line
  * Moving or resizing the Container and Web View for:
    * Modal take-over of the entire display
    * Modal or modeless “fly-out” of the Ad from the original Container bounds

### Lifecycle Examples ###
#### Simple Ad Lifecycle Example ####
In the simplest example, an application developer adds an ORMMA-compatible View to their application UI either programmatically or with an interface builder.

When the app developer wants their app to display an ad, they rely on the SDK to retrieve an HTML ad. The View then displays the resulting HTML. If the ad does not make use of the ORMMA API, then it will behave as a normal HTML ad and any links will open in the device’s default web browser. This fallback functionality allows fixed sized, non-interactive web creatives to be used without modification.

#### Rich Media Ad Lifecycle Example ####
In a more complex example, the Ad Designer uses the JavaScript API to take communicate with the native layer and interact with features of the device and OS. An initial ad displays first as a small shim with a static background or a “Loading…” message.

The ad may use local assets if they are available or request that assets be downloaded and executed locally.

As an example, when the user touches the ad, JavaScript uses the ORMMA API to notify the App (via the SDK) that the ad is expanding so that it can stop anything that the user will not be able to interact with. The SDK then resizes the web view to take up the entire screen of the device.

When the ad expands to the full screen, JavaScript registers event listeners for the accelerometer and shake gestures. The body of the expanded ad is a game where the user controls something by manipulating the device’s orientation. The event listeners for the accelerometer data get called with the devices 3D orientation vectors at the desired interval and the ad’s JavaScript controls the game rendering.

When the user is done with the expanded ad, they click a close button that causes the ad to unregister the event listeners, resize the ad to its original size, display the ad’s banner state, and notify the App that it can resume. If the device was off-line when the user interaction took place, any metrics called are cached by the SDK until the device is on-line again, and then the tracking calls are sent.

### Compliance Levels ###
For a variety of reasons, it is understood that SDK vendors may choose to support only some portions of the API. However, to be an effective standard, a minimum level of implementation is needed. This document identifies different levels of compliance so that developers and managers can make informed decisions when choosing a vendor.

#### Level 1 ####
This is a minimum level of compliance to meet the requirements of basic rich media ads, primarily to change the screen size.

#### Level 2 ####
In addition to the minimum requirements, this level exposes the native device properties with broadcast events.

#### Level 3 ####
This level includes all aspects of the specification, including offline rendering and tracking.