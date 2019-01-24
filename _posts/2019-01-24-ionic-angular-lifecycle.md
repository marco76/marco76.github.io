---
title: 'Ionic and Angular lifecycle'
description: 'How to set properties'
date: 2019-01-23T21:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: /ionic-angular-lifecycle
categories:
  - Angular, Ionic
tags:
  - angular, ionic
 
image: '/assets/img/'

introduction: 'ionic vs angular lifecycle, mixing components'
---

There is the excellent blog of ionic that shows how the lifecycle of a ionic component works.
[Ionic Blog](https://blog.ionicframework.com/navigating-lifecycle-events/)

Some problems could arise when ionic components are mixed with angular components.

A ionic component has the following lifecycle (creation only):

`[ngOnInit]-> ionViewDidLoad -> ionViewWillEnter -> ionViewDidEnter`

Angular OnInit precedes ionViewDidLoad.

If you initialise your component in ionViewDidLoad and your ionic component consumes some non ionic components you could have some surprises, e.g. ionic HomeComponent consumes angular SimpleComponent:
```html
<ion-content padding>
   <app-simple [message]="message"></app-simple>
</ion-content>
```
```
┌───────────────────────────────┐
│                               │
│  HomeComponent                │
│                               │
│  ┌────────────────────────┐   │
│  │                        │   │
│  │    SimpleComponent     │   │
│  │                        │   │                    
│  └────────────────────────┘   │                               
│                               │
└───────────────────────────────┘
```

In this case the (simplified) lifecycle of the page is the following:

`Home.onInit -> Simple.onInit -> Home.ionViewDidLoad -> Home.ionViewDidEnter`

In our case we pass the value of 'message' to the angular component.

If this property is initialised in Home.ionViewDidLoad the SimpleComponent won't use it without a change detector (https://angular.io/api/core/OnChanges) in the angular component.

If the value of `message` is initialised and never changed, the change detector should be avoided.

## Solutions for the initialisation of the property

- Initialisation in the Ionic component `constructor`.
- Initialisation in the Ionic [OnInit](https://angular.io/api/core/OnInit) method.
- Initialise the property in Home.ionViewDidLoad and create the SimpleComponent only after the initialisation. e.g.: `<app-simple *ngIf="initialisationCompleted" [message]="message"></app-simple>`
- Handle the change detection in SimpleComponent.

The last option is to avoid because of the added complexity and in many cases we simply don't have the possibility to change the component (external component).

## Example
Here you find a basic example of the problem discussed:

[https://ionic-inq3qt.stackblitz.io](https://ionic-inq3qt.stackblitz.io)