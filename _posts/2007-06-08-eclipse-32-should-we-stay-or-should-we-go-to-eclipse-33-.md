---
title: 'Eclipse 3.2 should we stay or should we go to Eclipse 3.3 ?'
author: 'Max Rydahl Andersen'

tags: [ JBoss Tools and devstudio ]
orignallink: 'http://blog.xam.dk/?p=59'
---
<div><p>Eclipse 3.3 and WTP 2.0 is planned to be released soon and I'm considering if we should move now or later.
<br><br>
Previous major Eclipse upgrades have been smooth for me as an user, but a *pain* as a plugin developer since many of the API's I've needed to integrate with WTP DOM and JDT changed. Of course that was all my own fault since I am and were using internal API, but hey what can I do when there is no public DOM editor API and previously there where no public java code completion API ?
<br><br>
Luckily, Hibernate Tools seem to be running just fine under Eclipse 3.3/WTP 2.0, but as you all know we got the rest of JBoss IDE and the plugins from Exadel which is now JBoss Tools to also migrate.
<br><br>
From the first look of things it seems like we can migrate 98% of our code base in JBoss Tools without any compilation issues at least, but what is obvious is that when we first do the move we can't be compatible with Eclipse 3.2 hence we will be Eclipse 3.3 only.
<br><br>
So technically it can be done, but what about our users, will you prefer an Eclipse 3.2/WTP 1.5 or Eclipse 3.3/WTP 2.0 based set of JBoss Tools plugins (and as a consequence Red Hat Developer Studio will also be based on Eclipse 3.3/WTP 2.0) ? 
<br><br>
There is also the whole issue about checking if the behavior is the same even though the API did not change...</p></div>
