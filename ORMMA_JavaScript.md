# Introduction #
The adoption of the ORMMA standard allows all ad developers to use the same JavaScript methods and know that their ad will behave as expected on a variety of platforms.

This page outlines all the methods and events that ad developers will have access to. ORMMA completely integrates the IAB MRAID standard. For methods and events that are only supported by MRAID containers, please find the IAB logo.




# Methods #
_`*`methods marked with an asterisk are dependent on the device. Ad developers and SDK implementers must use the supports() method to identify what methods are available._


## addEventListener ##
```
	ormma.addEventListener(event, listener)
	mraid.addEventListener(event, listener)
```
API version history
  * Introduced in ORMMA version: 1.0.0
  * Included in MRAID version: 1

Use this method to subscribe a specific handler method to a specific event. In this way, multiple listeners can subscribe to a specific event, and a single listener can handle multiple events. All ORMMA events are listed below, with the MRAID subset of events listed first:

| **value**        | **description**                       | **MRAID** |
|:-----------------|:--------------------------------------|:----------|
| ready            | report initialize complete            | |
| error            | report error has occurred             | |
| stateChange      | report state changes                  | |
| viewableChange   | report viewable changes               | |
| network          | report network connectivity changes   |           |
| keyboard         | report soft keyboard changes          |           |
| orientation      | report orientation changes            |           |
| heading          | report heading changes                |           |
| location         | report location changes               |           |
| response         | report response from request call     |           |
| screen           | report screen size changes            |           |
| shake            | report device being shaken            |           |
| size             | report ad size changes                |           |
| tilt             | report tilt changes                   |           |




> parameters:
    * event (String - required) : name of event to listen for
    * listener (Function - required) : function name (or anonymous function) to execute

> return value: none

> events triggered: none

> side effects: registering listeners for device features may power up sensors in the device that will reduce battery life.

> level: 1

## close ##
```
	ormma.close()
	mraid.close()
```
API version history
  * Introduced in ORMMA version: 1.0.0
  * Included in MRAID version: 1

The close method will cause the ad webview to downgrade its state. It will also fire the stateChange event. For ads in an expanded state, the close() method moves to a default state.

For ads in a default state, the close() method moves to a hidden state. This method may be used by ad designers as an addition to the SDK-enforced close ability.

An expanded or interstitial ad must provide an end-user with the ability to close the expanded creative. This is a requirement for the MRAID-compliant SDK to ensure that users are always able to return to the publisher content even if an ad has an error or cannot be closed. The ad designer may provide additional design elements to close the expanded view as desired.

The location and design of the SDK-controlled close ability is left to the vendor, but is strongly recommended as a 50x50 clickable area in the top-right corner of the ad. The ad designer may optionally choose to provide the indicator for the SDK-supplied close capability, although the ad designer may not move that capability from the SDK’s specified location. If the ad designer builds the close indicator into the creative they must specify to use their custom close indicator. If the ad designer does not provide its own close graphic within the creative, the SDK will supply a close graphic of its own. This SDK-supplied clickable area will be placed at the highest z-order possible, and must always be available to the end user.

If expand was used with a URL parameter, then close must display the original content. If the SDK suspended the app when the ad changed to the expand state, then the SDK should notify the app to resume.

If the expanded or interstitial ad view was closed using the SDK-supplied close control, then the stateChange event is still fired and the app still notified to resume. Expanded ads must always listen for the stateChange event and adjust as necessary.

> parameters:
    * none

> return value: none

> events triggered: stateChange

> side effects: changes state

> level: 1

## createEvent ##
```
	ormma.createEvent(date, title, body)
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to create a new event in the devices calendar.

> parameters:
    * date (Date - required) : the date and time of the event
    * title  (String - required) : the title of the event
    * body  (String - required) : the body of the event

> return value: none

> events triggered: none

> side effects: none

> level: 2

## expand ##
```
	ormma.expand(url)
	mraid.expand(url)
