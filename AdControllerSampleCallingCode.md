# Introduction #

This page provides examples of calling the methods and listening for events in the AdController. This code would work for any SDK that supports the ORMMA specification.



# Level 1 #
These calls focus on Level 1 compliance -- primarily, resizing the ad.


## Event handling ##
The following example code illustrates adding a listener:

```
var locateHandler = function(lat, lng) {
	alert('got ' + lat + ',' + lng);
};
ORMMA.addEventListener('location', locateHandler);
```

The following example code illustrates removing the listener that was added in the previous example:

```
ORMMA.removeEventListener('locate', locateHandler);
```

## Error handling ##
The following code expands upon the "locate" example to illustrate listening for an error event:

```
if (ORMMAe.supports('location')) {
	var onLocate = function(lat, lng) {
		alert('I\'m at ' + lat + ',' + lng);
		ORMMA.removeEventListener('location', this);
		ORMMA.removeEventListener('error', onLocateError);
	};
	var onLocateError = function(message, action) {
		if (action == 'locate') {
			alert('Geolocation is not available: ' + message);
			ORMMA.removeEventListener('error', this);
			ORMMA.removeEventListener('location', onLocate);
		}
	};
	ORMMA.addEventListener('location', onLocate);
	ORMMA.addEventListener('error', onLocateError);
}
```

## Controlling display ##
The following example illustrates setting, getting, and listening for changes to the display state:

```
if (ORMMA.getState() != 'visible') {
	ORMMA.addEventListened('state', function() {
		if(alert('Now you see me...');
		AdFrame.hide( function() {
			alert('Now you don\'t!');
		});
	});
}
```

The following example illustrates setting, getting, and listening for changes to the display state:

```
if (!AdFrame.isVisible()) {
	AdFrame.addEventListener('show', function() {
		alert('Now you see me...');
		AdFrame.hide();
		AdFrame.removeEventListener('show', this);
		AdFrame.addEventListener('hide', function() {
			alert('Now you don\'t!');
			AdFrame.removeEventListener('hide', this);
		});
	});
	AdFrame.show();
}
```

## Resizing display ##
The following example illustrates resizing, evaluating dimensions and properties, and listening for resize events:

```
var resizeListener = function(d, p) {
	alert('resized view to ' + (d.right-d.left) + 'x' + (d.bottom-d.top));
	AdFrame.removeEventListener('resize', this);
};

var d = AdFrame.dimensions();
var p = AdFrame.properties();
d.bottom = d.bottom + 100;
p.transition = 'slide';

AdFrame.resize(d, p, resizeListener);
```

Sample calling code would then look like

```
function bannerOnclick() {
 AdFrame.resize(
 { 0, 0, MAX_HEIGHT, MAX_WIDTH },
 {"transition":"roll","navigation":"close","use-background":"false","is-modal":"true"},
 function() {
 document.getElementById('interstitialad').style.display = 'block';
 document.getElementById('bannerad').style.display = 'none';
 }
 );
 return (false); //do not allow click event to bubble up
}
function panelCloseOnclick() {
 AdFrame.resize (
 {0, 0, INITIAL_HEIGHT, INITAL_WIDTH},
 {"transition":"roll","navigation":"none","use-background":"false","is-modal":"false"},
 function() {
 document.getElementById('bannerad').style.display = 'block';
 document.getElementById('interstitialad').style.display = 'none';
 }
 );
 return (false); //do not allow click event to bubble up
}
```

...

```
<div id="interstialad" style="display:none">
 <a href="#" onclick="return(panelCloseOnclick())"><img src="http://advertiser.url/close_button.gif"/></a>
 <a href="http://m.advertiser.url"><img src="http://advertiser.url/fullpage.gif"/></a>
</div>
<div id="bannerad">
 <a href="#" onclick="return(bannerOnclick())"><img src="http://advertiser.url/banner.gif"/></a>
</div>
```

# Level 2 #
These calls focus on Level 2 compliance -- primarily, access to native features.

## native features ##
### supports ###
The following example demonstrates feature inspection:

```
if (AdFrame.supports('location')) {
	AdFrame.addEventListener('locate', function(lat, lng) {
		alert('I\'m at ' + lat + ',' + lng);
		AdFrame.removeEventListener('locate', this);
	});
	AdFrame.locate();
}
```

### alignment ###
The following example demonstrates polling for a "alignmentChange" event:

```
AdFrame.addEventListener('alignmentChange', function(direction) {
	alert('I\'m pointed ' + direction + ' degrees from North.');
});
AdFrame.getAlignment();
```

### location ###
The following example demonstrates polling for a "locationChange" event:

```
AdFrame.addEventListener('locationChange', function(lat, lng) {
	alert('I\'m at ' + lat + ',' + lng);
});
AdFrame.addEventListener('error', function(action, error) {
	if (action=="location") {
		alert("I don't know where I am");
	}
});
AdFrame.getLocation();
```

### orientation ###
The following example demonstrates receiving an "orientationChange" event:

```
AdFrame.addEventListener('orientationChange', function(orientation) {
	if (orientation == 180) {
		alert('Oh no - I flipped over!');
	}
	AdFrame.removeEventListener('orientationChange', this);
});
```

