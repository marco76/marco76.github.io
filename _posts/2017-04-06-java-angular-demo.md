---
id: 1082
title: Java EE and Angular 4 Demo
date: 2017-04-06T13:38:58+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=1082
permalink: /2017/04/06/java-angular-demo/
image: /wp-content/uploads/2017/03/logo_ang-100x39.png
categories:
  - Angular
  - Angular 2
  - Angular 4
  - AngularJS
  - docker
  - Java EE
  - REST
tags:
  - angular 4
  - AngularJS 2
  - java and angualar 4 demo
  - java ee
  - rest services
---
### Java EE and Angular Demo

Web demo: <http://javademo.io> GitHub : <https://github.com/marco76/java-demo>

Java EE 8 will come this year with a lot of newsworthy improvements and some possible disappointments (MVC, JAX-RS NIO etc).

With the goal to show how the new JSR works we are preparing a demo of the new features and some already existing but maybe not well known.

The final specification should be ready in **July 2017** and the compatible containers will follow.

Inspired by some great active members of the Java Community we try to build a demo that allows the developers to see and play with some of the new JSR. The road is not easy because the specifications of the JSR are not even completed on paper. It will be a long and funny journey.

#### Java EE on the backend and &#8230; Angular on the frontend???

Yes, Angular. It&#8217;s not a Java technology and it&#8217;s in direct competition with some Java frameworks during the evaluation process of enterprises architectures. So why the choice of a JavaScript framework and not JSF?

One of the goal of the demo is to show how Java EE is an excellent and solid solution to the present and future enterprise needs. Companies are free to choice the new coolest and riskier JavaScript frameworks for the frontend and feel safe on the backend with a rock solid technology.

In my personal experience as consultant when I go to talk to clients they tell me &#8216;_for the new web projects we use the newest JS frameworks (Angular, React etc.) that communicate through RESTful API with a Java (Spring or Java EE) backend. The JSF projects are in maintenance (deprecated) mode. Same architecture for the mobile: native frontend and REST_&#8216;.

This seems to be the current trend and we have to accept it, maybe your experience is different.

If you are interested in a full stack Java EE 8 demo you can find an excellent reference here: <http://jj-blogger.blogspot.ch/2017/03/testing-java-ee-8.html?m=1>

#### The project with Java EE 8 &#8211; javademo.io

The demo (in permanent development) is visible here: <http://javademo.io>

The JSR are still in work in progress mode (review, final draft) and the Reference Implementations are not ready yet. The server used to run the application need some patches to integrate the new JSRs.

The code source is available here: <https://github.com/marco76/java-demo>

#### The architecture of the application

Server: WildFly 11 alpha (patched)

Frontend : Angular 4

Build: Maven and Webpack (Angular cli)

Deploy: Docker in AWS cloud or simple Web WAR in a Java EE (patched) server

The application is built combining Maven with Node.js. For the production mode maven build the deployable with an orchestration of the Java and Javascript build processes.

For the development the Java backend run on 8080 and Angular (ng serve) run on 4200.

#### Want to see some special features? Want to contribute?

Every contribution is welcomed. You can ask for features that you want to see in the blog, better in the GitHub project. Suggest for improvements. Submit PR for the Java or the Angular / Typescript code.

* * *

_Update_
  
Thanks to **André** and to the **Java and Angular Developers**

André twitted about this post and generated a strong reaction in the Java and Angular Community, thanks. This give me more energy to improve the demo during the next months (the Java EE 8 spec and implementations are still a &#8216;work in progress&#8217;, Angular improve/change regularly).

The goal of the demo (free time work) is to try to help the FullStack developers with a simple reference and a catalog of feature for their daily work.

You can help reviewing the code on [GitHub](https://github.com/marco76/java-demo)  or request for new features / improvements. Every feedback helps.

<blockquote class="twitter-tweet" data-width="500">
  <p lang="pt" dir="ltr">
    JavaEE 8 and Angular 4 Demo<a href="https://twitter.com/hashtag/JavaEE?src=hash">#JavaEE</a> <a href="https://twitter.com/hashtag/JavaEE8?src=hash">#JavaEE8</a> <a href="https://twitter.com/hashtag/angular4?src=hash">#angular4</a> <a href="https://twitter.com/hashtag/Java?src=hash">#Java</a> <a href="https://twitter.com/Java_EE">@Java_EE</a> <a href="https://twitter.com/hashtag/Angular?src=hash">#Angular</a> <a href="https://t.co/eu3h3DGejs">https://t.co/eu3h3DGejs</a> <a href="https://t.co/9y2yY01uGD">https://t.co/9y2yY01uGD</a>
  </p>
  
  <p>
    &mdash; André Sept (@andre_sept) <a href="https://twitter.com/andre_sept/status/852582411762270210">April 13, 2017</a>
  </p>
</blockquote>