```
API version history
  * Introduced in ORMMA version: 1.0.0
  * Included in MRAID version: 1

The expand method will cause an existing Web View (for one-part creatives) or a new Web View (for two-part creatives) to open at the highest z-order in the view hierarchy. The expanded view can either contain a new HTML document if a URL is specified, or a copy of the same document that was in the default position. While an ad is in an expanded state, the default position's HTML is suspended. Thus a complete implementation allows for ad designers to use one-part ads (where the banner and panel are part of one creative) and two-part ads (where the banner and panel are separate HTML creatives).

The expand method may change the size of the ad container, and will move state from "default" to "expanded" and fire the stateChange event. Calling expand more than once is permissible, but has no effect on state (which remains “expanded”).

An expanded view may cover all available screen area even though the ad creative may not (e.g. via a transparent overlay), or it may cover only a partial screen area. Issues of ad modality are left to the vendor. In order to avoid issues of partial screen and ad modality, use the ormma.resize method for partial screens and the ormma.expand method for full screen ads.

An expanded view must provide an end-user with the ability to close the expanded creative. These requirements are discussed further in the description of the close method.

When the ad size is greater than the screen size, the ad will be centered vertically and horizontally causing outlying areas to be cropped.

When the expand method is called without the URL parameter, the current web view will be reused, simplifying reporting and ad creation. The original creative is not reloaded and no additional impressions are recorded. Implementing this definition allows for one-part creatives.

When the expand method is called with the URL parameter, a new web view will be used. Implementing this definition allows for two-part creatives.

> parameters:
    * url (String) : The optional URL for the document to be displayed in a new overlay view. If null, the body of the current ad will be used in the current web view.

> return value: none

> events triggered: stateChange, sizeChange

> side effects: changes state

> level: 1

## getDefaultPosition ##
```
	ormma.getDefaultPosition()
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this parameter to get the location and size of the default ad view.

> parameters:
    * none

> return value: JSON {x, y, width, height}

> events triggered: none

> side effects: none

> level: 1

## getExpandProperties ##
```
	ormma.getExpandProperties()
	mraid.getExpandProperties()
```
API version history
  * Introduced in ORMMA version: 1.0.0
  * Included in MRAID version: 1

Use this method to get the properties for expanding an ad. The properties marked with an asterisk `*` are also available in MRAID containers.

**properties object**
```
properties = {
 "width`*`" : "nn",
 "height`*`" : "nn", 
 "useCustomClose`*`" :  "true|false",
 "isModal`*`" : "true|false" (read-only),
 "lockOrientation" : "true|false",
 "useBackground" : "true|false (deprecated)",
 "backgroundColor" : "#rrggbb (deprecated)",
 "backgroundOpacity" : "n.n (deprecated)"
}
```

**"width"**
"width" is an integer that identifies the width of creative in pixels, default is full screen width

**"height"**
"height" is an integer that identifies the height of creative in pixels, default is full screen height

**"useCustomClose"**
"useCustomClose" allows the ad designer to replace the default close graphic. True, stop showing the default close graphic and rely on ad creative’s custom close indicator; false (default), container will display the default close graphic

**"isModal"**
"isModal" is a read-only property that identifies if the expanded container is modal or not. True, the SDK is providing a modal container for the expanded ad; false, the SDK is not providing a modal container for the expanded ad.

**"lockOrientation"**
The "lockOrientation" property is a boolean value (true/false) and if it is not specified in the properties object a value of false is assumed.

**"useBackground"**
"useBackground" has been deprecated. Designers should provide their own background to the HTML ad creative using CSS.

**"backgroundColor"**
"backgroundColor" has been deprecated. Designers should provide their own background to the HTML ad creative using CSS.

**"backgroundOpacity"**
"backgroundOpacity" has been deprecated. Designers should provide their own background to the HTML ad creative using CSS.


> parameters:
    * none

> return value: JSON { ... } - the expand properties

> events triggered: none

> side effects: none

> level: 1

## getHeading`*` ##
```
	ormma.getHeading()
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to get the most recent compass direction of the current vertical axis of the device. To receive events when the a change occurs, register an event listener for "headingChange" events. Values are:

| **value** | **description**                |
|:----------|:-------------------------------|
|    -1     | no heading known               |
| 0-359     | compass direction in degrees   |



> parameters:
    * none

> return value: Number - the degrees

> events triggered: none

> related events: headingChange

> side effects: none

> level: 2

## getKeyboard`*` ##
```
	ormma.getKeyboard()
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to determine if the virtual keyboard is present on the screen. The boolean result is true if the keyboard is present and false if it is hidden or not applicable.

> parameters:
    * none

> return value: Boolean - the virtual keyboard is present

> events triggered: none

> related events: keyboardChange

> side effects: none

> level: 2

