---
title: 'Use Font Awesome with Angular'
description: 'How to configure your Angular app to use Font Awesome'
date: 2017-07-16T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: /2017/07/16/font-awesome-angular/
categories:
  - Angular
tags:
  - angular
 
image: '/assets/img/'

introduction: 'How to configure your Angular app to use Font Awesome'
---
Install the Font Awesome npm package
```
npm install font-awesome --save
```

In the _.angular-cli.json_ file you have to add the reference to the _font-awesome.css_ that you just installed:
```javascript
	"styles": [
	  "styles.css",
	  "../node_modules/bootstrap/dist/css/bootstrap.min.css",
	  "../node_modules/font-awesome/css/font-awesome.css"
	],
```

In your pages you can reference the icons, e.g.:

```html
<a href="http://twitter.com/marcomolteni" target="_blank"><i class="fa fa-twitter fa-2x menu-icon" aria-hidden="true"></i></a>
```
with the following result: 
<a href="http://twitter.com/marcomolteni" target="_blank"><i class="fa fa-twitter fa-2x menu-icon" aria-hidden="true"></i></a>

[<img src="{{site.baseurl}}/assets/img/uploads/2017/07/16/footer.png" alt=""/>]({{site.baseurl}}/assets/img/uploads/2017/07/16/footer.png)

Here the list of the available icons [http://fontawesome.io/icons/](http://fontawesome.io/icons/)
