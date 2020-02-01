---
title: 'Java 14 new features'
description: 'What''s new in Java 14'
date: 2019-11-19T01:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'java'
color: '#7AAB13'
permalink: /java-14-new-features
categories:
  - Java
tags:
  - Java

introduction: 'What''s new in Java 14'
---
_this post is still work in progress it will be updated regularly and often in the future_

- Java 14 Enhancements official page: [https://openjdk.java.net/projects/jdk/14/](https://openjdk.java.net/projects/jdk/14/)

## Developer / language new features
These features are interesting for your daily basis activity.

### Pattern matching for instanceof (Preview)
[https://openjdk.java.net/jeps/305](https://openjdk.java.net/jeps/305)

[Here you can find an extended explanation](https://cr.openjdk.java.net/~briangoetz/amber/pattern-match.html). The pattern matching should be implemented initially for
`instanceof`, `switch` could follow in future releases.

Java 11 example:
``` java
if (obj instanceof String) {
 String s = (String) obj;
 System.out.println(s);
}
```

In this code we can see a 'pattern':

-> test: `obj instanceof String`

-> conversion: `(String) obj;`

-> assignment: `String s = (String) obj;`

-> usage: `System.out.println(s);`

Another issue is the repetition of `String`, it's used 3 times in this small example.

The pattern 'test -> conversion -> assignement -> usage' is a common pattern used by the developer.

With Java 14 we can simplify our code:
``` java
if (obj instanceof String obj) {
 System.out.println(s);
}
```

If `obj` is an instance of `String` this is assigned to the `String` object `obj`.

You can find this feature already implemented in [C#](https://docs.microsoft.com/en-us/dotnet/csharp/pattern-matching), [Scala](https://docs.scala-lang.org/tour/pattern-matching.html), [Haskell](http://learnyouahaskell.com/syntax-in-functions)




#### Records (Preview)
[Records](https://openjdk.java.net/jeps/359)

You can find a post with many details here: [https://marco.dev/java-records](https://marco.dev/java-records)

#### Helpful NullPointerException
[https://openjdk.java.net/jeps/358](https://openjdk.java.net/jeps/358)

#### Switch Expression
[https://openjdk.java.net/jeps/361](https://openjdk.java.net/jeps/361)

#### Text Blocks (Second preview)
[https://openjdk.java.net/jeps/368](https://openjdk.java.net/jeps/368)

## Garbage collector

If you want to know more about Garbage colletors in the JDK you can see my presentation here: [http://marco.dev/garbage-collectors-intro](http://marco.dev/garbage-collectors-intro)

#### Z is coming to MacOS

[https://openjdk.java.net/jeps/364](https://openjdk.java.net/jeps/364). Z is already present as 'preview' feature for Linux 64-bit, now it is ported to MacOS and in future to Windows.

Z is the new powerful Garbage Collector developed by Oracle. This GC target Heaps until 1TB with pauses (stop-the-world) of max 10ms. Here you can find the details:
[ZGC Wiki](https://wiki.openjdk.java.net/display/zgc/Main).


#### CMS (Concurrent Mark Sweep) removed

CMS is now removed from the sources, it was alredy deprecatd in Java 9.
CMS has been replaced G1, if you didn't migrate yet it's really time to do it.

G1, Parallel and Shenandoah are currently your best alternatives.

#### Deprecate the ParallelScavenge + SerialOld GC Combination

[https://openjdk.java.net/jeps/366](https://openjdk.java.net/jeps/366)

There is a lot going on in the GCs for Java. This combination has been replaced by Parallel GC and is not used frequently, for this reason will be deprecated.

#### Other improvements
- G1 performances: [NUMA-Aware Memory Allocation for G1](https://openjdk.java.net/jeps/345)

## Tools
#### Packaging (Incubator)
[https://openjdk.java.net/jeps/343](https://openjdk.java.net/jeps/343)

#### JDK Flight Recorder: JFR Event Streaming
[https://openjdk.java.net/jeps/349](https://openjdk.java.net/jeps/349)

## JDK
#### Non-Volatile Mapped Byte Buffers
[https://openjdk.java.net/jeps/352](https://openjdk.java.net/jeps/352)