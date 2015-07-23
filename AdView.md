# Introduction #

The AdView object is a native iOS class that provides an ORMMA compliant ad container. A given App view can have one or more AdView subviews that will all act independently of one another. The visible UI of the AdView is a UIWebView control that the AdView wraps and manages. The parent App view should implement the AdViewDelegate protocol and set itself as the AdView's delegate to implement important methods necessary for the Ad lifecycle.

The primary method call of the AdView is loadRequest(URL), which causes the embedded Web view to load the specified document.

# Implementation Notes #

It is necessary that the Container be at the highest Z-order of any part of the UI that the Ad might cover and it is recommended that it be at the highest Z-order of all UI elements in the View.

When the developer wants the App to display an Ad, they generate a URL containing whatever targeting parameters are appropriate and pass it to the AdView's loadRequest (URL) method. The AdView then triggers the UIWebView to request the URL. There are several possibilities for the URL:

  * it may be an Internet (http:) URL that returns a complete HTML document to the AdView
  * it may be a local (file:) URL to a stub HTML file that will then, based on parameters in the request, will make subsequent calls (most likely to the Internet for JavaScript Ad content)
  * it may be a data (data:) URL containing the the body of an HTML document retrieved by alternate means

If the Ad does not make use of the ORMMA JavaScript object, it will behave as a normal, isolated HTML Ad and any links will open the OS’s default web browser. This fallback functionality allows fixed sized, non-interactive Web Creatives to be used without modification.

# API #

## Properties ##

state

## Methods ##
- (void)loadRequest:(NSURLRequest `*`)request

- (void)loadHTMLString:(NSString `*`)string baseURL:(NSURL `*`)baseURL

- (NSString `*`)stringByEvaluatingJavaScriptFromString:(NSString `*`)script

- (void)close: