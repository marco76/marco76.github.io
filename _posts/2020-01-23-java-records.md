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

introduction: 'Records in Java 14'
---

Java 14 introduces a new interesting feature: _records_.

Here you can find the official proposal: [JEP 359 Records](https://openjdk.java.net/jeps/359).

The goal is to have a 'data carrier' class without the traditional 'Java ceremony' (boilerplate).

In other words, a _record_ represents an immutable state. Record like Enum is a restricted form of class.

_The feature has still a preview status and it could change in future releases._

### Use Cases
A typical use case is to return multiple values from a method and replace some data structures and external frameworks used to fill this missing feature, e.g. _tuples_, _Pair_, _Map.Entry_.
DTO are another candidate to profit from _records_.
_Records_ are not intended to replace (mutable) data objects or third libraries like _Lombok_.

### Benefits
- _equals()_, _hashCode()_, _toString()_, _constructor()_ and read accessors are generated for you
- interfaces can be implemented

### Restrictions
- a _record_ cannot be extended, it’s a final class
- a _record_ cannot extend a class
- the value (reference) of a field is final and cannot be changed

### Record definition
<img src="/assets/img/uploads/2020/record_definition.png" alt="" />

The body is optional. The state description declares the _components_ of the _record_.

This simple line of code is translated by the compiler in a class similar to this one:
```java
public final class Person extends Record {
  public final String name;
  public final Integer yearOfBirth;
 
  public Person(String name, Integer yearOfBirth) {
    this.name = name;
    this.yearOfBirth = yearOfBirth;
  }
  
  public String name() {
      return name;
  };
  
  public Integer yearOfBirth() {
    return yearOfBirth;
  };
 
  public final int hashCode() { /* implementation according to spec */}
  public final boolean equals(Object o) { /* implementation according to spec */}
}
```

### Beware
If the fields contain objects only the reference is immutable.
 The referenced objects can change their value compromising the state of the record.
 For this reason, you should use immutable objects in your _record_ to avoid surprises.

## Examples

### How to use them
To execute the examples you can use `JShell` with the flag `--enable-preview`
 or compiling your source using the flags `javac --enable-preview --release 14 [source].java`
 and executing it using `java --enable-preview [mainclass]`.

If you are using a _single file program_ you need the _source_ flag: `java --enable-preview --source 14 [source].java`  
 
The code in this post has been tested with JShell and IntelliJ (EAP) using OpenJDK build 14-ea+32-1423.

### minimalistic
```java
record Person(){};
```

This is a minimalistic valid _record_ without components.
``` java
var marco = new Person();
var jon = new Person();

marco.hashCode(); // => 0

marco.equals(jon) // => true, the objects have the same field content
marco == jon // => false, the objects have different references

marco.toString() // => "Person[]", toString() is implemented by record. A default result for a standard class would have been something like: "Person2@573fd745"
```

### adding a field
```java
record Person(String name){};
```

In this example we add an argument (component) to the new _record_.

Java adds the private field (`final String name;`)
 and the accessor (`public String name() {return this.name;}`) to the class and implements _toString()_, _equals()_ and the constructor `Person(String name) {this.name = name}`.

<img src="/assets/img/uploads/2020/records_person.png" alt=""/>

``` java
// a new constructor is generated a mandatory parameter 'name' 
var marco = new Person("marco");

// the default no parameter constructor is not generated
var andy = new Person(); // => constructor Person in record Person cannot be applied to given types; required: java.lang.String

// to read the value of the name we access the field 
marco.name() // => "marco", no 'get' here!
marco.toString() // => "Person[name=marco]"
marco.hashCode() // => 103666250

marco.equals(new Person("andy")); // => false

// if we try to modify the state
marco.name = "andy"; // => Error: name has private access in Person
marco.setName("andy"); // => Error: cannot find symbol
```

Noteworthy here:
- the fields are private and final
- an accessor is created for the fields _without_ the traditional bean notation 'get'.

### implementing an interface

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

<img src="/assets/img/uploads/2020/records_interface.png" alt="" />

### implementing multiple constructors

_records_ implements a constructor with the fields declared as parameters.

```java
record Person(String name, Integer age){};
```

This code generates something like:
```java
public final class Person extends Record {
  private final String name;
  private final Integer age;
  
  // constructor generated
  public Person(String name, Integer age) {
    this.name = name;
    this.age = age;
  }
  // accessors
  public String name() {return name;}
  public String age() {return age;}
  
  // ... other methods

}
```

If you try to instantiate the _record_ without the two parameters an exception is thrown:
```java
var marco = new Person();
```

```java
constructor Person in record Person cannot be applied to given types;
|    required: java.lang.String,java.lang.Integer
|    found:    no arguments
|    reason: actual and formal argument lists differ in length
|  var marco = new Person();
```

You can add your own constructor if needed, e.g. not all the parameters are required.

```java
record Person(String name, Integer age){
  public Person() {
    this("unknown", null);
  }
};
```

In this case you can instantiate an object using the extra constructor:
```java
var marco = new Person(); // => marco ==> Person[name=unknown, age=null]
```

### mutating the state

In this example I show how it's possible to mutate the values inside a `record`.
The _types_ used in a record should be immutable to be sure that the _state_ won't change. 

```java
record Developer (String name, List<String> languages){};

List<String> languages = new ArrayList<String>(Arrays.asList("Java"));
languages; // ==> [Java];

var marco = new Developer("marco", languages); // marco ==> Developer[name=marco, languages=[Java]]
marco.languages(); // => [Java]
marco.hashCode(); // => -1079012009

// we add one value to the array referenced in the record
languages.add("JavaScript");

// the value of the record changes
marco.languages(); // => [Java, JavaScript]
marco.hashCode(); () // => 256362082

// the Array list is mutable
marco.languages().remove(1);
marco.languages(); // => [Java]
marco.hashCode(); // => -1079012009
```
### redefining an accessor

When you declare a _record_ you can redefine an accessor method. This is useful if you need to add annotations or modify the standard behaviour.
```java
record Developer(String name){
  // we modify the default accessor
  public String name() {
  // this.name refers to the field generated by record  
  return "Hi " + this.name;
  }
};
    
var bill = new Developer("William");
bill.name(); // => "Hi William"
```

