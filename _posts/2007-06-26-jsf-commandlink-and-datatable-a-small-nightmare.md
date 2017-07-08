---
title: 'JSF: commandLink and datatable a small nightmare'
date: 2007-06-26T18:05:05+00:00
author: Marco Molteni
layout: post
http://molteni.wordpress.com/2007/06/26/jsf-commandlink-and-datatable-a-small-nightmare/
permalink: /2007/06/26/jsf-commandlink-and-datatable-a-small-nightmare/
categories:
  - Javaserver Faces
  - JSF
---
Many developers had problems using a **commandLink** in a column&#8217;s **datatable**.
  
Often when the user clicks on the link the backing bean method linked by the action is not called.

In the picture the first icon edits and the second delete the row &#8230;

[![Row of Datatable](http://molteni.files.wordpress.com/2007/06/table_row.png)](http://molteni.files.wordpress.com/2007/06/table_row.png "Row of Datatable")

According to Sun this is a <a href="https://javaserverfaces.dev.java.net/issues/show_bug.cgi?id=69" target="_blank">feature </a>and it will not be solved.

The most used workaround is to place the managed bean with the methods in the **session** scope 🙁

You can find a good explanation of the problem and an alternative solution here:
  
 <http://typo.ars-subtilior.com/articles/2007/02/07/jsf-datatable-and-commandlink>