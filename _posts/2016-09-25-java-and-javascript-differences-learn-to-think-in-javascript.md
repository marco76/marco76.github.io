---
id: 61
title: Java and JavaScript differences. Learn to think in JavaScript!
date: 2016-09-25T15:29:41+00:00
author: Marco Molteni
layout: post
guid: http://ngjava.com/?p=61
permalink: /2016/09/25/java-and-javascript-differences-learn-to-think-in-javascript/
categories:
  - First Steps
  - Uncategorized
---
When you switch from Java to JS and TypeScript is good to pay attention to this differences: [Post in continuous improvement …] 

### 1. Equals are not equals

Coming from Java pay attention to the comparison operators in JavaScript. They behave differently from Java!
  
TypeScript requires explicit conversions and is much more similar to Java.

<table border="0" width="465" cellspacing="0" cellpadding="2">
  <tr>
    <td valign="top" width="67">
    </td>
    
    <td valign="top" width="109">
      JavaScript
    </td>
    
    <td valign="top" width="104">
      Comment JS
    </td>
    
    <td valign="top" width="183">
      Typescript
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="67">
      ==
    </td>
    
    <td valign="top" width="109">
      5 == 5 : true<br /> 5 == ‘5’: true<br /> ’’== 0 : true
    </td>
    
    <td valign="top" width="104">
      Automatic conversion of the type
    </td>
    
    <td valign="top" width="183">
      5 == 5 : true<br /> 5 == ‘5’: compiler error<br /> ’’== 0 : compiler error
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="67">
      ===
    </td>
    
    <td valign="top" width="109">
      5 === 5 : true<br /> 5 === ‘5’: false<br /> ’’ === 0: false
    </td>
    
    <td valign="top" width="104">
      Type and value are compared
    </td>
    
    <td valign="top" width="183">
      5 === 5 : true<br /> 5 === ‘5’: compiler error<br /> ’’ === 0: compiler error
    </td>
  </tr>
</table>

&nbsp;

### 2. Null is not undefined

In Java ‘null’ values are assigned to objects without value or undefined.
  
In JS and TS a variable without value is ‘undefined’ but not ‘null’. null is a value and can be assigned to a variable.
  
To add more complexity:

<pre class="brush: jscript; title: ; notranslate" title="">null == undefined :  true
null === undefined : false
null == null : true
null === null: true
undefined === undefined : true
</pre>