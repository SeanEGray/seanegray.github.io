---
layout: post
title:  "my first open source contributions"
date:   2018-10-07 00:36:00 +0100
categories: [bigyak, opensource, powershell, desiredstateconfiguration]
---
Inspired by [this blog entry](https://medium.freecodecamp.org/a-beginners-very-bumpy-journey-through-the-world-of-open-source-4d108d540b39) and [this one](https://medium.freecodecamp.org/new-contributors-to-open-source-please-blog-more-920af14cffd) by Shubheksha, this post is about the first time I contributed to open source software.

### SQL Servers
A couple of years ago I was spending a lot of time working with [PowerShell DSC](https://docs.microsoft.com/en-us/powershell/dsc/overview); I was using the xSQLServerSetup resource (now renamed to SQLSetup) to ... setup an SQL Server, but ran into a bug. Unfortunately, I don't actually remember what the symptoms were, but I know that I decided to dig through the DSC resource source code to try to troubleshoot it (it was all PowerShell, after all). I tracked the problem to a particular line in a function - it was trying to return the first item in an array of strings but if the array had only one entry, it went a bit wrong and returned the first character of that entry, instead. It was an easy thing to fix (literally adding two brackets to the line), so I raised an [issue](https://github.com/PowerShell/SqlServerDsc/issues/21) on GitHub to report it, but since I knew how to fix it, it made sense to actually submit the change.

I forked the repo, made the change, and submitted a pull request. It got accepted and merged. It was all a little anticlimatic, but it meant that I had fixed the bug! Now I and anyone else using that resource wouldn't have that problem any more. And it's pretty cool to contribute to something that lots of people use!

I checked while writing this post and unfortunately my line isn't there any more - that particular function seems to have been totally refactored. Oh well - incentive to contribute to something else!

### Certificates
My second contribution was once again to a DSC resource, but this one turned out to be a bit bigger. I was using the xCertReq resource (now renamed to CertReq) to request some certificates that had Subject Alternate Names. I found that the resource could create the certificates as expected if they weren't there, but it seemed to behave strangely if I changed the SAN - it wouldn't notice that the SAN was incorrect, and therefore wouldn't create a new certificate. I raised an [issue](https://github.com/PowerShell/CertificateDsc/issues/61) for it and started to try to work out if I could fix it.

I added some code to the module that resolved the issue. It was ugly but it worked; I submitted a pull request. Shortly afterwards, one of the project maintainers got back to me and was very supportive and grateful for the change, but asked if I could make some small changes (mostly styling) and add a unit test for my code. The changes were simple enough and I'd been playing with [Pester](https://github.com/pester/Pester) a lot, so I was happy for the opportunity to use that.

While I was working on these changes, another PR was merged in which created a merge conflict with mine, so I was asked to resolve it; fortunately that didn't turn out to be too complicated, since it was the first time I'd ever had to do that. I submitted the updates and the PR was accepted! 

The best thing about this one is that my code is still in that module, so anyone using it is using code that I wrote! That's pretty cool.

### Your turn
I'm definitely planning to contribute to more open source software, because I use so much of it and I'd like to give back to the community. I'd encourage you to do so too, but also to write about it - perhaps if more people who are more eloquent than I am blog about their experiences, others may be inspired to join in.