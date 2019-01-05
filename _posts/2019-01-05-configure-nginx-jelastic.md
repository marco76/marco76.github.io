---
title: 'Configure nginx in Jelastic'
description: ''
date: 2019-01-05T01:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'cloud'
color: '#7AAB13'
permalink: /nginx-jelastic
categories:
  - Java, Cloud
tags:
  - Java, Cloud
 
image: '/assets/img/'

introduction: 'Use compression with nginx'
---

# Activate nginx compression in Jelastic

Select the config icon in the Load Balancer service/node.

[<img src="{{site.baseurl}}/assets/img/uploads/2019/nginx_1.jpg" alt=""/>]({{site.baseurl}}/assets/img/uploads/2019/nginx_1.jpg)

[<img src="{{site.baseurl}}/assets/img/uploads/2019/nginx_2.jpg" alt=""/>]({{site.baseurl}}/assets/img/uploads/2019/nginx_2.jpg)



Open the file `/etc/nginx/nginx-jelastic.conf`.

Activate the compression `gzip on;`

Add the following types to the compression:
```
gzip_types
      text/plain
      text/css
      text/html
      text/javascript
      application/javascript
      application/x-javascript
      application/xml
      application/json
      application/ld+json;
```

Restart the nginx node;

## Redirect all the HTTP requests to HTTPS

in nginx-jelastic.conf search for the following text:
```
#GFADMIN

        server {
                listen *:80;
                listen [::]:80;
                server_name  _;
```

and add the line 
`return 301 https://$host$request_uri;`

Restart the nginx nodes