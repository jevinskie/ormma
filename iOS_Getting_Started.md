# Introduction #

Integrating the iOS Reference implementation into  your iOS application is a relatively straightforward process.

# Prerequisites #

The iOS Reference Implementation is focused on implementing and delivering the ORMMA specification; not on reinventing the wheel. It therefore makes use of the following common, tested, and freely licensed libraries for use on the iOS Platform:

  * ASI HTTP Request
  * Cocoa HTTP Server
  * Code by Erica Sadun
  * FMDB
  * JSON Framework

By default, all of these are included in the iOS Reference Implementation source tree and will be included in the built static library. If your application is already making use of any of these third party products, you may either rely on the versions supplied by ORMMA or you may want to change the way ORMMA is built to remove these from being included in the ORMMA static library.

# How to get ORMMA #

Using the Subversion client of your choice, download the ORMMA source tree from XXXXX.

Once you have done so, we recommend that you attempt to open the ORMMA project and build it to make sure there are no problems.

To build, make sure your target is set to "ORMMATestBed". This will construct both the static library and the iOS test bed. The static library created will work for both an in-simulator application as well as an on-device application.

# How to Include ORMMA in Your Project #

You will need to add the following 3 files from the build/Debug-universal directory of the ORMMA project to your project.

  * libORMMA.a
  * ORMMA.bundle
  * ORMMAView.h

Make sure that ORMMA.bundle is part of your target's "copy bundle resources" phase, and that libORMMA.a is part of your target's "link" phase.

# Required Frameworks #

Make sure to include the following frameworks in your application:

  * CF Network
  * Core Foundation
  * Core Graphics
  * Core Location
  * _Core Motion_
  * _Event Kit_
  * _Event Kit UI_
  * Foundation
  * libsqlite3.0.dylib
  * libz.1.2.3.dylib
  * _Message UI_
  * _Mobile Core Services_
  * Quartz Core
  * System Configuration
  * UI Kit

(frameworks in _italics_  may be weak linked, all others are required)

The reference implementation is compatible down to iOS 3.1.3 and does support weak linking of features.