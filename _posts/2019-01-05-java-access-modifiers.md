---
title: 'Difference between protected and default/package access in Java'
description: ''
date: 2019-01-05T01:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'java'
color: '#7AAB13'
permalink: /java-access-modifiers
categories:
  - Java
tags:
  - Java
 
image: '/assets/img/'

introduction: 'Visual memo of the access modifiers and controls'
---

## Protected vs default access control

Differences between the protected and the default/package access.

Quick overview, this is a typical interview question.

[<img src="{{site.baseurl}}/assets/img/uploads/2019/protected_exp_svg.svg" alt=""/>]({{site.baseurl}}/assets/img/uploads/2019/protected_exp_svg.svg)

In short:

- the protected access has a modifier (keyword): _protected_
- the default access doesn't have any modifier.
- the default access doesn't allow access outside of the package 

## The complete details (Java Language Specification 11)

[JLS: Access Control](https://docs.oracle.com/javase/specs/jls/se11/html/jls-6.html#jls-6.6)

[JLS:_protected_](https://docs.oracle.com/javase/specs/jls/se11/html/jls-6.html#jls-6.6.2)
