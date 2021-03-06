---
title: 'Calling conference bridge numbers with Skype'
author: 'Max Rydahl Andersen'

tags: [ applescript, skype, Work at JBoss ]
orignallink: 'http://blog.xam.dk/?p=187'
---
<div>
<p>I like Skype, but one of the features it has always lacked is a way to directly call into conference call bridges.
<br><br>
With a little help from AppleScript I finally managed to make that happen:
<br><br></p>
<pre lang="applescript" escaped="true">on run {input, parameters}
<br><br>
 set phone_number to "+1......"   
 set dtmf to "1234567894#,*1234#" 
<br><br>
 tell application "Skype"
 set active_call to send command "CALL " &amp; phone_number script name ""
 set skype_call_id to word 2 of active_call
 delay 10
 set bridge to "ALTER CALL " &amp; skype_call_id &amp; " DTMF "
 repeat with tone in the characters of dtmf
 if tone contains "," then
 delay 2
 else
 send command bridge &amp; " " &amp; tone script name "s2"
 delay 0.2
 end if
 end repeat
 end tell
end run</pre>
<br><br>
Copy and edit the above, remember to set the phone_number and dtmf variables.
<br><br>
Note: the dtmf variable treat's "," as a 2 second pause to use for conf call bridges that are a bit slow :)
<br><br>
Save it as a Service and you can now directly call your conference line directly from any app via the Service menu.
</div>
