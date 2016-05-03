---
layout: page
title: Best Wishes or Thanks for Email Signatures?
date: '2012-04-17T13:44:00.001+01:00'
author: Terry Lansdown
tags:
- '345'
- Funcationality
modified_time: '2012-04-17T13:44:48.313+01:00'
blogger_id: tag:blogger.com,1999:blog-8878096609151731808.post-7003414297269983040
blogger_orig_url: http://ui.lansdown.me/2012/04/best-wishes-or-thanks-for-email.html
---

I routinely sign off my emails with...<br /><br />"Best wishes,<br /><br /> Terry"<br /> <br />But often I'm saying thanks rather than wishing recipients all the very best. So, what do I do? I use an AppleScript wrapped in a TextExpander snippet. Here’s the text from the AppleScript, if it helps anyone. Easy and simple, but it deletes the previous sign-off as well as replacing it with the text I want. <br /><br /><blockquote><pre>tell application "System Events"<br />key code 123 -- left arrow<br />key code 123 -- left arrow			<br />key down shift<br />key code 125 -- down arrow<br />key code 125 -- down arrow<br />key code 125 -- down arrow<br />key code 125 -- down arrow<br />key up shift<br />key code 117 -- Delete<br />Keystroke return<br />Keystroke return<br />Keystroke "Thanks,"<br />Keystroke return<br />Keystroke return<br />Keystroke "Terry"<br />Keystroke return<br />end tell</pre></blockquote>