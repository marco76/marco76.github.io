---
id: 649
title: Java Threads meaning
date: 2016-04-21T18:29:00+00:00
author: Marco Molteni
layout: post
guid: http://marco.dev/?p=649
permalink: /2016/04/21/what-java-threads/
categories:
  - java
tags:
  - java
  - Thread
---
You started your ‘Hello World’ Java application and Visual VM … what are all those threads???

<img src="https://i1.wp.com/marco.dev/wp-content/uploads/2016/04/1461248796_thumb.png" align="middle" class="aligncenter" data-recalc-dims="1" />

  * main: the application you started with the main method
  * Signal Dispatcher: it handles the signals sent from the OS to the JVM
  * Attach Listener: it allows external process to start a thread in the JVM and send back information to the external process
  * Finalizer: pull object from the finalization queue and call the finalization method (it is created by Finaliser.java)
  * Reference Handler: it manages the reference queue (v. Reference.java)
  * RMI TCP Accept-0, RMI TCP Connection …,&nbsp;RMI Scheduler, RMI Reaper: it handles the access to <a href="https://docs.oracle.com/javase/8/docs/technotes/guides/rmi/hello/hello-world.html" target="_blank">remote objects</a>
  * JMX server connection: it allows the connections of JMX modules. It is created by com.sun.jmx.remote.internal.ServerCommunicationAdmin
  * DestroyJavaVM: the application is shutting down, wait for all non-daemon threads to end, then destroy the VM. It’s called in the java.c file
  * GC Daemon:<a href="http://www.docjar.com/html/api/sun/misc/GC.java.html" target="_blank">Garbage collection thread</a>