---
id: 870
title: Docker publish and deploy, cheat sheet
date: 2017-02-06T23:55:47+00:00
author: Marco Molteni
layout: post
main-class: 'cloud'
guid: http://javaee.ch/?p=870
permalink: /2017/02/06/docker-publish-and-deploy-cheat-sheet/
dsq_thread_id:
  - "5566114273"
image: /wp-content/uploads/2017/02/Docker_container_engine_logo-100x27.png
categories:
  - docker
  - tutorial
tags:
  - docker
---
In the post <a href="http://javaee.ch/2017/01/30/devops-for-the-poor-docker-ubuntu-apache-and-spring/" target="_blank">Cheap and easy DevOps for developers</a> I described the architecture of the deployment with the focus on the configuration of the proxy.
  
Here a cheatsheet of the operations that I execute to deploy the <a href="https://github.com/marco76/SpringAngular2TypeScript/blob/master/webClient/src/config/webpack.common.js" target="_blank">Angular & Java application</a> from the client to the server (<a href="http://angular.cafe" target="_blank">angular.cafe</a>).

On the **client**:
  
1. compile the application, from the parent module: `mvn clean install`
  
2. build the image, from the directory that contains the Docker configuration: `docker build -t javaee/angular2-java-hello-world .`
  
3. push the image to dockerhub: `docker push javaee/angular2-java-hello-world:latest`

On the **server** that hosts the application:
  
1. get the image: `docker pull javaee/angular2-java-hello-world`
  
2. stop the running image, angular is the name of the image: `docker stop angular`
  
3. remove the image: `docker rm angular`
  
4. run the new image: `docker run --name angular -d -p 8080:8082 javaee/angular2-java-hello-world`