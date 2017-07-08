---
id: 112
title: JSF 2 converters and Spring services
date: 2010-09-29T09:13:47+00:00
author: Marco Molteni
layout: post
guid: http://molteni.wordpress.com/?p=112
permalink: /2010/09/29/jsf-2-converters-and-spring-services/
jabber_published:
  - "1285748027"
categories:
  - Javaserver Faces
  - JSF
  - Spring
---
Problem:
  
I&#8217;ve a JavaServer Faces 2 converter (@FacesConverter) and I&#8217;ve to access a Spring service. The converter is managed by jsf and if I use @Service or a Spring bean I&#8217;ve a null pointer exception

Solution:
  
access the Spring service using FacesContextUtils. Ex:

`<br />
 @Override<br />
    public Object getAsObject(FacesContext facesContext, UIComponent uiComponent, String s) {<br />
  ...<br />
      if (personneService == null)<br />
          personneService = (PersonneService) FacesContextUtils.getWebApplicationContext(facesContext).getBean("personneService");<br />
   ...<br />
   }<br />
`