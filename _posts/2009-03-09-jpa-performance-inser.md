---
id: 38
title: 'JPA: lazy loading for lazy developers, the performance issue'
date: 2009-03-09T17:17:01+00:00
author: Marco Molteni
layout: post
guid: http://molteni.wordpress.com/?p=38
permalink: /2009/03/09/jpa-performance-inser/
categories:
  - java
---
I found and [interesting article](http://terrazadearavaca.blogspot.com/2008/12/jpa-implementations-comparison.html) about loading and reading data with JPA. There are performance issues when you want to insert 1&#8217;000 or 10&#8217;000 records in the database.
  
Before ORM life was complicated &#8230; 25 lines of code to insert a line in the database, not it&#8217;s easier: entityManager.persist(object)!
  
The frameworks (Hibernate, Toplink &#8230;) do all the work and they do very well the work. The problem with JPA is that we cannot flush the session like in hibernate and send 50 or 100 records in a block to the DB. JPA send and retrieve each object in a transaction. Inserting 10&#8217;000 records takes minutes &#8230; 1&#8217;000&#8217;000 hours.
  
The solution is to use direcly JDBC, ETL product or frameworks (Spring Batch, &#8230;).
  
JPA is something great but it cannot manage everything 🙁 no party for the lazy developers &#8230; yet!