# Description of Issues And Solutions #

## Introduction ##
As display advertising in mobile applications and on the mobile web has become more and more viable over the last several years, various innovative companies have taken on the challenge of creating an ecosystem for mobile ad serving. This innovation has led to many exciting possibilities for content publishers and advertisers, but it also has created inefficiencies that often delay and inhibit the optimal monetization of content.

Simplifying the process for designers of ad creatives significantly increases the likeliness that agencies will include mobile into their media buys. Advertisers want to review compelling creative, approve it and decide to buy a specific inventory of mobile media, regardless which device platform, application, and technology is used to display the media.

In today's mobile advertising ecosystem, this buying scenario is simply not possible. It should be possible to run a rich media cross platform campaign without requiring various device specific rich media vendors and ad servers. It should be possible to serve the rich media ad units to multiple applications and mobile sites and collect the metrics in one report.

All this is possible, as long as the ad unit's technical requirements do not exceed the common capabilities of smartphone and tablet web browsers or applications. While some devices may have special features and audience reach that an advertiser may decide to target, running ads reliably across devices and platforms serves a greater advertising goal.

Ad units should be created and delivered in a simplified manner so they are compatible with multiple software environments such as application environments and web browsers on various mobile smart phones. **The simplest route for cross platform interoperability will be to extend support for existing standard web technologies like HTML5 and JavaScript into applications.**

## Purpose ##
This document covers some proposed standards for interoperable rich media display advertising in mobile applications. It outlines the requirements and scenarios for flighting a compelling cross platform campaign with advanced creatives. This document can serve as a specification or just as a guide for developers who want to integrate HTML ads into an application on smartphones or tablets.

The specification is currently stable as version 1.0, but is under constant review. Contributions are welcome.

## Audience ##
The intended audience includes product developers and product managers who are active in the integration or development of mobile advertising infrastructure. This document covers concepts and problems "from the field" that require a shared solution from organizations that develop open mobile ad serving SDKs, ad units, or applications.


## Mobile Rich Media Advertising Background ##

### History ###
The serious constraints on data transmission speeds for mobile phones have limited mobile ads in size and formats. On feature phones, user input is tedious. An ad click typically results in displaying an advertiser's small micro site.
When 'smart phones', 'app phones' or 'super-smart phones' emerged, advertising initially continued within the same constraints, but advertisers were not satisfied with the poor experience in this rich environment. Innovations proliferated with ads that go beyond the banner, or rich media ads. This is also where most of the troubles with interoperability arise.

This section attempts to define the concepts of mobile rich media advertising for the specification.

### Definition ###
Mobile Rich Media ad units are mobile compatible ad units with various measurable, interactive options which drive greater brand engagement and messaging across to end-users compared to basic banner ads. Optionally, the ad units can contain animations, video and other advanced graphics which help the advertiser communicate their message. Optionally, the ad unit can capture information from the end-user to continue engagements at other times or via other media.

### Features ###
Mobile Rich Media ads may include one or more of the following features:
•	can expand with more content when clicked
•	can include or link to video/audio content
•	can include basic animations that can interact with the end-user
•	can include ways for sharing or saving ad content
•	can be dynamically composed so the ad content is targeted to the end-user

### Benefits ###
Historically, the benefits of Mobile Rich Media include:
•	leveraging the unique nature of mobile end-user behavior and usage;
•	more captivating experience;
•	it's a better presentation of a brand;
•	allows for higher engagement with the end-user;
•	improves the monetization rates to the benefit of publishers and ad networks alike.

## Stakeholder Needs ##

### Application Developers ###
Easier distribution and monetization are the primary concerns for application developers. Choosing a technology should not limit businesses options for mobile ad campaigns.

For example, to be part of major advertising campaigns may require solutions that go beyond basic banner ads. So while particular ad servers have unique capabilities, all should support a variety of ad units beyond those designed specifically for their technology.

Application developers already write applications that detect and use the entire available size of the display. They should not need to recompile applications just because the ad related code cannot adjust to devices with a larger screen and a larger available ad area.

Application developers must also know that their ad related code will not affect other elements of their applications. Engaging ads that interact with the end-user should not interrupt the application, and must return to the state users were in before interacting with the ad.

Application developers are responsible for making the application perform properly under all common conditions and within the memory constraints of the environment, so a light memory footprint of the ad integration is desired.

### Advertisers ###
Advertisers, brands and agencies require a simplified process for developing ad creatives and for media buying. The mobile channel is just one of many. For greatest reach, advertisers must be able to approve creatives that can be trafficked into multiple applications, multiple mobile sites, or wherever their preferred audience can be reached.

If an ad unit does not behave similarly across different media, the campaign becomes too complicated and the mobile channel is excluded.

Especially for premium inventory, advertisers need the option to run rich media ad campaigns. Not just to one platform like the iPhone, but to several of the most popular devices. Advertisers have standing relationships with various premium content publishers on the desktop web and want to buy media that also works on publisher's downloadable applications, regardless of the mobile ad serving vendor.

For billing and payment, impression and engagement metrics for the campaign must be collected in a similar fashion.

### End-Users ###
Ad designers often break usability rules to grab the user's attention. However, some consistency still helps with user interaction and engagement, especially on the smaller screens of mobile devices.

As Neilsen and others have documented, consistency in an end-user experience is a key to success. For example, users must be able to see the difference between advertising content and publisher content. This is true for publisher content on a mobile site and within an application. Ads should be in the same predictable space and if the ad expands, users should quickly be able to identify how to close it. Once closed, the ad should display in a minimum state (banner or smaller).

For best engagement, users should be able to put their current task "on hold" and return directly to their activity after closing an ad. In most scenarios, this means keeping the user on the site, or in the application, during the interactions. Finally, fewer well-targeted advertisements are preferred over frequent generic ads.

## Standardization Plan ##

### Path to Interoperability for Vendors ###
The specifications below intends to honor all these experiences by identifying a technological approach that can be implemented by multiple companies. Identifying others that supply ad serving technology and are willing to collaborate on standards is necessary to improve interoperability -- and fulfill the promise of great mobile advertising.

To reach a version 1 specification, a handful of cooperating partners authored these pages to identify behaviors of an open in-app SDKs, the naming of the API's to invoke native app functionality, and the common technology used to create and deliver the ad units.

In a future phases, we intend to showcase actual ad campaigns using early implementation among cooperating vendors. In addition, this initiative is currently under review by members of the MMA with interest growing at other mobile standardization bodies that are positioned to identify and refine an industry standard.

### Current Mobile Device/Platform Focus ###
While version 1 specification concentrated on features available on the mobile web and iPhone OS, project members are also implementing for additional platforms and vendor participation is encouraged. For advertisers, the most used and deployed application environments are the most interesting. Limiting standards to any specific list of device capabilities hurts adoption. Ultimately, these standards should not limit vendors from providing additional features that are unique to their business or technology.