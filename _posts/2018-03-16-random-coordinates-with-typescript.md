---
title: 'Random coordinates for Google Maps with Typescript'
description: 'Utility for the development of Angular apps'
date: 2018-03-16T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: /2018/03/16/google-maps-typescript/
categories:
  - Angular
  - TypeScript
  - NPM
  - Google Maps
  - Ionic 
tags:
  - angular
  - typescript
 
image: '/assets/img/'

introduction: 'Generate random coordinates for Google Maps with your Ionic/TypeScript application'
---
# Random GPS Coordinates for Google Maps in TypeScript

## What's for?

I'm developing a small Mobile Application using Ionic and Angular. The app uses a Map to show to the user some nearby services.

For the test phase, as you can imagine, we don't have real service to show and the user/demo can be anywhere.

To help with the test I created a simple generator of GPS coordinates based on the location of the user.

[Here the package NPM](https://www.npmjs.com/package/@molteni/coordinate-utils) and the code source [GitHub](https://github.com/marco76/CoordinateUtils)

<img src="{{site.baseurl}}/assets/img/uploads/2018/03/map_1.jpg" />

The coordinates of the 3 cook's hats are generated from the library. The green point is the current position.

You can test the app yourself (alpha on google play store):

[https://play.google.com/apps/testing/com.wicooks.next](https://play.google.com/apps/testing/com.wicooks.next)

## How to use it

The library is still a work in progress but already working.

You simply need the starting coordinates (latitude/longitude) and pass the maximum number of kilometers of distance for the new coordinates.

The coordinates are created using this function:
```typescript
import {LatLng as LatitudeLongitude} from "@molteni/coordinate-utils/dist/LatLng";

 generateCoordinates() : LatitudeLongitude {
    return RandomCoordinateUtils.randomCoordinateFromPositionWithExplicitLatLng(this.currentLocation.lat, this.currentLocation.lng, 0.1);
  };
```

To the current location are added/subtracted 0 to 100 meters to the longitude and latitude.

## How it works

The calculation is not extremely accurate but is enough for the Use Cases of most developers.

``` typescript

const EARTH_RADIUS = 6378;

    export function addKMToLatitude(latitude : number, kilometers : number) : number {
        return latitude + (kilometers / EARTH_RADIUS) * (180 / Math.PI);
    }

    export function addKMToLongitude(latitude : number, longitude : number, kilometers : number) : number {
        return longitude + (kilometers / EARTH_RADIUS) * (180 / Math.PI) / Math.cos(latitude * Math.PI / 180);
    }

    export function  getKMperDegree() {
        return (2*Math.PI / 360) * EARTH_RADIUS;
    }
}
```