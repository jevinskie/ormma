_Note: This page is out of date and will be brought back into line with the spec as development proceeds._

# Introduction #

The ORMMAViewDelegate protocol should be adopted by any UIView that has one or more ORMMAView subviews.


# API #

## Methods ##

-(void)adWillExpand:(ORMMAView)ad


-(void)adDidClose:(ORMMAView)ad


-(void)appWillSuspendForAd:(ORMMAView)ad


-(void)appWillResumeFromAd:(ORMMAView)ad