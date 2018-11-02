---
id: 885
title: 'Angular and D3.js &#8211; tutorial'
date: 2017-02-10T00:25:39+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
guid: http://javaee.ch/?p=885
permalink: /2017/02/10/angular-and-d3-js-tutorial/
image: /wp-content/uploads/2017/03/logo-d3-100x12.png
categories:
  - Angular
  - Angular 2
  - d3.js
  - Demo Application
  - tutorial
tags:
  - Angular
  - Angular 2
  - d3.js
---
Goal: use the [D3.js](https://d3js.org) library in an Angular 2 project.

Result: the result is visible here 

<http://angular.cafe/d3-example>

<img class="alignnone size-full wp-image-884" src="{{site.baseurl}}/assets/img/uploads/2017/02/Ohne-Titel.png?resize=842%2C484" data-recalc-dims="1" />

Here the steps to integrate the library with Angular 2.

In the `package.json` we have to declare the dependencies with d3:

    "@types/d3": "^4.4.1",
    "@types/d3-scale": "^1.0.6",
    "d3": "^4.5.0",
    "d3-scale": "^1.0.4",
    

In `vendor.ts`:

    // d3.js
    import &#039;d3/build/d3.min&#039;;
    import &#039;d3-scale/build/d3-scale.min&#039;;
    

In the [component](https://github.com/marco76/SpringAngular2TypeScript/blob/master/webClient/src/app/components/d3.component.ts) that will generate the view you have to import the d3 types:

    import * as d3 from "d3";
    import * as d3scale from "d3-scale";
    

In the component we declare the style used and we reference the external xml

    @Component({
        selector: &#039;d3-example&#039;,
        templateUrl:&#039;../html/d3.html&#039;,
        providers: [ConstantsService, Location],
        styles:[`.chart div {
        font: 10px sans-serif;
        background-color: steelblue;
        text-align: right;
        padding: 3px;
        margin: 1px;
        color: white;
        }`],
        encapsulation: ViewEncapsulation
            .None
        })
    
    

The external html simply declare the object that will be modified by the library (chart):

    <h3>Example with D3</h3>
    <div class="chart"></div>
    

Your class has to implement [AfterViewInit](https://angular.io/docs/ts/latest/guide/lifecycle-hooks.html). This method is called after that Angular initialises the component's view and child views.

`export class D3Component implements OnInit, AfterViewInit`

    ngAfterViewInit() {
        var data = [10, 20 ,30 ,15, 4, 26, 33];
        d3.select(".chart")
            .selectAll("div")
            .data(data)
            .enter().append("div")
            .style("width", function(d) { return d*10 + "px"; })
            .text(function(d) { return d; });
    }