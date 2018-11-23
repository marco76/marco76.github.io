---
id: 67
title: 'Angular JS and Java : dictionary'
date: 2016-09-26T19:12:10+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
guid: http://ngjava.com/?p=67
permalink: /2016/09/26/angular-js-and-java-dictionary/
categories:
  - First Steps
  - Uncategorized
tags:
  - Angular 2
  - angularjs
  - java
---
Angular and Java are full stack frameworks, for this reason the applications implemented with the 2 technologies tend to have similar logical layers.

The following table try to match the different terminology of Angular and Java. The match is not 100% correct but the goal is to help the developer to quickly find himself in the other technology.

<table border="0" width="836" cellspacing="0" cellpadding="2">
  <tr>
    <td valign="top" width="195">
      <strong>AngularJS and Angular 2</strong>
    </td>
    
    <td valign="top" width="326">
      <strong>Definition</strong>
    </td>
    
    <td valign="top" width="313">
      <strong>In Java</strong>
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="195">
      <em>Template</em>
    </td>
    
    <td valign="top" width="326">
      HTML with additional markup
    </td>
    
    <td valign="top" width="313">
      JSP or JSF page
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="195">
      <em>Directive</em>
    </td>
    
    <td valign="top" width="326">
      custom attributes for HTML
    </td>
    
    <td valign="top" width="313">
      custom tags in JSP, custom component in JSF
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="195">
      <em>Model</em>
    </td>
    
    <td valign="top" width="326">
      data showed in the view
    </td>
    
    <td valign="top" width="313">
      Model in Spring, Managed Bean in JSF
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="195">
      <em>Scope</em>
    </td>
    
    <td valign="top" width="326">
      context in where the model is stored
    </td>
    
    <td valign="top" width="313">
      Request / Session etc.
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="195">
      <em>Filter</em>
    </td>
    
    <td valign="top" width="326">
      formatting for the screen
    </td>
    
    <td valign="top" width="313">
      JSTL formatter in JSP, converter in JSF
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="195">
      <em>Controller</em>
    </td>
    
    <td valign="top" width="326">
      logic behind the screens
    </td>
    
    <td valign="top" width="313">
      @Controller in Spring, @RequestScoped
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="195">
      <em>Injector</em>
    </td>
    
    <td valign="top" width="326">
      retrieve object instances from the provider
    </td>
    
    <td valign="top" width="313">
      @Inject
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="195">
      <em>Module</em>
    </td>
    
    <td valign="top" width="326">
      container for a part of the app (services, directives, …)
    </td>
    
    <td valign="top" width="313">
      Package artifact
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="195">
      <em>Service</em>
    </td>
    
    <td valign="top" width="326">
      business logic
    </td>
    
    <td valign="top" width="313">
      @Service, @EJB
    </td>
  </tr>
</table>

&nbsp;

Don’t hesitate to suggest other terms or send corrections.