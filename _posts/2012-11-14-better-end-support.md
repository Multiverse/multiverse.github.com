---
layout: post
category: news
header: Better End Support
author: dumptruckman
limit: 100
---
The [latest development builds](http://ci.onarandombox.com/view/Multiverse/) of Multiverse-Core and Multiverse-NetherPortals now has significantly better end support.  This is something that we have been lacking for quite a while now so I'm pretty excited to see it finally here.

A look at what has changed:
 - Multiverse-Core now sets the scale of all newly created End worlds to 16.  *This will help prevent it from spawning people very far away from the tiny End island due to 1:1 scale.*
 - Multiverse-Core now has configuration for vanilla portal search radius.  *This should fix issues with nether portals ending up at unexpected locations.  The search radius is normally 128 in CraftBukkit and Multiverse now uses 16 by default.*
 - Multiverse-NetherPortals will now cause End exit portals to take you to the linked Normal world's spawn.
 - Multiverse-NetherPortals now takes you to a linked world exclusively base on the type of portal you're using no matter what world type you're in.
 - Multiverse-NetherPortals now uses NORMAL priority for PlayerTeleportEvent.  *This removed the need for duplicate checks by simply setting the warp location THEN letting MV-Core do the rest*
 - Multiverse-NetherPortals now displays a more information for why you can't go through portals.
 - Bug fixes
  
I'd like to state that the **newer builds of Multiverse-Core may not be stable**.  I had to make some signficant changes in Multiverse-Core to fix a significant and deep rooted issue.  All of our built-in tests have passed and at least a handful of people are using them without issue.  It would be great if we could get some more help from everyone out there in testing out these new builds.