## getLocation`*` ##
```
	ormma.getLocation()
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to get the most recent location reading from the device. To receive events when the a change occurs, register an event listener for "locationChange" events.

> parameters:
    * none

> return value: JSON {lat, lon, acc} - the latitude, longitude, and accuracy of the reading or null

> events triggered: none

> related events: locationChange

> side effects: none

> level: 2

## getMaxSize ##
```
	ormma.getMaxSize()
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to return the maximum size an ad can grow to using the resize() method. This may be set by the developer, or be the size of the parent view.

> parameters:
    * none

> return value: JSON {width, height} - the maximum width and height the view can grow to

> events triggered: none

> side effects: none

> level: 1

## getNetwork`*` ##
```
	ormma.getNetwork()
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to identify the most recent network status of the device. To receive events when a change occurs, register an event listener for "networkChange" events. Possible results include:

| **value** | **description**                                  |
|:----------|:-------------------------------------------------|
| offline   | no network connection                            |
| wifi      | network using a wifi antennae                    |
| cell      | network using a cellular antennae (such as 3G)   |
| unknown   | network connection in unknown state              |



> parameters:
    * none

> return value: String

> events triggered: none

> related events: networkChange

> side effects: none

> level: 2

## getOrientation`*` ##
```
	ormma.getOrientation()
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to get the most recent orientation of the device. To receive events when a change occurs, register an event listener for "orientationChange" events. Possible results include:

| **value** | **description**                                       |
|:----------|:------------------------------------------------------|
|      -1   | device orientation unknown                            |
|       0   | 0 degrees (portrait)                                  |
|      90   | 90 degrees (tilted clockwise to landscape)            |
|     180   | 180 degrees (portrait upside down)                    |
|     270   | 270 degrees (tilted counter-clockwise to landscape)   |


> parameters:
    * none

> return value: Number

> events triggered: none

> related events: orientationChange

> side effects: none

> level: 2

## getPlacementType ##
```
	ormma.getPlacementType(none)
	mraid.getPlacementType(none)
```
API version history
  * Introduced in ORMMA version: 1.1.0
  * Included in MRAID version: 1

For efficiency, ad designers sometimes flight a single piece of creative in both banner and interstitial placements.  So that the creative can be aware of its placement, and therefore potentially behave differently, each ad container has a placement type determining whether the ad is being displayed inline with content (i.e. a banner) or as an interstitial overlaid content (e.g. during a content transition).  The SDK returns the value of the placement to creative so that creative can behave differently as necessary.  The SDK does not determine whether a banner is an expandable (the creative does) and thus does not return a separate type for expandable.


value 	description
inline        	the ad placement is inline with content (i.e. a banner) in the display
interstitial   	the ad placement is over laid on top of content


> parameters:
    * none (String) :

> return value: String - either "inline" or "interstitial"

> events triggered: none

> side effects: none

> level: 1

## getScreenSize`*` ##
```
	ormma.getScreenSize()
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to get the current pixel width and height of the device. Although point width (pt) is preferred over pixel width (px) in the ad design because of device screens with different DPI specs, it is still essential for the designer to know how many pixels are on the screen.

> parameters:
    * none

> return value: JSON {width, height}

> events triggered: none

> related events: screenChange

> side effects: none

> level: 2

## getShakeProperties`*` ##
```
	ormma.getShakeProperties()
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to retrieve the current shake properties.

> parameters:
    * none

> return value: JSON {interval, intensity}

> events triggered: none

> related events: shake

> side effects: none

> level: 2

## getSize ##
```
	ormma.getSize()
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to get the current size of the ad.

> parameters:
    * none

> return value: JSON {width, height}

> events triggered: none

> related events: sizeChange

> side effects: none

> level: 1

## getState ##
```
	ormma.getState()
	mraid.getState()
```
API version history
  * Introduced in ORMMA version: 1.0.0
  * Included in MRAID version: 1

Each ad container (or Web View) has a state that is one of the following. Note that "resized" is currently only available in ORMMA.

| **value**  | **description**                                                                                   |
|:-----------|:--------------------------------------------------------------------------------------------------|
| loading    | the SDK is not yet ready for interactions with the Controller                                     |
| default    | the initial position and size of the ad container as placed by the application and SDK            |
| resized    | the initial position and size of the ad container has changed                                     |
| expanded   | the ad container has expanded to cover the application content at the top of the view hierarchy   |
| hidden     | the ad container no longer displays the ad                                                        |

This method returns the current state of the ad container, returning whether the ad container is in its default, fixed position, a changed, resized position, or is in an expanded, larger position.

The effect on state from calling expand(), resize(), and close() are defined in this table.

| **state** | **expand()**                  | **resize()**                 | **close()**                  |
|:----------|:------------------------------|:-----------------------------|:-----------------------------|
|loading    | no effect                     | no effect                    | no effect                    |
|default    | state changed to “expanded”   | state changed to "resized"   | state changed to “hidden”    |
|resized    | state changed to "expanded"   | no effect                    | state changed to "default"   |
|expanded   | no effect                     | state changed to "resized"   | state changed to “default”   |
|hidden     | no effect                     | no effect                    | no effect                    |



> parameters:
    * none

> return value: String (enumerated) - "loading", "default", "resized", "expanded", or "hidden"

> events triggered: none

> related events: stateChange

> side effects: none

> level: 1

## getTilt`*` ##
```
	ormma.getTilt()
