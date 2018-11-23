---
title: 'Mixing Angular with AngularJS'
description: 'How to avoid a migration integrating Angular Components with AngularJS'
date: 2018-11-07T21:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: migration-from-angularjs-to-angular/
categories:
  - Angular
  - AngularJS
tags:
  - angular
  - angularJS
 
image: '/assets/img/'

introduction: 'Integrate Angular component in an AngularJS application'
---

# Why mix AngularJS and Angular

In a previous project / client we had a working application developed using AngularJS. The developer (me!) could not justify the budget for a full migration of the application just to 'be more up to date' with the technology.

The application had no issue and the users were happy.

The benefits of Angular compared to AngularJS are obvious and it was a pity to don't use the new features and increase our development speed.

To work with Angular we found the solution to integrate Angular (5.x) with AngularJS through a 'downgrade' of the Angular components.

>The management didn't need any new budget or project for the 'Upgrade'

we did it step by step implementing the new features and 'downgrading' them to AngularJS

>The management was talking about a budget for a future migration from AngularJS to Angular 2 ... but we were already deploying using the version 5 :)

## Why this article now that everybody uses Angular

Because I see many companies that are still using AngularJS, big investments have been done in AngularJS projects and in many cases a migration cannot be done financially or technically.

For our migration we used the awesome documentation of Angular:

