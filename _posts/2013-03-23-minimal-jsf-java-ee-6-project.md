---
id: 238
title: Minimalistic JSF Java EE 6 tutorial using Maven/Eclipse/TomEE
date: 2013-03-23T22:32:20+00:00
author: Marco Molteni
layout: post
guid: http://marco.dev/?p=238
permalink: /2013/03/23/minimal-jsf-java-ee-6-project/
original_post_id:
  - "238"
categories:
  - java
  - Java EE
  - Javaserver Faces
  - JSF
  - Maven
  - TomEE
tags:
  - eclipse
  - java ee
  - jsf
  - maven
  - tomee
---
The goal of this tutorial is to build a basic Java EE hello world application. We will use eclipse, maven and tomee.

<img title="ecl1.png" alt="Untitled" src="{{site.baseurl}}/assets/img/uploads/2013/03/eclipseecl1.png?resize=525%2C233" border="0" data-recalc-dims="1" />

Select &#8216;Create a simple project (skip archetype selection)&#8217; our project is very simple 🙂

<img title="ecl2.png" alt="2" src="https://i0.wp.com/marco.dev/wp-content/uploads/2013/03/eclipseecl2.png?resize=600%2C148" border="0" data-recalc-dims="1" />

Fill the project informations, you have to change the packaging from jar to war:

<img title="3.png" alt="3" src="https://i1.wp.com/marco.dev/wp-content/uploads/2013/03/eclipse31.png?resize=600%2C279" border="0" data-recalc-dims="1" />

The the project has been created, you should have the following folder structure:

<img title="ecl_4.png" alt="Ecl 4" src="https://i1.wp.com/marco.dev/wp-content/uploads/2013/03/eclipseecl_4.png?resize=124%2C138" border="0" data-recalc-dims="1" />

The pom.xml is quite empty &#8230;

<pre class="brush: xml; title: ; notranslate" title="">&lt;modelVersion&gt;4.0.0&lt;/modelVersion&gt;
&lt;groupId&gt;ch.javaee&lt;/groupId&gt;
&lt;artifactId&gt;helloWorldTutorial&lt;/artifactId&gt;
&lt;version&gt;0.0.1-SNAPSHOT&lt;/version&gt;
&lt;packaging&gt;war&lt;/packaging&gt;
&lt;name&gt;Hello world tutorial&lt;/name&gt;
</pre>

… we have to add the dependency on JSF and add the maven plugin to compile the sources and produce a war file to deploy …

<pre class="brush: xml; title: ; notranslate" title="">&lt;project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"&gt;
&lt;modelVersion&gt;4.0.0&lt;/modelVersion&gt;
&lt;groupId&gt;ch.javaee&lt;/groupId&gt;
&lt;artifactId&gt;helloWorldTutorial&lt;/artifactId&gt;
&lt;version&gt;0.0.1-SNAPSHOT&lt;/version&gt;
&lt;packaging&gt;war&lt;/packaging&gt;
&lt;name&gt;Hello world tutorial&lt;/name&gt;
&lt;dependencies&gt;
&lt;dependency&gt;
&lt;groupId&gt;com.sun.faces&lt;/groupId&gt;
&lt;artifactId&gt;jsf-api&lt;/artifactId&gt;
&lt;version&gt;2.1.20&lt;/version&gt;
&lt;scope&gt;provided&lt;/scope&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
&lt;groupId&gt;com.sun.faces&lt;/groupId&gt;
&lt;artifactId&gt;jsf-impl&lt;/artifactId&gt;
&lt;version&gt;2.1.20&lt;/version&gt;
&lt;scope&gt;provided&lt;/scope&gt;
&lt;/dependency&gt;
&lt;/dependencies&gt;
&lt;build&gt;
&lt;finalName&gt;HelloWorldTutorial&lt;/finalName&gt;
&lt;plugins&gt;
&lt;plugin&gt;
&lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
&lt;artifactId&gt;maven-compiler-plugin&lt;/artifactId&gt;
&lt;version&gt;3.0&lt;/version&gt;
&lt;configuration&gt;
&lt;source&gt;1.6&lt;/source&gt;
&lt;target&gt;1.6&lt;/target&gt;
&lt;/configuration&gt;
&lt;/plugin&gt;
&lt;plugin&gt;
&lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
&lt;artifactId&gt;maven-war-plugin&lt;/artifactId&gt;
&lt;version&gt;2.1.1&lt;/version&gt;
&lt;/plugin&gt;
&lt;/plugins&gt;
&lt;/build&gt;

