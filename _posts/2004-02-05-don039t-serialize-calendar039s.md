---
title: 'Don&#039;t serialize Calendar&#039;s'
author: 'Max Rydahl Andersen'

tags: [ Java ]
orignallink: 'http://blog.xam.dk/?p=26'
---
<div><p>...or at least not a lot of them.<br><br>
Today we just discovered that ONE session in our fat/slim client serialized about 1 MB of data over the wire!<br>
1 meg is alot! .... more investigation showed that the amount of objects looked right, but we could not understand why they were so big!<br><br>
The reason ?<br><br>
We had a big bunch of objects with Calendars in them....and try to look at a GregorianCalendar - it got ALOT of fields which are all serialized........making our own serializiation of the Calendar objects and voila...the size was now 300k which still is big, but we will look for more of this.<br><br>
Really nasty to have a core class serialize so much unnecessary stuff...!</p></div>
