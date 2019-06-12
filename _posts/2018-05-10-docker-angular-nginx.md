---
title: 'Deploy your Angular app with Docker and Nginx'
description: ''
date: 2018-05-10T10:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'angular'
color: '#7AAB13'
permalink: /2018/10/10/docker-angular-nginx/
categories:
  - docker
  - Angular
tags:
  - angular
  - docker
 
image: '/assets/img/'

introduction: 'Deploy your Angular app with Docker and Nginx'
---

# Deploy your app in a container

For our deployments we use Kubernetes or OpenShift. Everything has to be in a container and be part of a develop / build / deploy pipeline.

In a previous post I already explained how to deploy an Angular app in a Java artifact using a maven structure.

When we want to completely separate the frontend from the backend we have to create multiple containers.

In this example we create a Docker container that contains an Angular application (http://javademo.io).

## The docker container

`Dockerfile`

``` docker
FROM nginx
COPY nginx.conf /etc/nginx/nginx.conf
COPY ./src/dist /usr/share/nginx/html
```

We use Nginx as web server for the deployment. In theory an Angular app is a simple Javascript application and it doesn't need a web server. An online file storage offered by a cloud provider could be enough.

Some advantages of a web server:
* own rules and filters
* compression
* resolution of the html urls (404 error after refresh)

Our docker consists in only 3 lines.

`FROM nginx` we use the nginx Docker image
`COPY nginx.conf /etc/nginx/nginx.conf` we copy our own nginx configuration.
`COPY ./src/dist /usr/share/nginx/html` we copy the prod build of our Angular app.

## Nginx configuration

We have a custom configuration for nginx `nginx.conf`:

``` bash
events {
  worker_connections  1024;  ## Default: 1024
}

http {

    ## use mime types
    include /etc/nginx/mime.types;

     server {
       
        listen 80;

        location / {
            root /usr/share/nginx/html;
            index  index.html;
            ## without this our .css are not loaded
            try_files $uri $uri/ /index.html?$query_string;
        }
    }

    ## enable gzip compression
    gzip on;
    gzip_vary on;
    gzip_min_length 256;
    gzip_proxied any;

    gzip_types
      ## text/html is always compressed : https://nginx.org/en/docs/http/ngx_http_gzip_module.html
      text/plain
      text/css
      text/javascript
      application/javascript
      application/x-javascript
      application/xml
      application/json
      application/ld+json;
}
```

This configuration creates a web server that listen port 80, makes a compressions of the response and redirect to the main page if an URL is not found.