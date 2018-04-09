---
title: 'How to deploy an Angular app in Google Cloud'
description: 'Easily deploy your app'
date: 2018-04-07T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: /2018/04/08/deploy-angular-google-cloud/
categories:
  - Angular
  - TypeScript
  - Google Cloud
tags:
  - angular
  - cloud
 
image: '/assets/img/'

introduction: 'How to deploy your app in Google Cloud?'
---

# How to deploy your Angular app in the Google Cloud Storage?

You created a beautiful Angular (or React or Html or ...) App and you need to deploy it on the web.

You decided to use a Cloud service because the app has to be scalable to millions of daily users.

## Create your bucket 

You can follow the official guide:
(https://cloud.google.com/storage/docs/hosting-static-website)

Remember to set the [index page](https://cloud.google.com/storage/docs/hosting-static-website#index-page), for an Angular website *you have to fill the [error page](https://cloud.google.com/storage/docs/hosting-static-website#error-page) with your index page* (the framework manages the urls).

## Build your Angular app
No surprise here:

```console
ng build -prod
```

## Prepare your bucket

When you copy your files in the Google storage your files are not accessible to anonymous users.

You can change this changing the default authorization assigned to the files copied in the storage:

```console
gsutil defacl ch -u AllUsers:R gs://www.[bucket]
````
This instruction doesn't modify the rights on the existing files.

## Copy your files

To copy your angular application:

```console
gsutil -m cp ./dist/* gs://www.[bucket]
```

## Update your registry

[As documented](https://cloud.google.com/storage/docs/hosting-static-website#cname) you should update your registry CNAME.

With this operation your website will work with the prefix www (e.g. www.mysite.com) but it won't work without (e.g. mysite.com).

To solve the issue you have to *create an URL redirect* (config vary for each provider) from @ to http://www.mysite.com.





