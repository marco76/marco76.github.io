---
title: 'Observables and RxJS tutorial / FAQ'
description: 'Understanding observables and RxJS'
date: 2019-11-17T01:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: /rxjs-tutorial
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

## When my Subscriber will start to receive values?
After the subscription when the Observable will emit the `next` value or, in case of `BehaviorSubject` immediately (previous or initial value).

## What is the difference between Subject, BehaviorSubject, ReplaySubject?
- _[Subject](https://github.com/ReactiveX/rxjs/blob/master/src/internal/Subject.ts)_ : emits the values only after the subscription, it doesn't store any value. It can publish and subscribe.
- _[BehaviorSubject](https://github.com/ReactiveX/rxjs/blob/master/src/internal/BehaviorSubject.ts)_ : it stores an initial value.
When subscribed it returns the last value emitted or the initial value.
This is very useful if you need to load data before it's effectively used / showed.
- _[ReplaySubject](https://github.com/ReactiveX/rxjs/blob/master/src/internal/ReplaySubject.ts)_ : it stores multiple values, it has a _bufferSize_ property that defines the number of values stored.
When subscribed it returns the values stored in the buffer.

<img src="/assets/img/uploads/2019/rxjs_subject_1.gif" alt="" />

## How to combine 2 streams, e.g. 2 database tables by key
This case is very frequent if you are working with databases and you have to combine two streams representing a database by key.
The best option you have is `combineLatest` :
- combines multiple streams
- uses the latest emitted values
- wait for each input stream to emit at last once
- emits an array of values, in order, containing the result of each stream

``` javascript
personWithAddress$ = combineLatest(
  this.personList$,
  this.adressList$)
)
```
this would emit an array containing the two arrays:
`[PersonList[], Adresses[]]`
this value can be passed through your rxjs pipe. 

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
