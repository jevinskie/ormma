# Ad Controller Requirements: Level 1 #

### Initialization ###
The Controller encapsulates and abstracts all interactions between the web layer and the native layer and identifies the SDK as compatible with these specifications.

**ORMMAReady** method

The window-scope method named exactly "ORMMAReady" will be called by the SDK once it is fully loaded and has attached to the underlying native code.

For ad designers, having a function named "ORMMAReady" is one of the most important methods to expose in your ad scripts. It will be called once by the SDK to initialize the bridge between native and web layers.

The ad designer should also wait for the ORMMAReady method to be called before taking any actions beyond initial state. This is your signal that the SDK is ORMMA compliant, that the libraries are ready for you, and that the ad state can be queries and listened to.


**supports** method

The supports method allows the ad to interrogate the SDK to both baseline functionality and specific device features.

### Preloading and Initial Display ###
It is up to the ad developer to provide simple HTML, such as an &lt;img&gt; tag, for the initial display of their ad while other assets are loaded in the background. This HTML will be displayed in the Container while JavaScript uses the Controller to request and invoke additional capabilities. Ultimately, the initial HTML display may be completely replaced by a rich media ad once all assets are ready, depending on the creative requirements.

### Event Handling ###
Event handling is a key concept of this recommendation. Communicating between the web layer and native layer is asynchronous by nature. Through event handling, the ad designer is able to listen for particular actions and respond to those actions on an as-needed basis. These specifications advocate broadcast-style events to support the broadest range of features/flexibility with the greatest consistency.

The controller exposes these methods.

**addEventListener** method

Use this method to subscribe a specific handler method to a specific event. In this way, multiple listeners can subscribe to a specific event, and a single listener can handle multiple events.

**removeEventListener** method

Use this method to unsubscribe a specific handler method from a specific event.

### Error Handling ###
When an error occurs, the "error" event is thrown with diagnostic information about the event. Any number of listeners can monitor for errors of different types and respond as needed.

**error** event

This event is thrown whenever an error occurs. The event contains a description of the error that occurred and, when appropriate, the name of the action that resulted in the error.

### Controlling Ad Display ###
Besides the initial display, the ad developer may have a number of reasons to control the display.
  * An application may load views in the background to help with latency issues so that an ad is requested, but not visible to the user.
  * The ad may expand beyond the default size over the application content.
  * The ad may resize within the application content.
  * The ad may return to the default size once user interaction is complete.
  * The ad may, in certain circumstances, hide itself.

**getState** method, **stateChange** event

Each ad has a state that is one of the following:

|**value**|**description**|
|:--------|:--------------|
|default  |the initial position and size of the ad as placed by the developer|
|expanded |the ad has expanded to cover the application content at the top of the view hierarchy|
|resized  |the ad has expanded within the view hierarchy, possibly moving application content|
|hidden   |the ad is in its default position, but is not visible|

The getState method returns the current state of the ad and the stateChange event fires when the state is changed programmatically by the ad or by the environment.

**getViewable** method, **viewableChange** event

In addition to the state of the ad in the container, it is possible that the container is loaded off-screen as part of an application's buffer to help provide a smooth user experience. This is especially prevalent in apps that employ scrolling views.

The getViewable method returns whether the ad is currently on or off the screen. The viewableChange event fires when the ad moves from on-screen to off-screen and vice versa.

**show** method

The show method will cause a hidden ad to become visible in the default position.

The show method will move the state from "hidden" to "default" and fire the stateChange event. If the state is not "hidden" there is no effect.

**expand** method

The expand method will cause the Web View to reposition itself at the highest z-order in the view hierarchy at the desired position and size. The view can either contain a new HTML document or a copy of the same document that was in the default position. While an ad is expanded, the default position's HTML is suspended. The transition is controlled by the expandProperties.

The expand method will move state from "default" or "resized" to "expanded" and fire the stateChange event. If the state is not in "default" or "resized" (or "expanded") there is no effect.

**resize** method

The resize method will cause the existing Web View to resize itself within the current view hierarchy using the existing HTML document. The transition is controlled by the resizeProperties.

The resize method will move the state from "default" to "resized" and fire the stateChanged event. If the state is not "default" (or "resized") then there is no effect.

_Note: In most cases, expand is the correct behavior for rich media ads. The resize method should only be used in views where application content can be change its size within the existing view hierarchy, for example an ad cell in a table view that can grow and push the cells above and below apart. Designers should use the getMaxSize method before calling resize._

**getMaxSize**

The getMaxSize method returns the maximum size an ad can resize to. This value defaults to the size of the screen but can be overridden by the app developer in native code. If an ad tries to resize larger than maxSize, then an error is thrown.

**getSize** method

The getSize method will return the current size of the ad.

**getDefaultPosition** method

The getDefaultPosition method returns the position and size of the default ad view regardless of what state the calling view is in.

**sizeChange** event

The sizeChange event fires when the ads size within the app UI changes. This can be the result of an orientation change of the device or calls to the resize or expand methods.

**close** method

The close method will cause either expanded or resized ads to return to their prior state.
  * In the case where an ad went from default to resized to expanded, close will return the ad to its resized size.
  * In the case where an ad went from default to expanded, close will return it to its default size.
  * In the case where an ad went from default to resized, close will return it to its default state.

The close method will move the state from "expanded" to "resized" if expand was called from a resized view and from "expanded" to "default" if expand was called from a default view. It will also move the state from "resized" to "default" and, in both cases, fire the stateChange event. If the state is not "expanded" or "resized" there is no effect.

**hide** method

The hide method will cause ads in their default state to hide themselves from the view they are in.

The hide method will move the state from "default" to "hidden" and fire the stateChanged event. If the state is not "default" then there is no effect.

### Controlling expandProperties ###

When an ad calls the expand method, the way the ad expands depends on the expandProperties. The expandProperties are held in a JSON object that can be written and read by the ad.

At a minimum, the following properties should be supported.
  * "useBackground" : boolean true|false,
  * "backgroundColor" : string formatted as "#rrggbb",
  * "backgroundOpacity" : float "n.n",
  * "lockOrientation" : boolean true|false


default values are

```
{
 useBackground:false,
 backgroundColor:'#ffffff',
 backgroundOpacity:1.0,
 lockOrientation:false
}
```


**getExpandProperties** method

The getExpandProperties method returns the whole JSON object.

**setExpandProperties** method

The setExpandProperties method sets the whole JSON object.

### Working with a Virtual Keyboard ###

On devices that have a virtual keyboard, the display of the keyboard can affect the ad display.

**keyboardChange** event

The keyboardChange event fires when the virtual keyboard opens or closes.

### Hyperlinks ###

Rich media ads can have HTML hyperlinks in them, but the ad developer needs to be careful about using them. Loading a new web page in the ad view that is not written to the ORMMA spec can leave the ad, and possibly the app, in an unusable state. Additionally, some devices override or implement certain URLs in their own applications, such as mail, maps, calendar, SMS, and phone calls. ORMMA provides methods for those functions in the Level-2 specification.

### Opening an Embedded Browser ###

If the ad wants to open an external mobile web site, or micro site, from an ORMMA ad, it can call the open method which will open an embedded browser window in the application.

**open** method

The open method will display an embedded browser window in the application that loads an external URL.

_Note: This should be used only for external web pages that are not ORMMA ads. The displayed page will not load the ORMMA SDK and the close method will not have any affect on the embedded browser. It can only be closed by the user selecting the close control for the window, which is implementation specific._