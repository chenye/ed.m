---
layout: post
title: "VirtualBox Only Shows 32-bit Guest Operating Systems Options in 64-bit Windows 10"
modified:
categories: blog
excerpt: "The solution to this problem is simple. Just turn on virtualization technology of the CPU in your BIOS setting."
tags: [virtualbox]
image:
  feature:
date: 2015-09-23T20:26:00-5:00
---

I recently upgraded the Windows 8 on my home laptop to Windows 10. Everything works fine except VirtualBox. I installed VirtualBox 5.0.4, after reseting Windows 10 to its original clean status. However, when I was trying to set up a 64-bit virtual machine, I found no 64-bit guest OS option available in the virtual machine setup wizard. There are many suggestions if you try to look up in the Internet. But this one solves my problem: [http://www.fixedbyvonnie.com/2014/11/virtualbox-showing-32-bit-guest-versions-64-bit-host-os/]
And a comments in that post suggests some steps are not necessary.

So actually there is only one thing you really need to do, that is, turn on the virtualization technology of the CPU in your BIOS setting. For my Intel CPU, that is the VT-x technology. And then problem solved!
