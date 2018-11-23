---
id: 178
title: How to map an SQL Server XML Type using Hibernate
date: 2012-10-16T14:02:44+00:00
author: Marco Molteni
layout: post
guid: http://molteni.wordpress.com/?p=178
permalink: /2012/10/16/how-to-map-an-sql-server-xml-type-using-hibernate/
jabber_published:
  - "1350392564"
categories:
  - Hibernate
  - java
  - JPA
  - SQL Server
---
The solution to map an SQL Server XML Type using Hibernate is quite easy:

if you have the MESSAGE field in your SQL SERVER with the format XML

[<img class="alignnone size-full wp-image-179" title="sql" alt="" src="http://molteni.files.wordpress.com/2012/10/sql.png?resize=179%2C27" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/10/sql.png?resize=179%2C27)

you can map it using a simple Hibernate String type in your .hbm.xml:

[<img class="alignnone size-full wp-image-181" title="mapping" alt="" src="http://molteni.files.wordpress.com/2012/10/mapping.png?resize=488%2C65" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/10/mapping.png?resize=488%2C65)

&nbsp;

and in your Java class:

&nbsp;

[<img class="alignnone size-full wp-image-180" title="class" alt="" src="http://molteni.files.wordpress.com/2012/10/class.png?resize=339%2C353" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/10/class.png?resize=339%2C353)