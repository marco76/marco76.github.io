---
id: 904
title: Add highlight.js to an Angular 2 application
date: 2017-02-13T20:44:45+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
guid: http://javaee.ch/?p=904
permalink: /2017/02/13/add-highlight-js-to-an-angular-2-application/
dsq_thread_id:
  - "5565701684"
image: /wp-content/uploads/2017/03/logo-highlightjs-100x38.png
categories:
  - Angular
  - Angular 2
  - javascript
  - TypeScript
  - Uncategorized
tags:
  - Angular
  - directive
  - TypeScript
---
With the goal to build my own blog application (for educational purposes) I integrated the [highlight.js library](https://highlightjs.org) in the [demo application](http://angular.cafe).

At the moment there are problems with safari and mobile platforms, a js error in the library.

The implementation is based on this answer in [Stack Overflow](http://stackoverflow.com/questions/37307943/highlight-js-does-not-work-with-angular-2) (thanks to the author) 😉

In packages.json you have to import the highlight library and the type:

<pre class="brush: jscript; title: ; notranslate" title="">'highlight.js': '^9.9.0',
'@types/highlight.js': '^9.1.9',
</pre>

The implementation is done via a directive:

<pre class="brush: jscript; title: ; notranslate" title="">import {Directive, ElementRef, AfterViewInit} from '@angular/core';
import * as hljs from 'highlight.js';

@Directive({
    selector: 'code[ highlight]' // css selector for the attribute
})
export class HighlightCodeDirective implements AfterViewInit{
    constructor(private eltRef:ElementRef) {
    }

    ngAfterViewInit() {
        hljs.highlightBlock(this.eltRef.nativeElement);
    }
}
</pre>

The html code of the example:

<img class="alignnone wp-image-902 size-full" src="{{site.baseurl}}/assets/img/uploads/2017/02/blog_highlight_small-e1487011265898.png?resize=600%2C411" data-recalc-dims="1" />

and the result:

<img class="alignnone wp-image-903 size-full" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2017/02/blog_highlight_2_small-1-e1487011315602.png?resize=600%2C647" data-recalc-dims="1" />