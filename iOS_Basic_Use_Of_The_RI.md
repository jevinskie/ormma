# Important Concepts #

Before you start using the ORMMA Reference Implementation, you should understand that its total purpose is the display of rich media creatives. It does not in itself make any determination as to what creative it should show nor does it interface directly with products such as Double Click.

The basic flow of events in your code are:

  1. determine where on your view you wish to place an ad
  1. create an ORMMAView instance sized and positioned appropriately
  1. implement any delegate methods you wish and let the view know the delegate
  1. retrieve an ad from the source of your choice
  1. pass the ad to the ORMMAView


# Interface Builder #

The ORMMA Reference Implementation is compatible with Interface Builder. To use Interface Builder to layout your view, simply add a UIView to your view, position it where you would like it and size it as you desire. Generally speaking, it will be a 320x50 banner for portrait views and a 480x50 banner for landscape.

Once this has been done, simply change the type of the object in Interface Builder to be an ORMMAView instead of a UIView and wire things up as you need.

# The ORMMAView Delegate #

The ORMMA View has a delegate that you MUST set. This delegate must implement exactly one method:

`- (UIViewController *)ormmaViewController`

This method should return a handle to a view controller and is usually the delegate itself. This view controller will be used to display any modal dialogs that a creative may need to fire.

Additionally, there are a number of points within the life time of the creative that the ORMMAView will call back to the delegate and give it an opportunity to do something. These are as follows:

`- (NSString *)javascriptForInjection` - implement this method if you want to include javascript inside each creative. This is primarily useful if your application needs to inject data into the advertisement.

`- (void)failureLoadingAd:(ORMMAView *)adView` - called if the ad cannot be loaded

`- (void)willResizeAd:toSize:` - called just before an ad resizes

`- (void)didResizeAd:toSize:` - called just after an ad resizes

`- (void)adWillShow:(ORMMAView *)adView` - called just before an ad becomes visible

`- (void)adDidShow:(ORMMAView *)adView` - called just after an ad becomes visible

`- (void)adWillHide:(ORMMAView *)adView` - called just before an ad becomes invisible

`- (void)adDidHide:(ORMMAView *)adView` - called just after an ad becomes invisible

`- (void)willExpandAd:(ORMMAView *)adView toFrame:(CGRect)frame` - called just before an ad expands

`- (void)didExpandAd:(ORMMAView *)adView toFrame:(CGRect)frame` - called just after an ad expands

`- (void)adWillClose:(ORMMAView *)adView` - called just before an ad closes

`- (void)adDidClose:(ORMMAView *)adView` - called just after an ad closes

`- (void)appShouldSuspendForAd:(ORMMAView *)adView` - called when the ad becomes modal, either expanded or shows a full screen window. The intent is to provide a hint that the application may stop heavy weight tasks until the ad releases modality.

`- (void)appShouldResumeFromAd:(ORMMAView *)adView` - called when the ad releases full screen or modal view. The application should restart any tasks that were paused.

`- (void)placePhoneCall:(NSString *)number` - allows the application to handle placing a phone call. The intent is to allow the application to inject itself into the flow, to display an alert for example.

`- (void)createCalendarEntryForDate:(NSDate *)date title:(NSString *)title body:(NSString *)body` - allows the application to handle creation of calendar events if desired. By default calendar events are handled directly without user interaction. If the application wants to present an alert to the user, this method should be implemented.

`- (void)showURLFullScreen:(NSURL *)url sourceView:(UIView *)view` - Allows the application to inject itself into the full screen browsers menu.

`- (void)emailNotSetupForAd:(ORMMAView *)adView` - called if the creative attempts to create an email, but the user does not have email setup. By default, no error is displayed, but this method gives the application the opportunity to take action (such as informing the user as to why the email is not going to happen)

`- (void)handleRequest:(NSURLRequest *)request forAd:(ORMMAView *)adView` - this method is called whenever a custom URL protocol is detected that has been previously registered by the application. It is expected that the application will handle the request.

- (NSString **)onLoadJavaScriptForAd:(ORMMAView**)adView` - if present, this method will be called to retrieve and execute any javascript that the application needs executed when a creative is loaded.


# The ORMMAView #

ORMMAView has a number of properties that your application may set. The most important of these are:

`ormmaDelegate` - used to set the delegate

`allowLocationServices` - used to control access to the location based services for privacy reasons.

`currentState` - allows the application to determine the current state of the currently running creative

`maxSize` - specifies the maximum size that a creative may "resize" itself to.

Additionally there are 3 methods that control most interactions with the ORMMAView. These are:

`-(void)restoreToDefaultState` - used by the application to force the currently running ad back to its default state. Usually used if an external action requires the user's attention.

`- (void)registerProtocol:(NSString *)protocol` - used to register a custom URL scheme with ORMMA that the application is expected to handle.

`- (void)loadHTMLCreative:(NSString *)htmlFragment creativeURL:(NSURL *)url` - used to display a creative through ORMMA. The "htmlFragment" represents the html to display, while the "url" represents the base URL and is used to differentiate like creatives. Generally, the "url" will be set to a constant value.
