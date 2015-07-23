# Testing Ads for ORMMA Compliance #

The ORMMA WebTester provides a method for ad designers to test their ads using a browser. If your ad works with the WebTester, then it is following the standard API.

## General Requirements ##
Your ad should be developed in HTML, CSS and JavaScript. These web standards allow for interoperability between platforms. Plugin vendors may support ORMMA at their discretion.

Your JavaScript should contain the global function ORMMAReady(). This function is called by the ORMMA-compliant SDK to initialize your ad for display.

It is recommended that your ad have something to display before ORMMAReady is called, such as an image.

Follow the documentation at the API reference for all your calls to
  * expand, resize and collapse
  * send trackers and cache assets
  * access native features

You do _NOT_ need to include an ORMMA JavaScript library. This is provided by the SDK.

Most development testing has been done against the Safari browser. However, some users report that Firefox works in most circumstances. For best results, please use Safari.

## Test Online ##
The WebTester is available online. Although this may not be the absolute latest version, it is built from the most recent stable release.
> http://ormma-tester.appspot.com/

## Download the WebTester ##
As an alternate, you can run the web tester locally by downloading the ORMMA source code. To limit cross-browser scripting errors, download the source files to your computer and run them in a local web server.
> http://code.google.com/p/ormma/source/browse/#svn%2Ftrunk%2FWebTester.

## Prepare ##
On the prepare tab, set the size of your device as well as the ad area. You can enter numbers on the form, or resize the boxes to visually configure your scenario.

## Flight ##
On the flight tab, enter a code-snippet like you would when trafficking a third-party HTML ad in an ad server.

As an alternate, you can enter a fully qualified URL. If the URL does not include a `<head>` and `<body>` tag, then be sure to select the "Fragment" checkbox.

Click "Render" to see your ad in a pop-up window.

## Test ##
A new window is created to display your ad. Be sure to enable pop-ups and disable open new windows in a tab for your browser.

The console displays Errors and Info messages from the WebTester. Your ad is compliant if there are no errors and the ad displays/behaves as you expect it to in the pop-up.