---
title: 'Swing+Foxtrot, wow! ;)'
author: 'Max Rydahl Andersen'

tags: [ Java ]
orignallink: 'http://blog.xam.dk/?p=11'
---
<div><p>I have always loved the Swing framework for it's allmighty model/view separation (as opposed to the current state of SWT), but I have also hated Swing for it's "one-eventque-to-rule-them-all" approach which makes the GUI look rather sluggish when doing things that takes time.<br><br>
At work we were forced to find a solution as our app would do "things that takes time" quite often and we needed the gui to look good and at the same time there should not be to much burden on the developers doing it to avoid having the sluggish gui.<br><br>
One alternative is the well-known SwingWorker approach which actually works ok - if you really WANT to start another thread for your work - Often this is NOT the case. You would like the code to be as simple as possible and that makes it hard to advocate the usages of threads, semaphores, monitors etc. each time one wants to fetch data. In other words, the solution should be as non-intrusive as possible - preferrably one should just code the gui and biz-logic and when it shows up that something takes too long and the gui is getting sluggish it should be as easy as possible to make the change.....<br><br>
So, when it came to show I remembered the <a href="http://foxtrot.sf.net" title="Foxtrot project">Foxtrot</a> project which i downloaded at once, and look and behold - it fullfilled all of my wishes ;)<br><br>
Integrating Foxtrot into our project was merely just an "add foxtrot.jar to classpath + write 10 lines of code" and now I can do the following transformation of my gui:<br><br>
This code is found in some action/action listener:<br><code><br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;try {<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; ... preprocessing ...<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;Something s = bizLogic.fetchSomething(x,y,z); // length operation which makes gui sluggish<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;somefield.setText(s.getX());<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;sometable.setModel(makeModel(s.getData());<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;} catch ...<br></code><br><br>
With Foxtrot I get:<br><code><br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;try {<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; ... preprocessing ...<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; Something s = (Something) getRunableContext().run(new Task() { <br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; &#160;&#160;&#160;Object run() throws SomeException() {<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;return bizLogic.fetchSomething(x,y,z); // same code, but now the gui looks good ;)<br>
  &#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;}<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;}); <br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;somefield.setText(s.getX());<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;sometable.setModel(makeModel(s.getData());<br>
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;} catch ...<br></code><br><br>
And now there is no complex need for handling multiple threads in all these simple cases...You gotta love it ;)<br><br>
Note: the getRunnableContext() thingy is simple just a wrapper around Foxtrot that we made to be able to start/stop e.g. a progressbar transparently to the code. It is highly inspired from the Eclipse jface framework which actually has their own "foxtrot"-implementation to be able to make SWT non-sluggish.</p></div>
