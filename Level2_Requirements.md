# Ad Controller Requirements: Level 2 #

### Access to Native Features ###
Using standard web technologies in the ad design, and relying on a web viewer in the app to render ads supports many presentation needs. But for truly rich media advertising, there must also be support for device features -- many of which are normally only available to application developers. Devices offer a wide array of functionality beyond simple display of ad content for rich media ads. The Controller provides a layer of abstraction between the ad designer and the device to greater enable cross-device media creation.

Dynamic properties require an event listener strategy. Using the addEventListener method allows the ad developer to access all native features the SDK supports.

Knowing what features are available to listen for, and what the event names are requires a naming convention. For each feature a Controller supports, the getter method is “get”+feature name and the event name is feature name+”Change”. For example, if a Controller supports the device’s native location capabilities, the supported feature is “location”, to get the location the ad developer would call “getLocation”, and to listen for changes the ad developer  would addEventListener for "locationChange".

As devices differentiate, or hardware vendors innovate, additional native features can be added using the same naming convention.

**supports** method

Use this method to determine whether a specific device feature is supported. Although these features, getters, and events are identified as Level-2 compliance, the supports method must be implemented by all SDK vendors.

The controller should support as many of the following features as is possible for a given device.

| **value**             | **description** |
|:----------------------|:----------------|
|network                |the device can report on its network connectivity and connectivity changes|
|orientation            |the device can report on its orientation and orientation changes|
|screen                 |the device can report on the screen size|
|heading                |the device can report on the compass direction it is pointing|
|location               |the device can report on its location|
|shake                  |the device can report on being shaken|
|sms                    |the device can send an SMS message|
|tilt                   |the device can report on its tilt and tilt changes|
|phone                  |the device can make a phone call|
|email                  |the device can compose an email|
|calendar               |the device can create a calendar entry|
|camera                 |the device can take a still picture image|
|screenshot             |the device can take a screenshot picture image|
|video                  |the device can play video|
|audio                  |the device can play audio|
|map                    |the device can display a location on a map|
|level-1                |the SDK supports ORMMA level 1 functionality|
|level-2                |the SDK supports ORMMA level 2 functionality|

### Working with the Device's Physical Characteristics ###

Most devices have several different kinds of sensors that can report on various physical characteristics of the device, such as its location, the direction it is pointing, its orientation, and its motion.

It's important to know that requesting a device's hardware features impacts physical properties such as battery life and available memory. Ad designers should only request native features on an as-needed basis and use removeEventListener when the feature is no longer needed.

**getScreenSize** method

The getScreenSize method returns the current size of the device screen. To receive updates on screen size changes use addEventListener for "screenChange" events.

**screenChange** event

The screenChange event fires when the devices screen size changes, usually as the result of an orientation change.

**getHeading** method

The getHeading method alone returns the last compass heading of the device. The heading may be unknown or out-of date.

To activate the compass and receive updates, use addEventListener for "headingChange" events.

**headingChange** event

The headingChange event fires when the devices compass direction changes.

**getLocation** method

The getLocation method alone returns the last location reading and accuracy of the device. The location may be unknown or out-of-date.

To activate the location system and receive updates use addEventListener for "locationChange" events.

**locationChange** event

The locationChange event fires when the devices location changes.

**getOrientation** method

The getOrientation method returns the current device orientation.

To receive updates on orientation changes use addEventListener for "orientationChange" events.

**orientationChange** event

The orientationChange event fires when the device is rotated or tilted to a new orientation.

**getTilt** method

The getTilt method returns the last reported device tilt readings in 3 dimensions.

To receive updates on tilt changes use addEventListened for "tiltChange" events.

**tiltChange** event

The tiltChange event fires when the devices 3 dimensional tilt values change.

### Controlling shakeProperties ###

When an ad calls the shake method, the way the ad expands depends on the shakeProperties. The shakeProperties are held in a JSON object that can be written and read by the ad.

At a minimum, the following properties should be supported.
  * "intensity" : float,
  * "interval" : float

default values are platform-specific

**getShakeProperties** method

The getShakeProperties method returns the current thresholds that define a shake gesture. The defaults should be sufficient in most cases, but setShakeProperties is available as required.

**shake** event

The shake event fires when the device is shaken within the thresholds of the current shake properties.

**setShakeProperties** method

The setShakeProperties will set ad specific thresholds for what is interpreted by the SDK as a shake gesture. The default values should be sufficient in most cases and ad designers are not required (nor encouraged) to use setShakeProperties.

### Working with Device Connectivity ###

**getNetwork** method

The getNetwork method returns the current network connection type for the device. To receive updates on network changes use addEventListener for "network" events.

**networkChange** event

The networkChange event fires when the devices network connectivity changes.

### Working with Native Applications ###

Web protocols such as sms: or mailto: allow ad designers to include applications into their rich media ads by simply using URLs. These protocols are not always consistent across devices.

This specification recommends using methods instead to ensure that the ad and current application are politely suspended while the user takes action.

**createEvent** method

The createEvent method opens the device UI to create a new calendar event with default values provided. The ad is suspended while the UI is open.

**makeCall** method

The makeCall method opens the device UI for making a phone call to a specified number. The ad is suspended while the UI is open.

**sendMail** method

The sendMail method opens the device UI for sending an email message with the content provided. The ad is suspended while the UI is open.

**sendSMS** method

The sendSMS method opens the device UI for sending an SMS message with the content provided. The ad is suspended while the UI is open.

### Handling Call-to-Action Events ###
A rich media ad implements multiple call-to-action events beyond the click to microsite. These events may be executed as anchor links or scripted functions. This means an SDK cannot just listen for clicks in the browser. It must support programmatic clicks as well.