---
title: Easy and effective solutions to improve performances issues in AngularJS 1.x
date: 2017-03-19T19:57:07+00:00
author: Marco Molteni
layout: post
main-class: 'angular'

permalink: /2017/03/19/easy-and-effective-solutions-to-improve-performances-issues-in-angularjs-1x/
image: /wp-content/uploads/2017/03/angularjs-logo-100x27.png
categories:
  - Angular
  - AngularJS
tags:
  - angularjs
  - performances
---
Here a list of the easiest remedies with a positive impact on the performances of AngularJS applications.

### Loading time issues

#### -> Compression

Verify that the application is compressed before the delivery to the client.

The compression can be done by WebPack during the building phase or Tomcat/Ngix after the deploy as described here: [Better performances with Spring and Tomcat](http://javaee.ch/2017/02/20/better-performance-with-smaller-and-faster-angular-applications-using-spring-boot-and-tomcat/)

### Digest related issues

#### -> ng-if and ng-visible

`ng-if` doesn&#8217;t publish the code in the DOM if it&#8217;s false, on the contrary `ng-show` and `ng-hide` simply hide the code from the screen (using CSS styles) but it&#8217;s still in the DOM.

For complex and long parts of the screen it&#8217;s better to use `ng-if` rather than `ng-show (or ng-hide)`.

Here the official documentation:

ng-if : <https://docs.angularjs.org/api/ng/directive/ngIf>

ng-show: <https://docs.angularjs.org/api/ng/directive/ngShow>

#### -> ng-model-options

This is one of the most important measures if you have forms on your screen. By default every time the user press a key in your form a `digest`cycle is launched and all the elements on your screen (visible or hidden) are validated.

`ng-model-options` allows to ask Angular to wait for a trigger before processing of a user input.

e.g. `<div ng-model-options="{debounce: 200}">` tells AngularJS to wait for 0.2 seconds after the last keypress before to process the new input.

`<div ng-model-options="{updateOn: 'blur'}">`tells AngularJS to process the input when the focus pass to a different component.

Without these options AngularJS process all the DOM after every keypress.

More information here: <https://docs.angularjs.org/api/ng/directive/ngModelOptions>

#### -> Move the filters in the controller

The default filters in AngularJS are easy to use and very powerful but also performance killers.

For this reason the _comparable_ filters typically used in tables and list have been eliminated from [Angular (2 +)](https://angular.io/docs/ts/latest/cookbook/ajs-quick-reference.html#!#filters-pipes).

Here the link to a good article that explains how to improve the performances of the filters moving them in the controller: <https://toddmotto.com/use-controller-filters-to-prevent-digest-performance-issues/>

#### -> One-time binding

If you start an expression with `::` e.g. `{{::name}}` this is evaluated a one-time binding by AngularJS and his value is calculated only one during the first digest. More info here: <https://docs.angularjs.org/guide/expression>

These are the easiest and more effective solutions we used in our day to day work. If you have other ideas you can add them in the comments.

### Links

A list of performance improvements measures : <https://www.alexkras.com/11-tips-to-improve-angularjs-performance/>