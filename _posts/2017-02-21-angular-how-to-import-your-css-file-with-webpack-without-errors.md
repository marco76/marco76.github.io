---
id: 946
title: 'Angular 2: how to import your css file with Webpack without errors'
date: 2017-02-21T23:10:21+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=946
permalink: /2017/02/21/angular-how-to-import-your-css-file-with-webpack-without-errors/
image: /wp-content/uploads/2017/03/webpack-logo-100x37.png
categories:
  - Angular
  - Angular 2
  - Uncategorized
tags:
  - Angular
  - css
  - error
  - webpack
---
According to the Angular [documentation](https://angular.io/docs/ts/latest/guide/component-styles.html) load an external style file in a component is as easy as :

    template:
     `
        <h2>{{hero.name}}</h2>
        <hero-team [hero]=hero></hero-team>
         <ng-content></ng-content>
    `,
      styleUrls: ['app/hero-details.component.css']
    

This configuration is challenging when you have building process that include Webpack. In our reference [example application](http://angular.cafe) the call of the css file using `styleUrl`

    @Component({
        selector: 'cv-experience',
        templateUrl:'cv-experience.html',
        styleUrls:['cv.css']
    })
    

resulted in this error:

    {'Uncaught Error: Expected 'styles' to be an array of strings. at assertArrayOfStrings (eval at <anonymous> '}
    

To solve the issue we had to migrate from Webpack 1.x to the new version 2 and <a href="https://github.com/marco76/SpringAngular2TypeScript" target="_blank">call the css file with the following code</a>:

    @Component({
        selector: 'cv-experience',
        templateUrl:'cv-experience.html',
        styles:[require('./cv.css').toString()]
    })
    

This is the loader of Webpack

    { test: /\.css$/, loader: "style-loader!css-loader" },
    { test: /\.scss$/,loaders: ['style', 'css', 'postcss', 'sass'] }

&nbsp;