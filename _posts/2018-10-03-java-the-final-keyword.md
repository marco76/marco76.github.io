---
title: 'The final keyword in Java'
description: ''
date: 2018-10-03T10:41:48+00:00
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

<img src="{{site.baseurl}}/assets/img/uploads/2018/java-final.png" />

The red reference cannot be changed (final). The values of the collection can change.
The *final* keyword refers to the reference and not to the object.

**final and performance gain ?**

The use of *final* is intended mainly has good practice during the development phase to avoid that variables are modified accidentally.

In theory the JVM implementation could optimise the compiled code improving the performance of final variables during the runtime, e.g. avoid the synchronisation.

The performance gains are only assumptions and the performances could change with every new release of the JVM, for this reason the *final* variable should be used only with the goal of improving the quality of the code.

**final keyword and effectively final variables and parameters**

For some local variables you don't need to specify the keyword **final**, the JVM assumes this variable is (effectively) final if:

* it is not declared *final*
* it is not assigned (beside the declaration)
* it is not incremented (++) or decremented (--) using pre- and postfix operators

The same rules are applied to the **parameters** of methods, constructors, lambda and exceptions. The JVM treats these parameters as local variables to determine if they are effectively final.

**when final and effectively final variables are mandatory to avoid exceptions?**
inner classes: outer classes variables used in inner classes
lambda: variables of the enclosing scope used in the lambda body

**Java Exceptions**

`Local variable defined in an enclosing scope must be final or effectively final`

**Links**

Language Specifications: [https://docs.oracle.com/javase/specs/jls/se8/html/jls-4.html#jls-4.12.4]()