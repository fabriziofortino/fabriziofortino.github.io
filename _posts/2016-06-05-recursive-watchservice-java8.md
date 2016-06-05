---
layout: post
title: "Java 8 Recursive WatchService"
description: "Monitor a folder tree using the power of Java 8 Lambdas"
category: articles
tags: [java, functional, watch, service, monitor]
comments: true
share: true
ads: true
image:
  feature: watchservice.jpg
---

<a href="https://docs.oracle.com/javase/8/docs/api/java/nio/file/WatchService.html" target="_blank">WatchService</a> is a
class included in the standard Java nio package since version 7. WatchService is extremely useful when we need to trigger an action after an event happens to an object in a specific folder or set of folders.

In the following code I have implemented a watch service registered to a root folder and all its subfolders.

Most of the examples around are based on Java 7. Since I am using Java 8 I have decided to re-implement it
using streams and lambdas:

* The code responsible to recursively register the folders has been implemented as a
<a href="https://docs.oracle.com/javase/8/docs/api/java/util/function/Consumer.html" target="_blank">Consumer</a>
Functional Interface (lines 67-84) and gets called to register the root folder (line 86) and the folders created at
successive stages (line 109).
* A lambda with no arguments (or thunk) is used at line 88 to create and start a separate thread to execute
the WatchService logic.
* The iteration of the pending events has been implemented in a functional style using Java 8 streams (lines 103-114).

{% gist fabriziofortino/83eb36c7b48e9b900c1da1d8508245cd RecursiveWatcherService.java %}

The code makes use of Spring but it's extremely easy to remove it and adapt to other needs.
