---
title: 'Configure Bootstrap with Angular 2, Webpack and Spring &#8211; Tutorial'
date: 2017-02-06T00:26:41+00:00
author: Marco Molteni
layout: post
permalink: /2017/02/06/configure-bootstrap-with-angular-2-webpack-and-spring-tutorial/
dsq_thread_id:
  - "5565869589"
image: /wp-content/uploads/2017/03/logo-bootstrap-100x37.png
categories:
  - Angular
  - Angular 2
  - Demo Application
  - java
  - JPA
  - REST
  - Spring
  - TypeScript
tags:
  - Angular
  - Angular 2
  - bootstrap
  - H2
  - spring
  - spring boot
  - webpack
---
I update the reference Angular/Java application for this blog (<http://angular.cafe>) adding [Bootstrap](http://getbootstrap.com/css/).
  
The new homepage is now like this:

[<img class="alignnone wp-image-855 " src="{{site.baseurl}}/assets/img/uploads/2017/02/home-1.png?resize=479%2C265" data-recalc-dims="1" />]({{site.baseurl}}/assets/img/uploads/2017/02/home-1-e1486332505220.png)

The data comes from an H2 database deployed with the application.

To use Bootstrap this step are required:
  
in [package.json](https://github.com/marco76/SpringAngular2TypeScript/blob/master/webClient/src/package.json) add the dependency to bootstrap

    "bootstrap": "3.3.7",
    "jquery": "3.1.1"
    

or import it with `npm install bootstrap@3 -save`
  
and `npm install jquery -save`

I didn&#8217;t import Bootstrap 4 because is still in alpha version.

We have to tell to webpackage to import the files in the distribution package. In [vendor.ts](https://github.com/marco76/SpringAngular2TypeScript/blob/master/webClient/src/vendor.ts) add:

    // bootstrap 
    import 'jquery';
    import 'bootstrap/dist/js/bootstrap';
    import 'bootstrap/dist/css/bootstrap.min.css';
    

in [webpack.common.js](https://github.com/marco76/SpringAngular2TypeScript/blob/master/webClient/src/config/webpack.common.js) I added the &#8216;synonyms&#8217; of jquery:

    new webpack.ProvidePlugin({
    jQuery: 'jquery',
    $: 'jquery',
    jquery: 'jquery'
    })
    

I created a menu component that contains the bootstrap tags:
  
You can see the content here: <a href="https://github.com/marco76/SpringAngular2TypeScript/blob/master/webClient/src/app/html/menu.html" target="_blank">home.html</a>
  
[<img class="alignnone wp-image-862 size-large" src="{{site.baseurl}}/assets/img/uploads/2017/02/code.png?resize=945%2C322" data-recalc-dims="1" />]({{site.baseurl}}/assets/img/uploads/2017/02/code.png)
  
The component [menu.component.ts](https://github.com/marco76/SpringAngular2TypeScript/blob/master/webClient/src/app/components/menu.component.ts) is self explanatory:

```typescript

import {Component} from "@angular/core";

@Component({
    selector : 'bootstrap-menu',
    templateUrl :'../html/menu.html'
})
export class MenuComponent {
constructor(){
}}

```

To reuse the same menu in every page I call the menu component from the [app.component.ts](https://github.com/marco76/SpringAngular2TypeScript/blob/master/webClient/src/app/components/app.component.ts) that is the main component of the module:

```typescript

    @Component({
    selector: 'my-app',
    template: `<bootstrap-menu></bootstrap-menu>
       <router-outlet></router-outlet>
    `,
    providers: [HttpModule, ConstantsService, Location]
    })
    
```
    

the template calls the bootstrap menu <bootstrap-menu> followed by the component called by the router.

In the developer console you can see the data sent by the server:
  
[<img src="{{site.baseurl}}/assets/img/uploads/2017/02/chrome-1-e1486333521414.png?resize=900%2C389" alt="" class="alignnone size-full wp-image-866" data-recalc-dims="1" />]({{site.baseurl}}/assets/img/uploads/2017/02/chrome-1-e1486333521414.png)