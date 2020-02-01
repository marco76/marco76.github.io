---
id: 559
title: 'The Spring Boot, AngularJS 2, TypeScript: Hello World Tutorial is now Java and Angular 5'
date: 2016-02-23T22:53:57+00:00
author: Marco Molteni
layout: post
guid: http://marco.dev/?p=559
permalink: /2016/02/23/spring-boot-angularjs-2-typescript-hello-world-tutorial/
dsq_thread_id:
  - "5565845282"
dsq_needs_sync:
  - "1"
main-class: 'spring'
color: '#7AAB13'
categories:
  - Angular
  - Angular 2
  - AngularJS
  - java
  - Java EE
  - REST
  - Spring
  - Spring Boot
  - TypeScript
tags:
  - Angular
  - REST
  - Spring Boot
  - TypeScript
---
_Update Aug. 07, 2017_

A lot changed from the first publication of the article (new versions of Angular).

I'm creating a new demo using Spring Boot, Angular (currently v. 5 beta) and Material Design.
The demo is more than a simple hello world and will collect the best practices for an enterprise development.

<img class="alignnone wp-image-902 size-full" src="{{site.baseurl}}/assets/img/uploads/2017/08/07/architecture.png" data-recalc-dims="1" />

The goals are the following:
- show the integration between Spring and Angular
- build a 'one-click' pipeline from the development to the deploy
- collect the best practices for the development in the enterprise

The project is more ambitious than a simple hello world and will require a bit of time to be developed in the free time.