```
API version history
  * Introduced in ORMMA version: 1.0.0

This method returns the last reading of the devices 3 dimensional tilt.

> parameters:
    * none

> return value: JSON {x, y, z}

> events triggered: none

> related events: tiltChange

> side effects: none

> level: 2

## getVersion ##
```
	ormma.getVersion()
	mraid.getVersion()
```
API version history
  * Introduced in ORMMA version: 1.1.0
  * Included in MRAID version: 1

This method allows the ad to confirm a basic feature set before display. The version number corresponds with the API specification version and not a vendor’s SDK version.

For example, calling mraid.getVersion() will return the MRAID version the container complies with. Calling ormma.getVersion() will return the ORMMA API version the container follows.

> parameters:
    * none

> return value: String – the API specification that this container is certified against, for example, “1.1.0”

> events triggered: none

> side effects: none

> level: 1

## hide ##
```
	ormma.hide()
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to hide the web viewer. The method has no return value and is executed asynchronously (so always listen for a result event before taking action instead of assuming the change has occurred).

> parameters:
    * none

> return value: none

> events triggered: stateChange

> side effects: changes state

> level: 1

## isViewable ##
```
	ormma.isViewable(none)
	mraid.isViewable(none)
```
API version history
  * Introduced in ORMMA version: 1.1.0
  * Included in MRAID version: 1

In addition to the state of the ad container, it is possible that the container is loaded off-screen as part of an application's buffer to help provide a smooth user experience. This is especially prevalent in apps that employ scrolling views.

This method returns whether the web container view showing the ad is currently on screen.

> parameters:
    * none (String) :

> return value: Boolean - true, container is on-screen and viewable by the user; false: container is off-screen and not viewable

> events triggered: none

> related events: viewableChange

> side effects: none

> level: 1

## makeCall ##
```
	ormma.makeCall(phoneNumber)
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to make a phone call on the device to the phone number provided. This is similar to a tel:// protocol, but the SDK will attempt to suspend the application.

> parameters:
    * phoneNumber (String - required) : the phone number for the device to call

> return value: none

> events triggered: none

> side effects: device may leave application to activate dialer and phone capabilites

> level: 2

## open ##
```
	ormma.open(url)
	mraid.open(url)
```
API version history
  * Introduced in ORMMA version: 1.0.0
  * Included in MRAID version: 1

The open method will display an embedded browser window in the application that loads an external URL. On device platforms that do not allow an embedded browser, the open method invokes the native browser with the external URL.

Note: This should be used only for external web pages that are not ORMMA ads. The displayed page will not load the app’s ORMMA or MRAID files and so the close() method will not have any effect on the embedded browser. It can only be closed by the user selecting the close control for the window, which is implementation specific.

Use this method to open an HTML browser to an external web page. This may launch an external browser, depending on the SDK implementation. To place the ad over content, use the expand() method instead.

The native browser controls – back, forward, refresh, close – will always be present. For reporting, open should always be used for click through actions.



> parameters:
    * url (String - required) : the URL of the web page to open

> return value: none

> events triggered: none

> side effects: application may be unavailable while user explores URL

> level: 1

## openMap`*` ##
```
	ormma.openMap(POI, fullscreen)
```
API version history
  * Introduced in ORMMA version: 1.0.1

Use this method to open a native map with the Point of Interest (POI) parameter formatted according to the Google Maps standard (see http://mapki.com/wiki/Google_Map_Parameters)

> parameters:
    * POI (String - required) : Google Maps-formatted argument. The parameter must describe a point on a map, not, for example, driving directions
    * fullscreen (Boolean, default value = true) : true - map displays within the current View, false - within a new View that takes up the whole screen

> return value: none

> events triggered: none

> side effects: device may require user permission for geolocation data

> level: 2

## playAudio ##
```
	ormma.playAudio(url, properties)
