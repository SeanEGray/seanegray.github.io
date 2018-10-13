---
layout: post
title:  "hacktoberfest: part three(b)"
date:   2018-10-14 00:05:00 +0100
categories: [bigyak, opensource, github, powershell, pester, hacktoberfest]
---
Fixed the bug.

The "Ubuntu1804" image I was using on AppVeyor was running PowerShell Core 6.1.0-preview. My problems were caused by bugs in the preview version. Switching the image over to "Ubuntu" changes to PowerShell Core 6.0.2 which does not have this bug.

I can sleep now.