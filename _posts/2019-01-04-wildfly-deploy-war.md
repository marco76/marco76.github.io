---
title: 'Deploy a WAR as root in WildFly'
description: ''
date: 2019-01-04T04:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'javaee'
color: '#7AAB13'
permalink: /use-wildfly-deploy-root
categories:
  - JavaEE, WildFly
tags:
  - JavaEE, WildFly
 
image: '/assets/img/'

introduction: 'Deploy to /'
---

## using maven / manual deploy
Save your war with the name ROOT.war and deploy it, WildFly will configure it as root application.

## using configuration files
In your `[MAVEN_ROOT]/webapp/WEB-INF` create `jboss-web.xml` and add:

```xml
<jboss-web>
    <context-root>/</context-root>
</jboss-web>
```

## serve index.html
To serve your main page update web.xml:
```xml
<welcome-file-list>
    <welcome-file>index.html</welcome-file>
</welcome-file-list>
```
