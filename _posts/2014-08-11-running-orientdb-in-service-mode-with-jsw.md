---
layout: post
title: "OrientDB in service mode with Java Service Wrapper"
description: "Run OrientDB as a service or daemon with Java Service Wrapper"
category: articles
tags: [java, nosql, orientdb, jsw]
comments: true
share: true
---

Many companies heavily rely on the functions of database that their daily business operations can not be executed if database are not available, making database management and maintenance a critical component of their business models.

<a href="http://www.orientechnologies.com/orientdb/" target="_blank">OrientDB</a> is an Open Source NOSQL database, written in Java, which nicely mixes Document and Graph features in a unique solution.

<a href="http://wrapper.tanukisoftware.com/doc/english/product-overview.html" target="_blank">Java Service Wrapper</a> is a configurable tool which allows Java-based applications to be installed, controlled and monitored as native Unix/Linux/MacOSX/Windows services in a painless way.

On a past project I have integrated JSW Community Edition for a critical application needed 24x7. Since the result was excellent I have decided to do the same with OrientDB. The complete integration scripts and related documentation on how to install and use it are available on <a href="https://github.com/fabriziofortino/orientdb-servicewrapper" target="_blank">https://github.com/fabriziofortino/orientdb-servicewrapper</a>

### Install orientdb-servicewrapper
{% gist fabriziofortino/bcafd9ef3e953fe24fb4 install %}

### Setup OrientDB home
To setup the integration between OrientDB and Java Service Wrapper, edit the file *$ORIENTDB-HOME/bin/service/orientdb.conf* and replace the placeholder for the variable *set.default.ORIENTDB_HOME* with the OrientDB installation full path.

### Ready to go
Then, at this point the OrientDB service wrapper is ready to go, you can test it by directly invoking the service:

{% gist fabriziofortino/bcafd9ef3e953fe24fb4 execute %}

### Daemonize and monitor OrientDB
As you can see, install or uninstall of the OrientDB daemon / NT service or retrieving useful information like the JVM dump are easy as pie. But JSW Community Edition gives us even more. Basically the wrapper will ping the OrientDB JVM every 5 seconds (you can change the polling time in orientdb.conf) to make sure the process has not frozen up. If the JVM does not respond the process will be automatically restarted. Moreover, it is possible to intercept any string coming from the OrientDB JVM output and execute any number of actions. By default, I have included in orientdb-servicewrapper a RESTART action triggered when an OutOfMemory exception occurs. You can easily add your custom trigger / actions just modifying the configuration file.