[Angular Migration](https://angular.io/guide/upgrade)

Here you can find some notes about this migration.

Here you can see a more modern approach using Angular Elements used by a Google Engineer: [Upgrading to Angular without NgUpgrade](https://goo.gl/Naf1TB)

## Upgrade strategy

Our application was developed using an internal (at the time no more maintained!!!) framework built on top of AngularJS.

Because of the complexity hidden in this internal framework we decided to use the current AngularJS application to load Angular modules.
<img src="https://angular.io/generated/images/guide/upgrade/dom.png"/>
The goal was to develop all the new features with the newest version of Angular and migrate, if and when possible (time and money restriction) the old AngularJS components.

> We started with AngularJS, Bootstrap and Gulp, we added Angular (version 4 followed by the 5) with Angular Material Components and Webpack.


### Webpack

In Webpack we declared as entry point the Angular application:

```javascript
module.exports = {
    entry: {
        'app': './src/main.ts',
        'polyfills': './src/polyfills.ts',
        'vendor': ['./src/vendors.ts']
    },
    output: {
        filename: '[name].js',
        path: path.resolve(__dirname, 'dist')
    },
```

### package.json
As you can imagine our package.json was colorful:
```javascript
"dependencies": {
    ... Angular ...
    "@angular/animations": "5.2.9",
    "@angular/cdk": "5.2.4",
    "@angular/common": "5.2.9",
    "@angular/compiler": "5.2.9",
    "@angular/core": "5.2.9",
    ... Material Design ...
     "@angular/material": "5.2.4",
     "@angular/material-moment-adapter": "5.2.4",
     ... AngularJS ...
     "@uirouter/angularjs": "1.0.15",
     "angular": "1.6.9",
         "angular-animate": "1.6.9",
         "angular-cookies": "1.6.9",
     ... Bootstrap...
     "bootstrap": "3.3.7",
     ... Ads and self promotion :) ...
     "@molteni/array-utils": "0.0.6",
     "@molteni/av-components": "0.0.10",    
```

### main.ts

We booted the application using main.ts.
We imported "./app/app.js" that is the entry point for AngularJS.
The  bootstrapModule inject the AngularJS application in the html body (`app` tag).

```typescript
import './polyfills.ts';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';
import { UpgradeModule } from "@angular/upgrade/static";
import "./app/app.js"

platformBrowserDynamic().bootstrapModule(AppModule)
    .then(ref => {
        const upgrade = ref.injector.get(UpgradeModule) as UpgradeModule;
        upgrade.bootstrap(document.body, ['app'], {strictDi: false} );
    });
```

### index.html

Surprise surprise our index.html contains the reference to AngularJS (`<app>`) and to Angular (`<ang2>`).

```html
<!doctype html>
<html lang="de">
    <head>
        <base href="/">
        <meta charset="utf-8">
        <title>...</title>
        <meta name="viewport" content="width=device-width initial-scale=1 maximum-scale=1 user-scalable=no">
        <meta name="mobile-web-app-capable" content="yes">
        <meta name="apple-mobile-web-app-capable" content="yes">
        <meta name="apple-mobile-web-app-status-bar-style" content="black">
        <link rel="icon" href="data:;base64,...">
        <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
    </head>

    <body>
        <app>Loading...</app>
        <ang2></ang2>
    </body>
</html>
```

The Angular component that reference `<ang2>` sounds like this:

```typescript
import { Component } from '@angular/core';
import { NotificationService } from "./components2/common/service/notification.service";
import { ParameterLoaderService } from "./components2/vm/service/ParameterLoaderService";
import { HttpService } from "./components2/httpservice/http.service";
declare let require: any;

@Component({
    selector: 'ang2',
    template: ``,
    providers: [NotificationService, ParameterLoaderService, HttpService]
})
export class AppComponent  {
    constructor(notificationService : NotificationService, parameterLoaderservice : ParameterLoaderService) {
        console.log('constructor AppComponent')
    }
}
```

### angular.module

In our `app.js` we declare the application module:

```javascript
import { downgradeComponent, downgradeInjectable } from '@angular/upgrade/static';

angular.module('app', [
    uiRouter, ngTranslate, ngTranslateStaticFilesLoader, ngCookies, ngResource, ngSanitize,
    uiBootstrap, 'dndLists', Components.name
])
```

the module declares directive that come from Angular:
``` 
.directive('appfooter', downgradeComponent({component: FooterComponent}))
.directive('appreport', downgradeComponent({component: ReportComponent}))  
```
with this code we can use our Angular components inside AngularJS components.s

### The proxy from AngularJS to Angular

To use Angular components inside AngularJS components and pages we injected proxies created in AngularJS, e.g.
This was due to be compatible with the existing framework and pass the parameter from Angular to AngularJS:

report.js:
```javascript
import angular from 'angular';
import uiRouter from '@uirouter/angularjs';

import reportComponent from './report.component';

let reportModule = angular.module('report', [
    uiRouter
])
    .config(/*@ngInject*/['$stateProvider', '$urlRouterProvider',($stateProvider, $urlRouterProvider) => {
        $urlRouterProvider.otherwise('/');

        $stateProvider.state('report', {
            url: '/report/:productNr',
            params: {productNr: null},
            template: '<report></report>'
        });
    }])
    .directive('report', reportComponent);

export default reportModule;
```
report.component.js
```javascript
import template from './report.html';
import controller from './report.controller';

let reportComponent = function () {
    return {
        restrict: 'E', scope: {}, template, controller, controllerAs: 'vm', bindToController: true
    };
};

export default reportComponent;
```

report.controller.js
```javascript
class ReportController {
    constructor() {
    }
}

export default ReportController;
```

report.html
```
<navbar></navbar>
<messages></messages>
<main>
   <appreport></appreport>
</main>
```

The referenced Angular component:
report.component.ts
```typescript
@Component({
    template: require('./report.component.html'),
    selector: 'appreport',
    providers: [HttpService, NotificationService]
})
export class ReportComponent implements OnInit {
    
...}

```
and report.component.html
```html
<div flex
     layout="column"
     style="max-width: 70%; width: 70%; align-items: center; padding-bottom: 200px">


    <av-autocomplete style="width:400px;"
                     [selectableObjects]="selectable"
                     [properties]="{placeholder: 'Name oder Nummer'}"
                     (changeText)="onTextChanged($event)"
                     (selectItem)="onSelected($event)"></av-autocomplete>

    <mat-spinner *ngIf="isLoading" style="margin:0 auto;"></mat-spinner>
...
```

The component is declared as [EntryComponent](https://angular.io/guide/entry-componentss) in the module:
```
entryComponents: [
        FooterComponent,
        ReportComponent,
```

### Conclusion
The 'migration' was successful, the new developers enjoyed to work with the newest versions of Angular.

We noticed some minor issues with the rendering performances (in particular with IE) but the users didn't complain.