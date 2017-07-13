---
id: 892
title: Send an image from the Java backend to Angular using REST services
date: 2017-02-11T13:23:50+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=892
permalink: /2017/02/11/send-and-image-from-the-java-backend-to-angular-using-rest-services/
dsq_thread_id:
  - "5565925771"
dsq_needs_sync:
  - "1"
categories:
  - Angular
  - Angular 2
  - Demo Application
  - java
  - javascript
  - JSON
  - REST
  - Spring
  - TypeScript
tags:
  - Angular
  - Base64
  - bytearray
  - image
  - java
  - REST
---
Problem: we want to show in our Angular 2 application images that should not be directly accessible by the browser. The images are provided by our Java backend.

Solution:

We can load the image in the backend and send the content to the client.

Backend:

    public @ResponseBody Map<String, String> getImage(@PathVariable String id) throws IOException {
            
       File file = new ClassPathResource(imagesPath + imageName).getFile();
        
        String encodeImage = Base64.getEncoder().withoutPadding().encodeToString(Files.readAllBytes(file.toPath()));
    
       Map<String, String> jsonMap = new HashMap<>();
    
       jsonMap.put("content", encodeImage);
    
       return jsonMap;
    }
    

We have to read the bytes of the image and encode the string in Base 64

`String encodeImage = Base64.getEncoder().withoutPadding().encodeToString(Files.readAllBytes(file.toPath()));`

Because we are working with REST services and JSON we have to transfer the content in a JSON format `{'name': 'content'}` we add the result in a Map structure:

    Map<String, String> jsonMap = new HashMap<>();
    jsonMap.put("content", encodeImage);
    

On the client side we can call the REST Service as usual, we get the JSON object in our custom JsonString object:

    getImage() : Observable<JsonString> {
            return this.http.get(this.serviceUrl)
                .map((response : Response) => {
                  return response.json();
           })}
    

The controller is more challenging:

We need to import the [sanitizer](https://angular.io/docs/ts/latest/api/platform-browser/index/DomSanitizer-class.html)

    import {__platform_browser_private__, DomSanitizer} from '@angular/platform-browser';
    

Sanitize the content received from the service adding the type of format received and assign it to the variable `image`:

    private sanitizer: DomSanitizer;
    private image : any;
    private readonly imageType : string = 'data:image/PNG;base64,';
    

    getImage(){
           this.imageService.getImage()
               .subscribe((data :JsonString ) => {
                   this.image = this.sanitizer.bypassSecurityTrustUrl(this.imageType + data.content);
    })}
    

In the template we can simply call the image content:

`<img [src]='image'>` 

Here the result with the JSON content from the server:

<img class="alignnone size-full wp-image-891" src="{{site.baseurl}}/assets/img/uploads/2017/02/angula_image_8.png?resize=900%2C702" data-recalc-dims="1" />