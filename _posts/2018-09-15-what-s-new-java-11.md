---
title: 'What''s new in Java 11'
description: ''
date: 2018-09-15T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'java'
color: '#7AAB13'
permalink: /java-11-newsworthy/
categories:
  - Java
tags:
  - java
 
image: '/assets/img/'

introduction: 'Here some of the new features of Java 11 with a direct impact on the daily development activity.'
---

# What's new in Java 11 (Open JDK)

## Some of the new features in Java 11
In this page we will list some of the new features of Java 11 (from 9 to 11) relevant for the daily activity of business application developers.

## Local-variable type inference
With Java 11 is possible to declare local variable with type inference **with initializer** (keyword *var*).

```java
// words is of type String[]
var words = new String[] {"abc", "def"};

Assertions.assertTrue(words instanceof String[]);

// the initializer is mandatory
// dataList is of type ArrayList
var dataList = new ArrayList();

dataList.addAll(Arrays.asList(words));
Assertions.assertEquals("abc", dataList.get(0));
Assertions.assertEquals("def", dataList.get(1));
```

##Features in the String class
*lines*
A new method *lines()* creates a *Stream* with the lines of the *String*.

```java
@Test
public void stringCountTest() {

  String text = "line 1\nline 2\nline3";

  // the new lines method return a Stream
  assertTrue(text.lines() instanceof Stream);

  // each element in the stream represents a line of the input string
  assertEquals(3, text.lines().count());
}
```

*strip - stripLeading - stripTrailing*
*strip* eliminates the whitespaces at the beginning and at the end of a String.

```java
@Test
public void stripTest() {
  
  String example = "      leading and trailing spaces      ";

  assertEquals("leading and trailing spaces", example.strip());
  assertEquals("leading and trailing spaces      ", example.stripLeading());
  assertEquals("      leading and trailing spaces", example.stripTrailing());
}
```

*isBlank*
isBlank() returns *true* if the String is empty or it contains only whitespaces.

```java
@Test
public void isBlankTest() {

  assertTrue("     ".isBlank());
  assertTrue("".isBlank());

}
```

*repeat*
*repeat* composes a string repeating its content for a defined number of times.

```java
@Test
public void repeatTest() {
  
  assertEquals("abc abc abc ", "abc ".repeat(3));
  assertEquals("", "abc ".repeat(0));

}
```
## Optional
Optional get a new method to check if the value is not present: *isEmpty()*

```java
@Test
public void isEmptyTest() {
  Optional<Integer> evaluate = Optional.of(5);
  Assertions.assertFalse(evaluate.isEmpty());
}
```