```
API version history
  * Introduced in ORMMA version: 1.0.1

Use this method to play audio on the device. This may launch an external player, depending on the SDK implementation. To place the audio with the content, set the position property. For the most part, properties follow the HTML5 audio tag conventions.

Controls:
| **property** | **values**          | **description**                                                                                                                               |
|:-------------|:--------------------|:----------------------------------------------------------------------------------------------------------------------------------------------|
| autoplay     | autoplay            | include if audio should play immediately                                                                                                      |
| controls     | controls            | include if native player controls should be visible                                                                                           |
| loop         | loop                | include if audio should start over again after finishing                                                                                      |
| position     | {left, top}         | include if audio should be included with ad content. Calculated relative to the web container                                                 |
| startStyle   | normal/fullscreen   | set to fullscreen if audio should start playing in native full screen mode -- user may still use controls to change size, default is normal   |
| stopStyle    | normal/exit         | set to exit if audio player should exit after the audio stops, default is normal                                                              |

There are some special combinations to identify:
**loop=loop and stopStyle=exit. In this case, the stopStyle will be ignored while the audio is playing. That is, the audio will loop back to the beginning after having played all the way through. However, when the user stops the audio with controls, the player will exit.** position={top,left} and startStyle=fullscreen. In this case, the position will be ignored while the audio control is in full screen. That is, the audio will start with a full screen player. However, when the user resizes the player with controls, the player will display at the position given.


> parameters:
    * url (String - required) : the URL of the audio or audio stream
    * properties (JSON - required) : list of the properties for native player

> return value: none

> events triggered: none

> side effects: audio player invoked

> level: 2

## playVideo ##
```
	ormma.playVideo(url, properties)
```
API version history
  * Introduced in ORMMA version: 1.0.1

Use this method to play a video on the device. This may launch an external player, depending on the SDK implementation. To place the video over content, set the position property. For the most part, properties follow the HTML5 video tag conventions.

Controls:
| **property** | **values**          | **description**                                                                                                                                  |
|:-------------|:--------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------|
| audio        | muted               | include if audio track should be muted                                                                                                           |
| autoplay     | autoplay            | include if video should play immediately                                                                                                         |
| controls     | controls            | include if native player controls should be visible                                                                                              |
| loop         | loop                | include if video should start over again after finishing                                                                                         |
| position     | {left, top}         | provide the top and left coordinates in pixels if video should play inline (or on top of) ad content. Calculated relative to the web container   |
| width        | (pixels)            | pixel width of video, required for position                                                                                                      |
| height       | (pixels)            | pixel height of video, required for position                                                                                                     |
| startStyle   | normal/fullscreen   | set to fullscreen if video should start playing in native full screen mode -- user may still use controls to change size, default is normal      |
| stopStyle    | normal/exit         | set to exit if video player should exit after the video stops, default is normal                                                                 |

There are some special combinations to identify:
**loop=loop and stopStyle=exit. In this case, the stopStyle will be ignored while the video is playing. That is, the video will loop back to the beginning after having played all the way through. However, when the user stops the video with controls, the player will exit.** position={top,left} and startStyle=fullscreen. In this case, the position will be ignored while the video is in full screen. That is, the video will start with a full screen player. However, when the user resizes the player with controls, the video will display at the position given.


> parameters:
    * url (String - required) : the URL of the video or video stream
    * properties (JSON - required) : list of the properties for native player

> return value: none

> events triggered: none

> side effects: video player invoked

> level: 2

## removeEventListener ##
```
	ormma.removeEventListener(event, listener)
	mraid.removeEventListener(event, listener)
```
API version history
  * Introduced in ORMMA version: 1.0.0
  * Included in MRAID version: 1

Use this method to unsubscribe a specific handler method from a specific event. Event listeners should always be removed when they are no longer useful to avoid errors. If no listener function is provided, then all functions listening to the event will be removed.

> parameters:
    * event (String - required) : name of event
    * listener (String - required) : name of function to be removed

> return value: none

> events triggered: none

> side effects: none

> level: 1

## request ##
```
	ormma.request(url, display)
