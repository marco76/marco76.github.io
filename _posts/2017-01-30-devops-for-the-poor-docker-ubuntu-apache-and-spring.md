---
id: 820
title: Cheap and easy DevOps for developers (Docker, Ubuntu, Apache and Spring)
date: 2017-01-30T21:58:39+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=820
permalink: /2017/01/30/devops-for-the-poor-docker-ubuntu-apache-and-spring/
dsq_thread_id:
  - "5565702832"
categories:
  - Angular
  - docker
  - java
  - Spring
  - Uncategorized
tags:
  - Angular
  - devops
  - docker
  - java
  - spring mvc
---
Today the trend in every company is to use a DevOps architecture as a magical solution for everything. Translated for the developer not only he has to build the software but **deploy it as well**.

The standard solutions are platforms like OpenShift or CloudFoundry. These solutions are oriented and affordable for companies but way too expensive for most of the developers that want to publish their ready to grow application.

#### Our goal

Easily automatically deploy more applications on the web (www.mydomain.com, www.myseconddomain.com) using a &#8216;cheap&#8217; (cheaper than the DevOps platforms) but powerful server.
  
To give an idea about the prices a small app running on a DevOps cloud solution (1 GB RAM) cost around $30 per month, with a basic VPS and docker only $3-5 (2GB RAM for more apps!).

<img class="alignnone size-full wp-image-819" src="https://i0.wp.com/javaee.ch/wp-content/uploads/2017/01/blog_vps.png?resize=600%2C531" data-recalc-dims="1" />

#### What we need

  * vps hosting of your choice
  * ubuntu or other linux distribution
  * docker
  * apache
  * your software

#### VPS and Docker

Install Docker on your server using the official Docker documentation or the instructions of your server provider. You can pull your own image or an image from dockerhub.com. For instance my Angular and Java demo

`docker pull javaee/angular2-java-hello-world`

to run the app on port 8080

`docker run --name angular -d -p 8080:8082`

😀

If you prefer you can deploy directly your web app on the server but you risk to miss most of the fun. 😉

#### Create a VirtualHost

You will run multiple docker containers in server, for this reason is better to have a proxy that redirect the external requests to the correct applications.

Example:

www.angular.cafe -> localhost:8090
  
www.mycv.host -> localhost:8088

In ubuntu you have to install apache2

    sudo apt-get install apache2
    sudo apt-get install libapache-mod-jk
    

activate the features to use the proxy:

    sudo a2enmod proxy
    sudo a2enmod proxy_http
    sudo a2enmod ssl
    sudo a2enmod proxy_balancer
    

Create a file with your host configuration in the following path:

`/etc/apache2/sites-available`

the file name suffix has to be .conf e.g.

`sudo vi ./mydomain.conf`

#### Apache Virtual Host configuration for Tomcat

    # *.80 listen all the requests at the port 80
    <VirtualHost *:80>
    
    # the following rules apply for the requests 
    # to the www.mycv.host server
    ServerName www.mycv.host
    
    # aliases of our server name (answer to this calls too)
    ServerAlias *.mycv.host *.mycv.help
    
    # email of the administrator
    ServerAdmin webmaster@localhost
    
    #
    ProxyPreserveHost On
    
    # where to redirect the request to mycv.host
    ProxyPass / http://localhost:8080/
    ProxyPassReverse / http://localhost:8080/
    
    # root of the target (doesn't apply for tomcat)
    DocumentRoot /var/www
    </VirtualHost>
    

To activate this rule:

`sudo a2ensite mydomain.conf` _
  
_ 

Deactivate the default answering for the port 80 (apache welcome) :

`sudo a2dissite 000-default-conf` _
  
_ 

Restart your apache service

`sudo /etc/init.d/apache2 restart`

You can check the status of your service:

`systemctl status apache2.service`

Your application running on port 8080 (if Java based) is accessible at the url: mydomain.com, port 80.

If your application doesn&#8217;t answer from internet it is possible that the port 80 of your server is not closed. You can test in your server to see if the application is correctly deployed with:

`curl localhost`

If your terminal shows the content of your application home page it mean that the deploy is correct.

Enjoy!

Continue &#8230; <a href="http://javaee.ch/2017/02/06/docker-publish-and-deploy-cheat-sheet/" target="_blank">Docker publish and deploy, cheat sheet</a>