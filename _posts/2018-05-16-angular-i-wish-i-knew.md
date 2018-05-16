---
title: 'Starting a new Angular project - I wish I knew'
description: ''
date: 2018-05-15T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: /new-angular-project-i-wish-i-knew/
categories:
  - Angular
tags:
  - angular
 
image: '/assets/img/'

introduction: 'Lessons learned'
---

# A new Angular project a new opportunity to improve quality

Every time we start a new project we have the opportunity to include the previous experience in the new product and improve the quality of the final product.

Here you can find some tips that come from my personal experience.

## Define and use strict guidelines

The chaos in your project an exponential value of the number of the participants.

Angular has excellent guidelines:
[https://angular.io/guide/styleguide](https://angular.io/guide/styleguide)

Review it with your developers. Use it or adapt to your own standard. The guidelines can be integrated in your IDE (e.g. TSLint).

## Define a graphic charter

Almost every business application need a Data Table (e.g. [https://material.angular.io/components/table/overview](https://material.angular.io/components/table/overview))
... but where you place the 'add' button?

How to edit the existing values? With overlay or a new page?

Are you going to use Bootstrap, Material other?

The filter is server side? backend? multiple columns?

The design of these elements have to be defined at the beginning of the project and if possible create your own components that implements this guideline.

## I found a solution on GitHub ... we use it ... no ... wait ...

You found a very nice component that you need for your project and it will save you days of work, can you use it?

Maybe ... check how many committers the project have, the speed and rate of development, the answers of the committers in the forums. In 2 years when you will found a bug in production or you will have a performance problem, who will correct it?

If you are looking for components I recommend:

* Clarity : [https://vmware.github.io/clarity/](https://vmware.github.io/clarity/), I find the design very nice and clear guidelines
* Angular Material: [https://material.angular.io/](https://material.angular.io/), If you start with Material you will have to put a lot of energies in the original code and the Material guidelines. Often you won't find an answer.

You can find other interesting components libraries:
* Teradata: [https://teradata.github.io/](https://teradata.github.io/)
* Primefaces: [https://www.primefaces.org/primeng/#/](https://www.primefaces.org/primeng/#/)
* ng-bootstrap: [https://ng-bootstrap.github.io/#/home](https://ng-bootstrap.github.io/#/home)
* ngx-bootstrap: [https://valor-software.com/ngx-bootstrap/#/](https://valor-software.com/ngx-bootstrap/#/)



## Modules as much as you can and until when it make sense

Group your components and code in modules, the advantages:

* easy [lazy loading](https://angular.io/guide/lazy-loading-ngmodules) in your Routes : 

``` javascript
const routes: Routes = [
  {
    path: 'your-module-component',
    loadChildren: 'app/my-module/mymodule.module#MyComponent'
  },
``` 

* easily share components (e.g. export to npmjs)
* share the workload between teams 

## Define a communication strategy between components

How your components communicate between them?
* using @input @output?
* using a shared service at the parent level
* using a store? e.g. ngrx/store

In my opinion for most of the applications you don't need to use external libraries to create a 'single source of truth / store'.

You should define or better draw how the components communicate to avoid a spaghetti messages flow in your application.
 
The risk is very high when you modularize your application in small components.

## Think about performances

Working with JS frameworks is easy to quickly have performance problems. Angular helps us a lot in the tuning and optimization of the application, but it cannot replace your intervention.

Remember to :
* Validate your knowledge about the [detection change mechanism](https://blog.thoughtram.io/angular/2016/02/22/angular-2-change-detection-explained.html). 
Sometime changing to [OnPush](https://angular.io/api/core/ChangeDetectionStrategy) helps a lot of useless checks: `changeDetection: ChangeDetectionStrategy.OnPush`
* Check when it is better to hide or remove (`*ngIf`) elements from the DOM.
* Check the performances with different browsers and with your users' browser in particular.
* Verify with your browser development tools or with a simply `console.log()`how often your methods are called.

## Animations ... easy but not out of the box and not too many

In Angular it is easy to add visual effects that greatly improve the User eXperience. The choice of the type of animations is up to you. Decide with your user which animation are helpful to them.

A wonderful animation for every `:hover` can help the view effect during the presentation, but it could tire the user that has to see this animation thousand of time during the day.

Worth are the effects that show the change of data (delete, new etc.).

When you start the project remember that the animations require a change in state. When you build your data / component a state has to be recorded.

## Security and logging

At the beginning of the project you have to evaluate how to integrate the security in your application.

* Who asks the token to the oauth server, the backend or the frontend?
* We have to store some credentials in the browser session? In an Angular Store or Service?
* How much is the cost in performances of the user validation (usually done at every request).
* Which parts of the application are accessible to anonymous users and which require the login?

## Forms, are you reactive?

* Are you going to use Reactive Forms, Template driven forms or Dynamic forms?

If you don't know the difference you have to go here: [https://angular.io/guide/forms](https://angular.io/guide/forms)

Who comes from AngularJS or other frameworks/libraries could find natural to use Template Driven Forms (ngModel).

The problems of ngForms come when you start to update existing record. You have to manage the state of the record alone.
You should create a copy of the values, manage the copy in the form and update the original record.
 
Do you know how to clone a value using Javascript? This is another debate.
The problem is not with a single form developed from a single developer. The problems come when you have 30 forms developed by 5 developers.

I strongly suggest to use Reactive Forms and Dynamic Forms when possible, the maintenance is easier than the Template-driven forms.

Dynamic Forms require a lot of work but they can be used by all your developers in multiple projects.


## SEO
If you publish your website on internet you need some time to prepare and tune it for the Search Engines.
More here: [Basic SEO for Angular](http://javaee.ch/2018/05/05/basic-seo-for-angular/)