```
API version history
  * Introduced in ORMMA version: 1.0.0

The method executes asynchronously, but returns a Boolean value of false to facilitate use in anchor tags. There is also an option explicitly for metrics tracking that will cache requests offline and execute them whenever the device reconnects. The display parameter supports the following values:

| **value** | **description**                                                                          |
|:----------|:-----------------------------------------------------------------------------------------|
| ignore    | the response is ignored                                                                  |
| proxy     | the response is cached if the device is off-line and proxied when connectivity returns   |


> parameters:
    * url (String - required) : the fully qualified URL of the page or call to action asset
    * display (String - required) : the display style for the call to action

> return value: Boolean - false

> events triggered: response

> side effects: none

> level: 2

## resize ##
```
	ormma.resize(width, height)
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to resize the main ad view to the desired size. The views place in the view hierarchy will not change, so the effect on other views is up to the app developer. To place the ad over content, use the expand() method instead. You may build any transition effect needed into the ad creative. Resize is bound by the maxSize currently in effect.

> parameters:
    * width (Number - required) : the width in pixels
    * height (Number - required) : the height in pixels

> return value: none

> events triggered: sizeChange, stateChange

> side effects: changes state

> level: 1

## sendMail ##
```
	ormma.sendMail(recipient, subject, body)
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to compose an email message on the device. This is similar to the [mailto://](mailto://) protocol, but the SDK will attempt to suspend the application.

> parameters:
    * recipient (String - required) : the email address for the message -- several recipients are seperated by ; or ,
    * subject (String - required) : the subject line of the message
    * body (String - required) : the body of the message

> return value: none

> events triggered: none

> side effects: device may leave application to invoke email

> level: 2

## sendSMS ##
```
	ormma.sendSMS(recipient, body)
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to compose an SMS message on the device. This is similar to the sms:// protocol, but the SDK will attempt to suspend the application.

> parameters:
    * recipient (String - required) : the mobile device number for the message
    * body (String - required) : the body of the message

> return value: none

> events triggered: none

> side effects: device may leave application to invoke SMS client

> level: 2

## setExpandProperties ##
```
	ormma.setExpandProperties(properties)
	mraid.setExpandProperties(properties)
```
API version history
  * Introduced in ORMMA version: 1.0.0
  * Included in MRAID version: 1

Use this method to set the ad's expand properties.

> parameters:
    * properties (JSON - required) : this object contains any number of standard properties that might be used by the library when presenting the full screen web viewer. For more info see properties object.

> return value: none

> events triggered: none

> related events: getExpandProperties

> side effects: none

> level: 1

## setShakeProperties`*` ##
```
	ormma.setShakeProperties(properties)
```
API version history
  * Introduced in ORMMA version: 1.0.0

Use this method to set the shake properties. This method rarely needs to be called as supported devices have default settings.

> parameters:
    * properties (JSON - required) : { intensity, interval }

> return value: none

> events triggered: none

> related events: getShakeProperties

> side effects: none

> level: 2

## show ##
```
	ormma.show()
```
API version history
  * Introduced in ORMMA version: 1.0.0

This method has no return value and is executed asynchronously (so always listen for a result event before taking action instead of assuming the change has occurred).

> parameters:
    * none

> return value: none

> events triggered: stateChange

> side effects: changes the state value

> level: 1

## storePicture`*` ##
```
	ormma.storePicture(url)
```
API version history
  * Introduced in ORMMA version: 1.0.0

This method will store the image or other media type specified by the URL.

> parameters:
    * url (String - required) : the URL to the image or other media asset

> return value: none

> events triggered: none

> side effects: saves image to device

> level: 2

## supports ##
```
	ormma.supports(feature)
```
API version history
  * Introduced in ORMMA version: 1.0.0

For devices that do not expose any of the native device features, this method should always return false. The features are:

| **value**     | **description**                                                              |
|:--------------|:-----------------------------------------------------------------------------|
| screen        | the device can report on the screen size                                     |
| orientation   | the device can report on its orientation and orientation changes             |
| heading       | the device can report on the compass direction it is pointing                |
| location      | the device can report on its location                                        |
| shake         | the device can report on being shaken                                        |
| tilt          | the device can report on its tilt and tilt changes                           |
| network       | the device can report on its network connectivity and connectivity changes   |
| sms           | the device can send an sms message                                           |
| phone         | the device can make a phone call                                             |
| email         | the device can compose an email                                              |
| calendar      | the device can create a calendar entry                                       |
| camera        | the device can take a still picture image                                    |
| map           | the device can display a location on a map natively                          |
| audio         | the device can play native audio                                             |
| video         | the device can play native video                                             |
| level-1       | the SDK supports ORMMA level 1 functionality                                 |
| level-2       | the SDK supports ORMMA level 2 functionality                                 |



> parameters:
    * feature (String - required) : name of feature

