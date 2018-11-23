---
title: 'Basic SEO for Angular'
description: ''
date: 2018-05-05T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: /2018/05/05/basic-seo-for-angular/
categories:
  - Angular
  - SEO
tags:
  - angular
  - seo
 
image: '/assets/img/'

introduction: 'How to improve your Angular search engine performance'
---

# Basic SEO for Angular

The search engine optimization (SEO) is nearer wizardry than science.

Here a list of tips if you are want to improve the ranking of your angular application with Google search engine.

# Every page needs an URL
Every page of your app needs it's own URL.
This URL need to be directly accessible using the browser.

If you have 3 pages, e.g.: home, angular, java these pages have to be accessible directly using distincts URLs, e.g.:

mywebsite.com/home

mywebsite.com/angular

mywebsite.com/java

You should link your pages using `<a>` and avoid the use of `onClick`.

To have a direct access to your website pages you need a reverse proxy (e.g. nginx) or a server response (e.g. java backend).

# You want to test with Chrome?

Google uses an old version of Chrome to render and index the pages.
When you optimize your website you should think that maybe for the SEO is better to think that the page will be indexed by a Chrome version 20 versions older (currently version 41).
[Rendering on Google Search](https://developers.google.com/search/docs/guides/rendering).

# Become friend of the Google's tools

These tools should be part of your weekly routine (daily if you work in marketing):

https://analytics.google.com/

https://www.google.com/webmasters/tools/home

# analytics.js is not enough

Your JavaScript Application (Single Page Application) maybe shows a different adress for every page that the user navigate but analytics.js is not reloaded and Google Analytics wont log the visit.

Google explains clearly the problem:
[Analytics and SPA](https://developers.google.com/analytics/devguides/collection/analyticsjs/single-page-applications)
and the solution: [Autotrack] (https://github.com/googleanalytics/autotrack)

# Help Google adding a sitemap

A sitemap file helps Google to know which pages your app contains.

You can prepare a complex xml file with more information and details (images, type etc.) or a simple txt file that contains only the list of the url exposed to the world.

You can test and submit this file in the webmasters' console. 
