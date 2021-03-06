---
title: 'JAAS and functional security ?'
author: 'Max Rydahl Andersen'

tags: [ Java ]
orignallink: 'http://blog.xam.dk/?p=12'
---
<div><p>I've always liked <a href="http://java.sun.com/products/jaas/index-14.html" title="JAAS">JAAS</a> as it provides for a pluggable login mechanism. The exact same code is used for login to LDAP, NT, Kerboros, passwd files, Propertiary system A, B, C etc. That's beautifull ;)<br><br>
The only thing i'm really missing, is to find good usages of the SecurityManager, Subject, Prinicipal and Credentials classes/concepts. I can find loads of articles talking about the pluggable architechure, the ClassLoader protection and encryption support of JAAS - but no real articles about how one would go about doing something like:<br><br><code><br>
&#160;&#160;&#160;public List findDrugs(x,y,z) {<br>
&#160;&#160;&#160;&#160;&#160;&#160;checkPermission("MayFindDrugs");<br>
&#160;&#160;&#160;&#160;&#160;&#160;List drugs = performSearch(x,y,z);<br>
&#160;&#160;&#160;&#160;&#160;&#160;foreach drug in Drugs {<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;if(drug.isNarcotic() &amp;&amp; checkPermission("MayPrescribeNarcotics")) {<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;result.add(drug);<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;} else {<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;result.add(drug);<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;}<br>
&#160;&#160;&#160;&#160;&#160;&#160;}<br>
&#160;&#160;&#160;}<br></code><br><br>
In other words - how would one setup JAAS and all its "cousins" classes, policy-file etc. to provide<br>
FUNCTIONAL security ?<br><br>
Also the current file based policy system seems to restrictive, simple and unmanageble, isn't there <br>
any good example of a good, clean and flexible policy "provider" ? <br><br>
And is JAAS the right tool for the job ? (I hope so as it seem to have all the needed parts...)<br><br>
Maybe I'm just not seeing the forrest for all the trees - but I just can't seem to find it....maybe I don't got<br>
the right Credentials yet ? ;-)</p></div>