### network ###
The following example demonstrates receiving a "networkChange" event:

```
AdFrame.addEventListener('networkChange', function(isOnline, connectionType) {
	if (isOnline) {
		alert('Watch streaming movies now');
	} else {
		alert('Streaming movies are not available');
	}
});
```

### screenSize ###
The following example demonstrates receiving a "screenChange" event:

```
AdFrame.addEventListener('screenChange', function(width, height) {
	alert('Wow - the screen is ' + width + 'x' + height);
	AdFrame.removeEventListener('screenChange', this);
});
```

### shake ###
The following example demonstrates receiving a "shakeChange" event:

```
AdFrame.addEventListener('shakeChange', function(threshold, time) {
	if (threshold >= 10 && time >= 30) {
		alert('Kaboooom!');
		AdFrame.removeEventListener('shakeChange', this);
	}
});
```

### tilt ###
The following example demonstrates polling for a "tiltChange" event:

```
AdFrame.addEventListener('tiltChange', function(x, y, z) {
	alert('I\'m pitching ' + y + ' degrees from straight up and down.');
});
setTimeout(AdFrame.getTilt(), 100);	
```

## call-to-action ##
### request ###
The following example demonstrates usage of the request action and response event:

```
<script type="text/javascript">
AdFrame.addEventListener('response', function(uri, response) {
	if (uri == 'http://a.com/some/uri.xml') {
		alert(response);
		AdFrame.removeEventListener('response', this);
	}
});
AdFrame.request('http://a.com/some/uri.xml', 'proxy');
</script>
<a href="http://a.com/b.html" 
 onclick="AdFrame.request(this.href, 'external')">Open a web browser</a>
```

# Level 3 #
These calls focus on Level 3 compliance -- offline display and tracking.

## Asset management ##

```
AdFrame.addAssets({
	'/tmp/one.png':'http://my.domain.com/image1.png',
	'/tmp/two.mp4':'http://a.domain.org/someMovie.mp4',
	'/tmp/three.js':'http://an.adserver.com/some/ad/request?to=execute'
});
```

This code sample illustrates usage of the local asset cache:

```
<script type="text/javascript">
AdFrame.addAlias('/logo.png', 'http://a.com/my/logo.png');
AdFrame.addEventListener('assetReady', function(alias) {
	if (alias == '/newmap.png') {
		var img = document.createElement('img');
		img.src = '/newmap.png';
		document.body.appendElement(img);
		AdFrame.removeEventListener('assetReady', this);
	}
});
if (AdFrame.cacheRemaining() >= 50000) {
	AdFrame.addAlias('/newmap.png', 'http://a.com/big/img.png');
} else {
	AdFrame.addEventListener('assetRemoved', function(alias) {
		if (alias == '/oldmap.png') {
			AdFrame.addAlias('/newmap.png', 'http://a.com/big/img.png');
			AdFrame.removeEventListener('assetRemoved', this);
		}
	});
	AdFrame.removeAsset('/oldmap.png');
}
</script>
<img src="/logo.png">
```

# Complete code sample #
In a complex example, the Ad Creative uses the JavaScript AdController object to send directives to the AdContainer and interact with features of the device and OS. The initial Ad can be a small shim with a static background or a “Loading…” message and JavaScript that exposes an ORMMAReady() method.

The first signal the Ad gets from the SDK is from the SDK calling the ORMMAReady() method. Then the Ad does can check to see if the local assets it needs are already downloaded. If they are not, it downloads a new base HTML page to the local file system as well as all of the JavaScript, CSS, image, and other files it needs to execute locally. It also registers an Event Listener function for downloads and keeps track of the files that successfully download. Once all of the necessary files are present locally, the Ad instructs the AdController to tell the AdContainer to load the local HTML file, which then takes over as the body of and business logic of the Ad.
Once the local version of the Ad is loaded and in charge, it can reference the local assets and notify the AdController object that it can operate in off-line mode. The specifics of how an SDK handles off-line Ads are up to the SDK developer, but Ads that indicate that they can run off-line should have everything they need stored locally or modify their behavior if the AdController object indicates that the device is off-line.

When the Ad is ready to display, the SDK fills the native Web view with a banner or other in-view Ad unit. For this example, the only user interaction for this Ad’s banner state is to expand when touched, so the Ad waits to be touched. When the user touches the Ad, the Ad’s JavaScript uses the AdController object to notify the App that the Ad is expanding so that it can stop anything that the user will not be able to interact with. The Ad then resizes the Container and Web view to take up the entire screen of the device using a transition of its choice.

As the example continues, when the Ad reaches full screen, its JavaScript registers Event Listeners for the accelerometer and shake gestures. The body of the expanded ad is a game where the user controls a graphic by manipulating the device’s orientation. The Event Listeners for the accelerometer data get called with the devices 3D orientation vectors at the desired interval and the Ad’s JavaScript controls the game rendering.

When the user is done with the example expanded Ad, they click a close button that causes the Ad to unregister the Event Listeners, return the Ad to its original size, display the Ad’s banner state, and notify the App that it can resume. If the device was off-line when the user interaction took place, any metrics called made by the Ad to the AdController object are cached by the SDK until the device is on-line again, and then the calls are made.

Code TBD