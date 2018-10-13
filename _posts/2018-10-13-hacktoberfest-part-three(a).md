---
layout: post
title:  "hacktoberfest: part three(a)"
date:   2018-10-13 20:35:00 +0100
categories: [bigyak, opensource, github, powershell, pester, hacktoberfest]
---
Hacktoberfest PR three is interesting but a little bit infuriating. I saw [this issue](https://github.com/Stephanevg/PSHTML/issues/54) in PSHTML - a PowerShell module for converting a PowerShell-like DSL to HTML. Given that I use both Windows and Linux, and write a lot of PowerShell, the idea of getting a module to work on Linux and Windows appealed to me (I couldn't offer any help with the MacOS part of the issue).

The module already had a good suite of tests (written in Pester) for Windows and run through AppVeyor, so a good place to start seemed to be to get AppVeyor to run these tests on Linux as well as Windows, then see what breaks. Fortunately, AppVeyor makes this really easy, all I had to do was add:

    image:
    - Visual Studio 2015
    - Ubuntu1804

to the top of the appveyor.yml file.

The first run very quickly highlighted what was basically the only thing stopping this module from running under Linux - Linux filesystems are case-sensitive and the module was written for a Windows filesystem, which is not. I was a little concerned that this meant that I was going to be digging for hours to tidy up all of the case-insensitive parts, but fortunately there were only a few places where changes were needed.

The next issue to solve was that Install-PackageProvider, which was called in the AppVeyor install script, didn't work as expected on Linux because the NuGet package didn't seem to be available. I'm not sure if that's specifically a Linux issue, or a PowerShell Core issue, but either way it doesn't matter, because that line wasn't actually needed for the tests at all, so I just removed it.

Following on from that, the Install-Module commands were failing because we weren't running as root, so I just stuck:

    -Scope CurrentUser

on the end, which works fine on Windows, too.

At this point we were basically working, except for two issues: 

1) Some of the tests call Get-Service to generate some test data, but that cmdlet doesn't exist on Linux. I've left a note for the module maintainer to decide how they want to handle this.

2) A lot of the tests fail with a very peculiar error that looks like this:

    [-] Error occurred in Describe block 173ms
      ValidationMetadataException: The attribute cannot be added because variable Result with value  would no longer be valid.
      MethodInvocationException: Exception calling "AddTestResult" with "8" argument(s): "The attribute cannot be added because variable Result with value  would no longer be valid."

This one's a bit infurating because It Works On My Machine!(tm) AppVeyor must be doing something I'm not aware of, or using a different version of something important. Oddly, it always happens on the second test of a test file, and I can't see any significant differences between test files that generate the errors and those that don't. I'm going to have to think about this and come back to it, but I've submitted a PR as is to see if anyone else has any ideas.