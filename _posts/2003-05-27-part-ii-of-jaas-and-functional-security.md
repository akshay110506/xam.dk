---
title: 'Part II of JAAS and functional security'
author: 'Max Rydahl Andersen'

tags: [ Java ]
orignallink: 'http://blog.xam.dk/?p=13'
---
<div><p>Hmm.....since the last time I posted, I've got one comment and I've searched even more around the 'net about JAAS and authorization (NOT authentication) and I've got a bit wiser ;)<br><br>
The best thing first: JAAS <b>authentication</b> is well documented.<br><br>
The second best thing: JAAS <b>basic</b> authorization is documented, but the documentation and implementation is not very usefull in context of an enterprise level role and policy based security mechanism. Basic problem: the default policy is file-based (hint: non-manageble when we are talking 1000 users) and concentrates about Classes, ProtectionDomains and CodeSource. <br><br>
The third best thing: I can find small pieces of docs around the net indicating that it <b>should</b> be possible to use JAAS (instead of yet-another-security-framework) to implement a full scale enterprise security - but that I already knew, just can't seem to find any reallife examples about it - just small-time tutorials and references to big-bucks systems that noone seem to use (or at least, they don't write about it anywhere - except on the big-bucks company's website ;)<br><br>
So, where are those systems and docs that describes how they use JAAS to implement enterprise level security ?<br>
(And no, big-bucks alternative solutions is not a viable solution! It should be 100% JAAS based, I don't want to "import com.ibm.security.SpecialPolicyOrPermissionSystem" in my code - the only thing I want is "import javax.security.*" :)<br>
If that's possible it is ok if it's a big-bucks implementation - I just want to see an example of JAAS applied instead of simple toy examples ;)<br><br>
Note: The configuration and Policy implementation may of course use the big-bucks implementation, but they should conform to the JAAS API - nothing more should be needed. So, do you got any ? <br><br>
whoops - almost forgot to list my "Good JAAS links"-list:<br><br><a href="http://www-106.ibm.com/developerworks/java/library/j-jaas/" title="Extend JAAS for class instance-level authorization">IBM article</a> that is the only article I could find showing how to properly extend JAAS to perform functional and instance based authorization (e.g. "Any registered (authenticated) user can create an auction but only the user who created the auction may modify it." kind of permissions)<br><br><a href="http://www.javageeks.com/Papers/JavaPolicy/JavaPolicy.pdf">JavaGeeks article</a>, examples and critique on how the default implementation of Policy works.<br><br><a href="http://www.research.ibm.com/people/k/koved/Presentations/OReilly2000/indexc.htm">Another IBM slideshow</a>, that Matt gave me in his comment. Among other good stuff it describes more precisly (almost mathematically) how the permissions works etc. A good read.</p></div>
