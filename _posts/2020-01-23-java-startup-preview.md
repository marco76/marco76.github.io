---
title: 'Preview the new features in Java'
description: 'How to use the Preview features'
date: 2020-01-23T00:40:48+00:00
author: Marco Molteni
layout: post
main-class: 'java'
color: '#7AAB13'
permalink: /preview-java
categories:
  - Java
tags:
  - Java

introduction: 'How to use preview features in Java'
---

Every version of Java contains some features that are only in 'Preview' status and are not supported for production.

The goal of these features is to allow developers to test and comment potential future functions before they are officialy released.

To enable the new feature you need to use some flags:

### Standard compilation

compilation: 
`javac --enable-preview --release [release number] [your main class name].java`

execution:
`java --enable-preview [your main class]`

Example:
```java
javac --enable-preview --release 14 HelloWorld.java 
java  --enable-preview HelloWorld
```

### JShell

If you are using JShell it's enough to launch the shell with the `--enable-preview flag`;

`> jshell --enable-preview`

### Single-source-code program

`java --enable-preview [your java source code class]`

Example:
`java --enable-preview HelloWorld`