> return value: Boolean – true, the feature is supported and getter and events are available; false, the feature is not supported

> events triggered: none

> side effects: none

> level: 1

## useCustomClose ##
```
	ormma.useCustomClose(flag)
	mraid.useCustomClose(flag)
```
API version history
  * Introduced in ORMMA version: 1.1.0
  * Included in MRAID version: 1

Although it is required that all implementing SDKs provide a clickable area with a default “close” graphic, it is possible for ad creators to use their own designs.

This method serves as a convenience method to the expand property of the same name. Setting the property or calling this method both have the same effect and can be used interchangeably. They signal to stop using the default close indicator.

For expanded ads, the designer does not need to call this method and would normally set the useCustomClose property in setExpandProperties().

For a stand-alone interstitial where there is no call to expand(), but there is still a close() requirement, the ad designer should call this method as early as possible.

Ad designers should be clear that a default close indicator will always show until the useCustomClose method is called and/or the property is set.

> parameters:
    * flag (Boolean - required) : true – ad creative supplies its own designs for the close area; false – default image will be displayed for the close area

> return value: none

> events triggered: none

> related events: setExpandProperties

> side effects: none

> level: 1

## getViewable (deprecated) ##
```
	ormma.getViewable()
```
API version history
  * Introduced in ORMMA version: 1.0.1
  * Deprecated in ORMMA version: 1.1.0

Alias for isViewable().

> parameters:
    * none

> return value: Boolean - true, container is on-screen and viewable by the user; false: container is off-screen and not viewable

> events triggered: none

> related events: viewableChange

> side effects: none

> level: 1

## ORMMAReady (deprecated) ##
```
	ormma.ORMMAReady()
```
API version history
  * Introduced in ORMMA version: 1.0.0
  * Deprecated in ORMMA version: 1.1.0

The ORMMAReady() method has been deprecated in favor of the ready event. It is still available for compatibility.

This method initializes communication between the web layer and the native layer. The method must be in window scope of the ad unit and is called by the SDK after it has successfully initialized.

> parameters:
    * none

> return value: none

> events triggered: none

> related events: ready

> side effects: ORMMA JavaScript library available to ad unit

> level: 1

# Events #
_`*`events marked with an asterisk are dependent on the device. Ad developers and SDK implementers must use the supports() method to identify what events are available._

## error ##
```
	"error" -> function(message, action)
```
API version history
  * Introduced in ORMMA version: 1.0.0
  * Included in MRAID version: 1

This event is thrown whenever an SDK error occurs. The event contains a description of the error that occurred and, when appropriate, the name of the action that resulted in the error (in the absence of an associated action, the action parameter is null). JavaScript errors remain the full responsibility of the ad designer.

> parameters:
    * message (String) : description of the type of error
    * action (String) : name of action that caused error

> triggered by: anything that goes wrong

> level: 1

## headingChange`*` ##
```
	"headingChange" -> function(heading)
```
API version history
  * Introduced in ORMMA version: 1.0.0

This event is thrown when the devices compass direction changes.

> parameters:
    * heading (Number) : compass heading in degrees or -1 for unknown

> triggered by: a change in the device heading after the compass has been activated by registering a "heading" event listener.

> level: 2

## keyboardChange`*` ##
```
	"keyboardChange" -> function(open)
```
API version history
  * Introduced in ORMMA version: 1.0.0

This event is thrown when the software keyboard is opened or closed for text entry in an ad.

> parameters:
    * open (Boolean) : true if keyboard is open, false if keyboard is not open

> triggered by: a change in the state of the virtual keyboard after registering a "keyboard" event listener.

> level: 2

## locationChange`*` ##
```
	"locationChange" -> function(lat, lon, acc)
```
API version history
  * Introduced in ORMMA version: 1.0.0

This event is thrown when the device has successfully geolocated itself.

> parameters:
    * lat (Number) : latitude value of device
    * lon (Number) : longitude value of device
    * acc (Number) : accuracy of the reading

> triggered by: a change in the device heading after the GPS has been activated by registering a "location" event listener.

> level: 2

## networkChange`*` ##
```
	"networkChange" -> function(online, connection)
```
API version history
  * Introduced in ORMMA version: 1.0.0

This event is thrown when the device network connection changes, such as loosing or acquiring an Internet connection. The connection type values will vary depending on the device and carrier.

> parameters:
    * online (Boolean) : true – device is connected to the Internet, false – device cannot access the Internet
    * connection (String (enumerated)) : description of connection type such as none, wifi, or cell

