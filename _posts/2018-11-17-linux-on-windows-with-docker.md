---
layout: post
title:  "linux on windows with docker"
date:   2018-11-17 16:25:00 +0000
categories: [bigyak, docker, linux, windows]
---
My work laptop runs Windows 10, but every now and then I have a need for some tools that I'm more comfortable using on Linux such as OpenSSL, Dig, Whois, etc (I know that these run on Windows these days, but habit is a powerful beast). I used to have Virtualbox installed with Linux VMs that I could fire up when I wanted to use these things, but recently I tried to start one and it didn't work. I remembered that I'd recently installed Docker on the laptop, which had forced me to install Hyper-V, which broke Virtualbox (Virtualbox and Hyper-V are both great, but don't play well - it's one or the other).

I was stuck. I didn't fancy going to the hassle of setting up a Hyper-V VM, and uninstalling it didn't guarantee that my Virtualbox VMs would work. Then I remembered that I had Docker installed!

Fire up a command prompt and:
```Batchfile
    docker run -it ubuntu bash
```
BOOM! A few seconds later I'm at a bash prompt where I can easily install and run whatever tools I want.

As an added bonus, I built a quick toolkit container with the tools I wanted preinstalled:
```Dockerfile
FROM ubuntu

MAINTAINER bigyak

RUN apt-get update && apt-get -y install netbase dnsutils openssl whois
```

PS: Yes, I know that WSL is a thing, but that would also have required more setup than the one line Docker command and I didn't want to wait around.