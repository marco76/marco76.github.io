---
title: 'How to develop a mobile app if you are an Angular/Java fullstack developer'
description: 'Which technology for you next mobile App?'
date: 2018-03-21T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'mobile'
color: '#7AAB13'
permalink: /2018/03/26/angular-java-mobile-framework/
redirect_to:
- https://www.ngjava.com/angular-java-mobile-framework/
categories:
  - Angular
  - TypeScript
  - Flutter
  - Android
  - IOS
  - NativeScript
  - Firebase
tags:
  - angular
  - mobile
 
image: '/assets/img/'

introduction: 'Which technology for you next mobile App?'
---

# You are a fullstack and you have to develop a mobile App ...

A quick note, wrote during a commute about a recent mobile dev experience.

## Project

For few weeks I supported some friends with their project of startup. They needed a prototype for a mobile application.

The app was a marketplace. DB + heavy use of Maps.
 
This gave me the opportunity to evaluate which technologies better suit somebody who comes from a Java background.

 
## Cloud

One of the requirements was that the platform has to use the Cloud. For the VC (Venture Capital) the word cloud is very important.
Technically my idea was to:
 - deploy a Java backend in a Docker image
 - the web frontend could use a simple storage (S3)
 - for the DB docker image or use a DB of the cloud provider

For the prototype we wanted to reduce the expenses at the minimum so the instances with less ressources were preferred.

#### AWS
I already knew AWS. It's not easy but you are not wrong choosing it. Like IBM or Oracle in the past.

#### Azure
For the estimation of the costs there is a page with a calculator and dozens of option and every option was extremly expensive.

I found the experience with Azure quite complicated. It seems oriented to big organizations were the sales have to confuse the client with millions of options to go deep in their waller.

So Azure was out of competition.

#### Google Cloud Engine
With Google we received a generous 3 months trial and the deploy of Docker images, network configuration etc. went smoothly.
We opted to use Cloud DB (MySql). The prices were clear.

Big plus:
- integration with Firebase
- Kubernetes

### Cloud, the winner is ... Google Cloud
AWS and GCE were excellent options for us. Google Cloud seemed to us more 'architecturally structured' and less 'we invented the cloud and we are stacking features over and over'.
And again the firebase integration was pefect with Google Cloud.

## Firebase ... a winner

The offer for mobile (and web development) is excellent. After the discovery of Firebase it became one of the fundamental components of the architecture.

## Mobile Dev: ouch!

The choice for the mobile Dev was awful. We decided to target Android and IOS (no surprise) and according to the market share Android-first.

Having done some development in Android and knowing a litte Swift I excluded the pure native development.
Develop a prototype in the free time with two different framework, languages, philosophies was simply not possible.

Alternatives:

#### React Native
This option is well known because of Tesla, AirBNB, Facebook etc.

We didn't like to use Javascript as main language and the 'React is only a library' concept. Basically we had to build all the framework.

Full respect for RN but build the framework and implements the differences of IOS and Android was not possible (time-limit).

My clients work with Java and Angular. It took months to eliminate Javascript I'm not really motivated to use it during my free time.

#### Flutter
Flutter came out with a Beta during our evaluation. Cool, super fast. It's all Widget oriented.
The differences between IOS and Android are managed by the framework.

Dart is compatible with a Java/TS guy!

Problems: 
- where is Google Maps? Ouch ... [not there](https://github.com/flutter/flutter/issues/73). The GFlutter team has to talk to the GMaps team and find a solution
- we still need to add new technologies to our development (Flutter and Dart)
- only one public success story ... welcome beta testers ;)
- 'Flutter doesn't stand a chance against progressive webapps' ([according to the tech lead for internal mobile app at Google](https://www.quora.com/Is-Flutter-likely-to-replace-Java-for-Android-app-development))

#### NativeScript

Cool! Angular, Native and no need to work directly with IOS or Android and the NS team is cool.

For our first prototype we used NativeScript.

- The startup time was not good (5-10 seconds), sure was possible to optimize but was not a good start
- Many error in the log were not easily understandable (thanks Javascript)
- The rendering was different in IOS and Android
- It happen that the application didn't start and we didn't succeed to find the reason ('did you try to switch off and switch on again?')
- We didn't find relevant success stories and we didn't find any NS user in our country (Switzerland). NativeScript is on the market since a few years this was not a good sign.

What really scared us was the announce of the support to Vue.js.
 
NS is cool but there are a lot of problems. In GitHub many devs seem to share the opinion that NS Angular is not ready for production.
The integration of a new framework (definitely lighter than Angular) comes with a cost.
The Telerik (NS) commercial PRO components become free and open source. Nice but why? Usually if a commercial product become free is a sign of 'EOL' :|

In conclusion, NS was cool but there were to many fears toward the technology future.

#### Ionic

Ionic is well known. Was not our first choice because of his 'web' nature and his 'slow tech' reputation.

We built our second prototype with Ionic. The dev went smooth, without big issues with performances.

What we didn't like:
- [the blog post of Ionic 'what's the issue with the issues'](https://blog.ionicframework.com/whats-the-issue-with-issues/), basically the support for the current version is minimal
- the fact that Ionic seems to abandon his Angular nature for a new custom framework

Ionic was cool for the demo but I don't feel it as a long term technology.
The company want to become 'THE PWA company' and it is not fixing Ionic 3.

Maybe going directly full PWA is more promising ;)

#### Progressive Webapps
... we are waiting IOS ... could be the future if ... in the infinite loop somebody decides for it.

### Mobile, the winner is ... nobody

There was no magic bullet for the mobile development.
 
We found too many limits in the 'Javascript Native' approach of React and NativeScript.
The integration of Angular with such solutions (Ionic and NS) seems not successful. Angular excels for big business webapps, maybe it is simply not well suited to run in a small device on top of Javascript.  

 
The summary is something like:
- fast prototyping : Ionic + Angular
- fast native and feature limited development : Flutter + Dart
- full feature: Android + Java + Swift + IOS + luck and time
- the future (finger crossed): PWA with Angular




