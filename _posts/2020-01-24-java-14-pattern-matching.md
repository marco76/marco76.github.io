---
title: 'Java Records'
description: 'Java 14 new feature'
date: 2020-01-23T01:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'java'
color: '#7AAB13'
permalink: /java-records
categories:
  - Java
tags:
  - Java

introduction: 'How to use preview features in Java'
---

Java 14 introduces a new interesting feature: 'Records'.

_The feature has still a preview status and it could change in future releases._

Here you can find the official [Enhancement request](https://openjdk.java.net/jeps/359).

The goal is to have a 'data carrier' class without the traditional 'Java (unnecessary) ceremony'.

In other words a 'Record' represents an immutable state. Record like Enum is a restricted form of class.


### Benefits
- it generates _equals()_, _hashCode()_, toString(), read accessors ('getters') for you

### Restrictions
- extend a Record, it's a final class
- extend a Class
- change a field value (no get)

### Support
- implement an interface

### Tricks
- mutable field: the fields should be immutable but if a field contains a List or an Array you can still change the content of the object.

## Examples

### basic
`record Person(){};`

This is a valid record. The compiler accepts it and you can use it in your code:
``` java
var marco = new Person();
var jonatan = new Person();
marco.hashCode(); => 0
```

`marco.equals(jonatan) => true`, `marco == jonatan => false`, `marco.toString() => "Person[]"`;

### adding a field
`record Person(String name){};`

In this example we add an argument to the new record.
Java add the private field and the accessor to the class and implements the 'toString' ,'equals' methods.

``` java
var marco = new Person("marco");
marco.name() // => "marco", no 'get' here!
marco.toString() // => "Person[name=marco]"
marco.hashCode() // => 103666250

marco.equals(new Person("jonatan")); => false

// if we try to modify the state
marco.name = "jonatan"; // => Error: name has private access in Person
marco.setName("jonatan"); // => Error: cannot find symbol
```

Noteworthy here:
- the fields are private and final
- an accessor is created for the fields without the traditional bean notation 'get'.

### implement an interface

`records` can implement an interface, here an example:
```java

interface Person {
  String getFullName();
}

record Developer(String firstName, String lastName) implements Person {
  public String getFullName() {
    return firstName + " " + lastName;
  }
}

var marco = new Developer("Marco", "Molteni"); // => marco ==> Developer[firstName=Marco, lastName=Molteni]
marco.getFullName(); // =>  "Marco Molteni"
```