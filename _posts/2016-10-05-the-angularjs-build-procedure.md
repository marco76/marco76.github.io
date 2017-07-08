---
id: 84
title: The Javascript and AngularJS build procedure
date: 2016-10-05T11:44:02+00:00
author: Marco Molteni
layout: post
guid: http://ngjava.com/?p=84
permalink: /2016/10/05/the-angularjs-build-procedure/
categories:
  - First Steps
  - javascript
  - Uncategorized
---
<p dir="auto">
  A Java developer we have the good habits to split the building process of a project in steps.<br />In Javascript we use the same approach with some differences. An example of simplified build procedure:
</p>

<div class="custom-html-block">
  <table>
    <tr>
      <th width="200px">
        Java build
      </th>
      
      <th>
        Angular / Javascript build
      </th>
    </tr>
    
    <tr>
      <td>
        code
      </td>
      
      <td>
        code
      </td>
    </tr>
    
    <tr>
      <td>
        compile
      </td>
      
      <td>
        <i>transpile</i>
      </td>
    </tr>
    
    <tr>
      <td>
        test
      </td>
      
      <td>
        test
      </td>
    </tr>
    
    <tr>
      <td>
        &nbsp;
      </td>
      
      <td>
        <i>concatenate</i>
      </td>
    </tr>
    
    <tr>
      <td>
        &nbsp;
      </td>
      
      <td>
        <i>minification</i>
      </td>
    </tr>
    
    <tr>
      <td>
        package
      </td>
      
      <td>
        package
      </td>
    </tr>
    
    <tr>
      <td>
        deploy
      </td>
      
      <td>
        deploy
      </td>
    </tr>
  </table>
</div>

&nbsp;

<p dir="auto">
  <strong>Differences between Java and Javascript</strong><br />1. In Java we <em>compile</em>, in Javascript we <em>transpile</em>. This step is needed only if you are not using the scripting language of the target platform. If you use TypeScript you need to convert your code in a code compatible with your target interpreter (i.e. ECMA5).
</p>

<p dir="auto">
  2. Concatenation: when an application is distributed is easier to manage only one file than dozens. The concatenation is used to merge multiple files in only one. This is similar to the concept of FatJar in Java.
</p>

<p dir="auto">
  3. Minification: the code file used in a JS project contains many useless characters (for the interpreter). The minification process try to optimize the code reducing the size of the JS file. In Java is the compile that optimize the code.
</p>

<p dir="auto">
  <strong>Tools<br /></strong>In Java the common used tools for the building process are : maven, gradle and ant
</p>

<p dir="auto">
  In Javascript we can split this tools in 3 categories:<br />Package manager to handle the dependencies: <a href="https://www.npmjs.com" target="_blank">npm</a>, <a href="https://bower.io" target="_blank">bower</a><br />Task Runners: <a href="http://gruntjs.com/" target="_blank">Grunt</a>, <a href="http://gulpjs.com/" target="_blank">Gulp.js</a><br />Module bundlers: <a href="https://webpack.github.io/" target="_blank">WebPack</a>
</p>