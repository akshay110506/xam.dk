---
title: 'Import/Export of Eclipse 3.2.x Working sets Hack!'
author: 'Max Rydahl Andersen'

tags: [ Java ]
orignallink: 'http://blog.xam.dk/?p=56'
---
<div>
<p>For some reason Eclipse Working Set's definitions are not exportable from Eclipse (see Eclipse Bug <a href="https://bugs.eclipse.org/bugs/show_bug.cgi?id=156041">#156041</a> and <a href="https://bugs.eclipse.org/bugs/show_bug.cgi?id=138854">#138854</a>.
<br><br>
We should fix that, but until then the following hack can be used to ease the pain of migration between two setups:
<br><br></p>
<ol>
<li>Setup your new Eclipse and import all the projects you want to have in it</li>
 <li>Stop Eclipse</li>
<code>cd [old-workspace]/.metadata
  cp workingsets.xml [new-workspace]/.metadata/.plugins/org.eclipse.ui.workbench</code>

<li>Start Eclipse with a -clean argument</li>
</ol>
<br><br>
After this all the shared projects between old and the new eclipse that you had grouped into working sets will now also be grouped in the new eclipse.
<br><br>
That saved a couple of hours/days of my time, so here it is for you ;)</div>
