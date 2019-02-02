---
title: 'The final keyword in Java'
description: ''
date: 2018-10-03T10:41:48+00:00
updated: 2019-02-03T00:00:00+00:00
author: Marco Molteni
layout: post
main-class: 'java'
color: '#7AAB13'
permalink: /java-the-final-keyword/
categories:
  - Java
tags:
  - java
 
image: '/assets/img/'

introduction: 'The final keyword doesn''t guarantee the immutabily of your objects. It''s really performant?'
---
# final doesn't guarantee that your object state is immutable
A *final* variable can be assigned only once but the state the object referenced by the variable can change.

This could be particular dangerous with collections if the developer assume that the values cannot be changed.

The *final* variable will reference always the same Collection Object but it will be possible to add and remove values.

<img src="{{site.baseurl}}/assets/img/uploads/2018/java-final.png" width="500px"/>

The red reference cannot be changed (final). The values of the collection can change.
The *final* keyword refers to the reference and not to the object.

**final and performance gain ?**

The use of *final* is intended mainly has good practice during the development phase to avoid that variables are modified accidentally.

In theory the JVM implementation could optimise the compiled code improving the performance of final variables during the runtime, e.g. avoid the synchronisation.

The performance gains are only assumptions and the performances could change with every new release of the JVM, for this reason the *final* variable should be used only with the goal of improving the quality of the code.

> the performance benefit gained by declaring a method or class as final is usually zero
>
> _Brian Goetz, 2002_

**final keyword and effectively final variables and parameters**

For some local variables you don't need to specify the keyword **final**, the JVM assumes this variable is (effectively) final if:

* it is not declared *final*
* it is not assigned (beside the declaration)
* it is not incremented (++) or decremented (--) using pre- and postfix operators

The same rules are applied to the **parameters** of methods, constructors, lambda and exceptions. The JVM treats these parameters as local variables to determine if they are effectively final.

**when final and effectively final variables are mandatory to avoid exceptions?**
inner classes: outer classes variables used in inner classes
lambda: variables of the enclosing scope used in the lambda body

**Optimization**

`final` methods are candidates for an inline optimization done by the compiler.
But only `private` methods can be safely inlined, for this reason the `final` keyword is redundant. 

`final` fields can be optimized by the compiler, e.g. caching the value in a register.

**When to use the `final` keyword**

> In my experience, final is vastly overused for classes and methods (generally because developers mistakenly believe it will enhance performance), and underused where it will do the most good -- in declaring class instance variables.
>
>_Brian Goetz, 2002_

> with final classes and methods you should always ask yourself if you really need to use final. With final fields, you should ask yourself the opposite question -- does this field really need to be mutable? You might be surprised at how often the answer is no.
>
> _Brian Goetz, 2002_

The recommendation is to use the final for fields that won't change value. The use for classes and methods should be exceptional and documented.

The `final` keyword guarantees that a field is initialized before to be accessed by a different thread.

> Make all fields final.
>
> _Joshua Bloch, Effective Java_

The [Oracle Java Tutorial](https://docs.oracle.com/javase/tutorial/java/IandI/final.html) suggests to declare `final` the methods called by a constructor, to avoid unexpected results in subclasses.

**Java Exceptions**

`Local variable defined in an enclosing scope must be final or effectively final`

**Links**

- Language Specifications: [https://docs.oracle.com/javase/specs/jls/se11/html/jls-17.html#jls-17.5]()
- Discussion about the corner case of the specification [https://stackoverflow.com/questions/5066866/testing-initialization-safety-of-final-fields/15517168#15517168]()
- [Brian Goetz on the final keyword, 2002](https://www.ibm.com/developerworks/java/library/j-jtp1029/index.html)
