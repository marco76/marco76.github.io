---
title: 'Common highlighters with the Shadow DOM'
description: ''
date: 2018-12-27T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'javascript'
color: '#7AAB13'
permalink: /highlighters-shadow-dom
categories:
  - Web, Javascript
tags:
  - Web, Javascript
 
image: '/assets/img/'

introduction: 'Use the code highlighters with the Shadow DOM in your Web Components'
---

# Code Highlighters with the Shadow DOM

If your website uses the [Shadow DOM / Web components](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM) and you want to highlight the code source  in your webpage.

The common highlighters search the `<code>`code tag in the javascript `document` because of this     your shadowed `<code>` is not visible.

## [highlight.js](https://highlightjs.org)
If you use highlight.js you have to call the function `highlightBlock` in the code that generates your shadow DOM:
```javascript
let blocks = this.shadowRoot.querySelectorAll('pre code');

for (let i = 0; i< blocks.length; i++) {
                            hljs.highlightBlock(blocks[i]);
```

## [prism.js](https://prismjs.com)
With prism.js you can pass the container reference:
```javascript
Prism.highlightAllUnder(this.shadowRoot);
```