> triggered by: a change in the state of the network after registering a "network" event listener.

> level: 2

## orientationChange`*` ##
```
	"orientationChange" -> function(orientation)
```
API version history
  * Introduced in ORMMA version: 1.0.0

This event is thrown when the application screen orientation changes.

> parameters:
    * orientation (Integer) : degrees from upright portrait view, expected values are 0, 90, 180, 270

> triggered by: a change in the device orientation after registering an "orientation" event listener.

> level: 2

## ready ##
```
	"ready" -> function()
```
API version history
  * Introduced in ORMMA version: 1.1.0
  * Included in MRAID version: 1

The ready event triggers when the SDK is fully loaded, initialized, and ready for any calls from the ad creative.

It is the responsibility of the container to prepare the API methods before the ad creative is loaded. This prevents a condition where the ad cannot register to listen for the ready event because the API methods are unavailable. However, the container may still need more time to initialize settings or prepare additional features.

The ad should always attempt to wait for the ready event before executing any rich media operations. Because of timing issues, such as the ready event firing before the ad has registered to listen, ad designers should use the ready event in conjunction with the getState() method.

```
 function showMyAd() { ...
}
if (mraid.getState() === 'loading') { mraid.addEventListener('ready', showMyAd);
} else {
}
showMyAd();
```
> parameters:
    * none

> triggered by: The ready event triggers when the SDK is fully loaded, initialized, and ready for any calls from the ad creative.

> level: 1

## response ##
```
	"response" -> function(url, response)
```
API version history
  * Introduced in ORMMA version: 1.0.0

This event is thrown when a request action with a display type of "proxy" returns a response.

> parameters:
    * url (String) : the URL of the original request action
    * response (String) : the full body of the response

> triggered by: a request() method call returning.

> level: 2

## screenChange`*` ##
```
	"screenChange" -> function(width, height)
```
API version history
  * Introduced in ORMMA version: 1.0.0

This event is thrown when the device screen size changes.

> parameters:
    * width (Number) : the width of the screen in pixels
    * height (Number) : the height of the screen in pixels

> triggered by: a change in the device orientation after registering a "orientation" event listener.

> level: 2

## shake`*` ##
```
	"shake" -> function()
```
API version history
  * Introduced in ORMMA version: 1.0.0

This event is thrown when the device accelerometer detects that the device has been "shaken" as defined by the getShake parameters.

> parameters:
    * none

> triggered by: The device if a shake gesture is detected after registering a "shake" event listener.

> level: 2

## sizeChange ##
```
	"sizeChange" -> function(width, height)
```
API version history
  * Introduced in ORMMA version: 1.0.0

This event is thrown when the display state of the web viewer changes.

> parameters:
    * width (Number) : the width of the view in pixels
    * height (Number) : the height of the view in pixels

> triggered by: a change in the view size as the result of a resize, expand, close, orientation, or the app after registering a "size" event listener.

> level: 1

## stateChange ##
```
	"stateChange" -> function(state)
```
API version history
  * Introduced in ORMMA version: 1.0.0
  * Included in MRAID version: 1

This event fires when the state is changed programmatically by the ad or by the environment. This event is thrown when the Ad View changes between default, resized, expanded, and hidden states as the result of an expand(), resize() or a close(). The SDK may also close an ad as the result of a user or system action, such as resuming from background.

Note that "resized" is currently only available in ORMMA.


> parameters:
    * state (String (enumerated)) : either "loading", "default", "resized", "expanded", or "hidden"

> triggered by: resize, close, hide, show, expand, or the app

> level: 1

## tiltChange`*` ##
```
	"tiltChange" -> function(x, y, z)
```
API version history
  * Introduced in ORMMA version: 1.0.0

This event is thrown when the device has successfully determined its spacial orientation.

> parameters:
    * x (Number) : the x axis value in radians
    * y (Number) : the y axis value in radians
    * z (Number) : the z axis value in radians

> triggered by: a change in the device tilt after the accelerometer is activated by registering a "tilt" event listener.

> level: 2

## viewableChange ##
```
	"viewableChange" -> function(boolean)
```
API version history
  * Introduced in ORMMA version: 1.1.0
  * Included in MRAID version: 1

This event is thrown when the container showing the ad changes from on-screen to off-screen and vice versa.

> parameters:
    * boolean (Boolean) : true - container is on-screen and viewable by the user, false - container is off-screen and not viewable

> triggered by: a change in the application view controller

> level: 1