You can find a first demo here (everything has been integrated in this demo):
~~[molteni.io](http://molteni.io)~~ [javademo.io](javademo.io)

The code is here:
[https://github.com/marco76/spriNGdemo](https://github.com/marco76/spriNGdemo)

~~There is a Jenkins job that build the project here:~~
~~[http://springdemo.io:8081/job/spring-demo-pipeline/](http://springdemo.io:8081/job/spring-demo-pipeline/)~~

~~A quality analysis with SonarQube:~~
~~[SonarQube](http://springdemo.io:9000/dashboard?id=spring-ng-demo%3Aparent%3Acandidate)~~

~~And a Docker deploy:~~
~~[molteni.io](http://molteni.io)~~

~~The [Java EE Demo](http://javademo.io) will be updated in the future.~~
~~The [springdemo.io](http://springdemo.io) won't be updated and will be replaced by the current molteni.io.~~

_Update Apr. 11, 2017_ _Spring Boot 2.0 Angular Demo *I update the **Spring** Demo. The new demo uses Spring Boot 2.0 (beta) and Angular 4. Website: <http://springdemo.io> GitHub: <https://github.com/marco76/spriNGdemo> The old Spring example is here : <http://angular.cafe> , [GitHub](https://github.com/marco76/SpringAngular2TypeScript) *Java EE Angular Demo_ Because Java has a rich and wonderful community there is a similar demo for **Java EE** You can find the Angular 4 and Java EE demo here: <http://javademo.io> Post: <http://marco.dev/2017/04/06/java-angular-demo/> GitHub: <https://github.com/marco76/java-demo>

Goal of the demo Java is a perfect technology for the modern development in few lines of code you can build rock solid backends. The demo is a showcase of how to integrate many features of Java EE and Spring in your next JavaScript application. The two demos

_are updated regularly_ and I will try to show the new features of Spring and Java EE.

#### Features

  * REST communication between Java Backend and TypeScript Frontend
  * Microservice oriented architecture, easy installation and deploy
  * The latest and greatest from Java EE and Spring
  * Webpack for an optimised deploy (size, packaging, compression)
  * Automatic refresh in the development phase
  * Docker deploy of the application (pull and run) Please leave a feedback here or on GitHub. Any sign from you is a stimulus to improve the demos.

  *   * *Here the old article. New articles will follow. Here some examples of the interface: 

[<img class="alignnone size-large wp-image-888" src="https://i1.wp.com/marco.dev/wp-content/uploads/2016/02/1.png?resize=945%2C472" alt="" data-recalc-dims="1" />]({{site.baseurl}}/assets/img/uploads/2016/02/1-e1486679732769.png)[<img class="alignnone size-full wp-image-889" src="https://i1.wp.com/marco.dev/wp-content/uploads/2016/02/2-e1486679774449.png?resize=400%2C242" alt="" data-recalc-dims="1" />](https://i1.wp.com/marco.dev/wp-content/uploads/2016/02/2-e1486679774449.png)

### **Deployment architecture**

#### Development

For the development there are 2 servers deployed: Webpack serves the Angular interface, Spring boot handles the backend requests. The Webpack server automatically redeploys the pages during your development. Commands:

    mvn clean package java -jar [PARENT_MODULE]/server/target/server-0.14-SNAPSHOT.war cd [PARENT_MODULE]/webClient/src npm start
    

go to http://localhost:8080

#### Production

In the production mode one Java WAR (Tomcat embedded) containing the javascript pages and the backend is created. This WAR can be deployed with a java -jar command. Thanks to WebPack the JavaScript code is optimized. Commands:

    mvn clean package java -jar [PARENT_MODULE]/server/target/server-0.14-SNAPSHOT.war
    

open your browser and visit http://localhost:8082

#### Hello World Example

In the example application (
  
<http://angular.cafe/app-hello-world>) there is a very simple example of communication between the frontend and the backend. [<img class="alignnone size-full wp-image-1038" src="https://i0.wp.com/marco.dev/wp-content/uploads/2016/02/2017-03-08_23-51-04.png?resize=469%2C202" alt="" data-recalc-dims="1" />](https://i0.wp.com/marco.dev/wp-content/uploads/2016/02/2017-03-08_23-51-04.png) The frontend calls the backend service and &#8216;subscribes&#8217; the answer. The frontend waits backend answer before to execute the code in the subscription part: Code on GitHub: <https://github.com/marco76/SpringAngular2TypeScript/tree/master/webClient/src/app/hello-world> **The Controller**

    export class HelloWorld {
        // string to publish on the screen
         helloWorldJava : string;
         constructor(helloWorldService : HelloWorldService){ 
        // subscribe to the service response, we are using Observable
        helloWorldService.getHelloWorldFromJava().subscribe((data : JsonString){ 
        // we receive a json object, we have to extract the string
         this.helloWorldJava = data.content;
        })}
    }
    

The **Service** called by the Controller: 

    // this url is declared in the HelloWorldController.java
      private helloUrl = this.ConstantsService.BACKEND\_URL + HELLO_WORLD_API;
    // return an observable and not a Promise
      getHelloWorldFromJava() : Observable<JsonString> {
          console.log('calling : ' + this.helloUrl);
         Observable.map((response: Response)<>
         { return response.json(); }) }
    

The backend called by the Service:

    @RequestMapping(value = "/rest/hello-world", method = RequestMethod.GET, produces = MediaType.APPLICATION_JSON_VALUE)
    Map<String, String; sayHello() { String helloWorld = helloWorldService.getHelloWorld();
    // we want to return a JSON object so we have to convert our String to a JSON key/value compatible format
    Map<String, String> jsonMap = new HashMap<>();
    // {"content" : "Hello World"}
    jsonMap.put("content", helloWorld);
    return jsonMap;
    } 
    

#### Other examples

On the example online you can find other examples (list, forms, &#8230;) and links to other articles. The code deployed is the code that you find in GitHub. The code is deployed using Docker in an AWS instance.

#### Very important

If you want to develop with this Stack (Angular with Java) you have to understand some critical points of the stack:

  * WebPack : it packages and manages the static resources of the frontend
  * Observable and Promises: the calls done by the frontend to the backend are managed by observables or promises.
  * Security : when you have an application that run in production every request has to be authenticated and (in theory) you should not use sessions.

#### Questions?

If you have questions, don&#8217;t hesitate 🙂 It&#8217;s funny to find solutions to common problems.  