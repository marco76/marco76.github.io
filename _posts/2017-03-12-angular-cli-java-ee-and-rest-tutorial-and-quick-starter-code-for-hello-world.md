---
id: 1042
title: 'Angular CLI, Java EE and REST : tutorial and quick starter code for Hello World'
date: 2017-03-12T17:25:30+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=1042
permalink: /2017/03/12/angular-cli-java-ee-and-rest-tutorial-and-quick-starter-code-for-hello-world/
image: /wp-content/uploads/2017/03/logo_ang-100x39.png
categories:
  - Angular
  - Angular 2
  - java
  - Java EE
  - REST
  - TypeScript
tags:
  - Angular
  - angular cli
  - hello world
  - java ee
  - tutorial
---
##### Complete Java EE and Angular example

_New demo available here_: <http://javademo.io> &#8211; <https://github.com/marco76/java-demo> &#8211; <http://javaee.ch/2017/04/06/java-angular-demo/>

##### Problem we need a new web application that uses Java EE as backend and Angular as frontend. It should be easy to configure.

##### Solution you can find the code of the application here:

<https://github.com/marco76/javaee-angular-starter> The scope of the application is limited (1 angular route, 1 http service, only a JSON ressource), this allow you to start with a clean application. I will build another app to shows some other features.

#### Features

  * frontend built and compatible with [Angular CLI](https://cli.angular.io)
  * configured to use the development and production mode of WebPack
  * ready to use out of the box
  * build a WAR file compatible with Java EE servers (JBoss, Payara/Glassfish etc.)
  * it comes with all the nice features of Angular CLI: tests, good practices etc.
  * automatic deploy during the development

### How to install

##### Prerequisites -you need maven, npm, git and an application server Java EE

##### Production mode

  1. clone the git project
  2. from the root of the project launch `mvn package` this generates a package named _ROOT.war_ in the _PROJECT/server/target_ directory
  3. you can deploy this package in your favourite application server. The Angular application should answer at the requests to _http://localhost:8080_

##### Development mode

  1. clone the git project
  2. You can start the server using your favourite IDE. The project uses a standard Maven directory structure. You need to configure the server to deploy the _server.war_ artifact.
  3. from the _PROJECT/client/src_ directory install the npm packages : `npm install`
  4. launch the client with `ng serve`. The client uses the port 4200 (default for Angular CLI) and you can navigate to _http://localhost:4200_

### The result

 <img class="alignnone size-full wp-image-1041" src="https://i2.wp.com/javaee.ch/wp-content/uploads/2017/03/payara_easy.png?resize=700%2C281" data-recalc-dims="1" />The result is not particularly fancy. It show the result of a call to a Java REST service &#8230; but hey, you have a ready to go configuration based on Java EE and Angular that follows the Angular best practices.

#### How it works As for the previous

[Angular/Spring project](http://javaee.ch/2016/02/23/spring-boot-angularjs-2-typescript-hello-world-tutorial/) we have a main maven project with 2 modules :

    <modules>
        <module>client</module>
        <module>server</module>
    </modules>
    

Client: contains the Angular project generated using Angular CLI (= top quality) Server: contains the Java EE project (very minimalistic, 1 REST service and 1 filter to avoid the CORS restrictions during the development). When we build the final package the angular project is build and the result saved in a directory that will be integrated in the Java EE build. During the development the two separate projects are deployed. For the production only 1 deployable war is generated. Here the details of the maven build phase:

    <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
        <version>1.5.0</version>
        <executions>
            <execution>
                <id>install npm</id>
                <configuration>
                    <workingDirectory>./src/</workingDirectory>
                    <executable>npm</executable>
                    <arguments>
                        <argument>install</argument>
                    </arguments>
                </configuration>
                <phase>generate-resources</phase>
                <goals>
                    <goal>exec</goal>
                </goals>
            </execution>
            <execution>
                <id>build js</id>
                <configuration>
                    <workingDirectory>./src</workingDirectory>
                    <executable>ng</executable>
                    <arguments>
                        <argument>build</argument>
                        <argument>--target=production</argument>
                        <argument>--environment=prod</argument>
                    </arguments>
                </configuration>
                <phase>generate-resources</phase>
                <goals>
                    <goal>exec</goal>
                </goals>
            </execution>
        </executions>
    </plugin>