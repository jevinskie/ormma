_Note: This page is out of date and will be brought back into line with the spec as development proceeds._

# Specifications for iOS Reference Implementation #



## Audience ##
This section is intended for SDK developers responsible for implementing these guidelines for rich media interoperability on Apple’s iOS platform.

## Implementation Notes for iOS Developers ##
The primary method for providing native access to the web view is to use the "stringByEvaluatingJavaScriptFromString" method. This allows the SDK developer to inject code into the web view for the JavaScript developer to access, as well as provide listeners on the native side for web view triggers.

As an example, to implement an outbound endpoint, or event trigger for shaking, the SDK developer would listen for UIEventSubtypeMotionShake, then throw the JavaScript event “shakeChange” with the method stringByEvaluatingJavaScriptFromString.

## Overview ##
This standard defines a native AdView container for Rich Media Ads to display in Apps and a JavaScript AdController for Ad Creatives to interact with the AdView and the device. The AdView encapsulates an XHTML and JavaScript enabled Web browser view, such as iOS’s UIWebView, and a bridge that can integrate Ads with the OS’s capabilities.

Example: iOS Components

![http://media.cw.s3.amazonaws.com/ormma/iOSComponents.png](http://media.cw.s3.amazonaws.com/ormma/iOSComponents.png)

The reference SDK implements the Container on iOS 3+ devices using an Objective-C subclass of UIView called AdView. AdView instantiates a child UIWebView, sets itself as the Web view’s delegate, and implements the UIWebViewDelegate methods.

The JavaScript AdController class communicates with the Objective-C AdView using a custom URI scheme that is inspected and dispatched in the UIWebViewDelegate [shouldStartLoadWithRequest…] method. The URI scheme is:

**ormma:command:parameter1:parameter2:…:parameterN**

The first element is always the static string “ormma”. The second parameter is a sting representing a command name. Additional parameters, if present, are an ordered list of parameters specific to the command and defined by convention.

The Objective-C AdView class communicates with the JavaScript AdController class by calling the UIWebView’s [stringByEvaluatingJavaScriptFromString…] method with a JavaScript function call that modifies the state of the static AdController JavaScript class. This interface is private and defined by convention.