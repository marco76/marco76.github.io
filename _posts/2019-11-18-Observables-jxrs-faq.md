---
title: 'Observables and jxrs tutorial / FAQ'
description: 'Understanding observables and JXRS'
date: 2019-11-17T01:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: /jxrs-tutorial
categories:
  - Angular, rxjs
tags:
  - Angular

introduction: 'Common questions about Observables'
---
_this post is still work in progress it will be updated often in the future, trying to quickly clarify some 'reactive thinking' to Java devs_

## Why I have to unsubscribe?
The subscriptions can survive the components that created them.
This could create a memory leak in the application.

## When I don't need to unsubscribe?
- [ActivateRoute](https://angular.io/guide/router#!%23route-parameters) 

  _The ActivatedRoute and its observables are insulated from the Router itself. The Router destroys a routed component when it is no longer needed and the injected ActivatedRoute dies with it._

- [AsynchPipe](https://angular.io/api/common/AsyncPipe) `{{expression | asynch}}`.

  _When the component gets destroyed, the async pipe unsubscribes automatically to avoid potential memory leaks._

- Singletons: e.g. `AppComponent`, `Services` they are instantiated only once.

- Finite subscriptions:
  - Using the [HttpClient](https://angular.io/guide/http)

- Operators that unsubscribe themselves (usually after they complete):

  - `take()`
  - `first()`
  - `debounce()`
  - `switchMap()`
  - ...

## Are Observables eager or lazy?
Lazy. Observables starts only when an observer subscribe (e.g. `.subscribe()`).

Promises are eager.

## Are Observables synchronous or asynchronous
They can be synchronous or asynchrounous.

Promises are only asynch.

## How many values are returned?
From 0 to potentially infinite values.

## Observables can manage multiple observers?

## When to unsubscribe
There are multiple strategies:

1. `ngOnDestroy` and Array

A very common solution is to unsubscribe when the component is destroyed.
A common practice is to store all the [Subscriptions](https://rxjs-dev.firebaseapp.com/guide/subscription) in an array and unsubscribe them in a loop `forEach`.

``` javascript
private subscriptions: Subscription[] = [];

ngOnInit() {
  this.subscriptions.push(this.yourObservables.subscribe())
}

ngOnDestroy() {
  this.subscriptions.forEach(subscription => subscription.unsubscribe());
}
```

2. `Subject` and `takeUntil`

_example_

## Some useful operators
- `takeUntil`: 
- `takeWhile`:  