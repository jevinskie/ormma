# Introduction #

The AdController JavaScript object is a singleton and self-initializes when the Ad HTML loads the JavaScript include containing that SDK’s AdController implementation.



# Details #

### Registration Object for Synchronous Methods ###
Nearly all interactions are asynchronous because of the requirements to communicate with the native layer with location.href commands. However, the AdController may use a registration object to retrieve initial/static values from the native layer. It is then the responsibility of the AdController to maintain this model for methods that have return values.

Example properties to request during an initial registration include
  1. Methods with return values
    1. Size of local cache for cacheRemaining()
    1. Ad Viewer dimensions for dimensions()
    1. Ad Viewer properties for properties()
    1. Ad Viewer visibility for isVisible()
    1. Network status for getNetwork()
    1. Initial device orientation for getOrientation()
    1. Device screen size for getScreenSize()
    1. Supported device features for supports()
  1. SDK function names, so AdController provides a façade layer to existing code
  1. Commonly requested data, so AdController can optimize/minimize communications between layers
    1. Device location
    1. ~~Device ID~~ (this is no longer allowed per Apple privacy policies)
    1. Available transitions
    1. SDK version