&lt;/project&gt;

</pre>

In the web app folder we have to create a web.xml file

<img title="webxml.png" alt="Webxml" src="https://i1.wp.com/marco.dev/wp-content/uploads/2013/03/eclipsewebxml.png?resize=132%2C108" border="0" data-recalc-dims="1" />

<pre class="brush: xml; title: ; notranslate" title="">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"&gt;
&lt;display-name&gt;HelloWorldTutorial&lt;/display-name&gt;
&lt;context-param&gt;
&lt;param-name&gt;facelets.REFRESH_PERIOD&lt;/param-name&gt;
&lt;param-value&gt;2&lt;/param-value&gt;
&lt;/context-param&gt;
&lt;context-param&gt;
&lt;param-name&gt;javax.faces.DEFAULT_SUFFIX&lt;/param-name&gt;
&lt;param-value&gt;.xhtml&lt;/param-value&gt;
&lt;/context-param&gt;
&lt;servlet&gt;
&lt;servlet-name&gt;Faces Servlet&lt;/servlet-name&gt;
&lt;servlet-class&gt;javax.faces.webapp.FacesServlet&lt;/servlet-class&gt;
&lt;load-on-startup&gt;1&lt;/load-on-startup&gt;
&lt;/servlet&gt;
&lt;servlet-mapping&gt;
&lt;servlet-name&gt;Faces Servlet&lt;/servlet-name&gt;
&lt;url-pattern&gt;/faces/*&lt;/url-pattern&gt;
&lt;/servlet-mapping&gt;
&lt;/web-app&gt;
</pre>

We can now create the controller class

<img title="index.png" alt="Index" src="https://i0.wp.com/marco.dev/wp-content/uploads/2013/03/eclipseindex.png?resize=522%2C267" border="0" data-recalc-dims="1" />

<pre class="brush: java; title: ; notranslate" title="">package ch.javaee.helloWorldTutorial.backing;

import javax.faces.bean.ManagedBean;
import javax.faces.bean.RequestScoped;

@ManagedBean
@RequestScoped
public class IndexBean {

private String world = &quot;world&quot;;

public String getWorld(){

return world;

}

}
</pre>

You can see your new class in the project explorer

<img title="controller.png" alt="Controller" src="https://i0.wp.com/marco.dev/wp-content/uploads/2013/03/eclipsecontroller.png?resize=262%2C105" border="0" data-recalc-dims="1" />

We have now to create the facelet page that shows the hello world message:

<img title="htmlx.png" alt="Htmlx" src="{{site.baseurl}}/assets/img/uploads/2013/03/eclipsehtmlx.png?resize=144%2C89" border="0" data-recalc-dims="1" />

<pre class="brush: xml; title: ; notranslate" title="">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;
&lt;html xmlns="http://www.w3.org/1999/xhtml"
xmlns:h="http://java.sun.com/jsf/html"
xmlns:ui="http://java.sun.com/jsf/facelets"&gt;

&lt;h:head&gt;&lt;/h:head&gt;

&lt;h:body&gt;

Hello &lt;h:outputText value="#{indexBean.world}" /&gt;

&lt;/h:body&gt;

&lt;/html&gt;</pre>

The code simply writes &#8216;Hello&#8217; and goes to look for the variable world in the indexBean.

<img title="maven.png" alt="Maven" src="{{site.baseurl}}/assets/img/uploads/2013/03/eclipsemaven.png?resize=600%2C248" border="0" data-recalc-dims="1" />

In Eclipse we can create a maven configuration to execute the goals &#8216;compile&#8217; et &#8216;war:war&#8217;. This configuration creates the package to be loaded by the application server.

<img title="artifacts.png" alt="Artifacts" src="https://i1.wp.com/marco.dev/wp-content/uploads/2013/03/eclipseartifacts.png?resize=600%2C157" border="0" data-recalc-dims="1" />

As you can see the application file &#8216;HelloWorldTutorial.war&#8217; has a size of only 4KB. All the components used by the Java EE application are already in the server library.

We are using TomEE, to deploy the application we can copy the .war file in the web apps directory.

<img title="tomee.png" alt="Tomee" src="{{site.baseurl}}/assets/img/uploads/2013/03/eclipsetomee.png?resize=245%2C160" border="0" data-recalc-dims="1" />

The final result:

<img title="result.png" alt="Result" src="{{site.baseurl}}/assets/img/uploads/2013/03/eclipseresult.png?resize=523%2C152" border="0" data-recalc-dims="1" />