---
id: 333
title: 'Using Spring with JSF &#8211; the managed bean &#8211; tutorial'
date: 2009-03-06T00:06:49+00:00
author: Marco Molteni
layout: post
guid: http://molteni.wordpress.com/?p=5
permalink: /2009/03/06/spring-jsf-tutorial/
categories:
  - JSF
  - Spring
---
Two J2EE technologies are becoming dominant in the Java environment: Spring and Java ServerFaces.
  
Spring beans could be easily integrated in a JSF project. Spring Beans can coexists with JSF managed beans or interact directly with the view.

The steps necessary to use Spring in a JSF projects are the following:

**Libraries**
  
Spring libraries must be accessible 🙂

**web.xml**
  
In web.xml you have to declare your spring listener:

<pre class="brush: xml; title: ; notranslate" title="">&lt;listener&gt;
&lt;listener-class&gt;
    org.springframework.web.context.ContextLoaderListener
  &lt;/listener-class&gt;
&lt;/listener&gt;
</pre>

To use the request values between the view (jsp/jsf) ad the bean you have to add a second listener:

<pre class="brush: xml; title: ; notranslate" title="">&lt;listener&gt;
&lt;listener-class&gt;
org.springframework.web.context.request.RequestContextListener
&lt;/listener-class&gt;
&lt;/listener&gt;
</pre>

**faces-config.xml**
  
In faces-config.xml usually you define the managed beans. To use spring beans you have to add the following lines:

<pre class="brush: xml; title: ; notranslate" title="">&lt;application&gt;
&lt;variable-resolver&gt;
org.springframework.web.jsf.DelegatingVariableResolver
&lt;/variable-resolver&gt;&lt;code&gt;
&lt;/application&gt;
</pre>

**applicationContext.xml**
  
In the applicationContext.xml you have to declare your beans like a standard Spring application, the only difference is the property scope:

<pre class="brush: xml; title: ; notranslate" title="">&lt;bean name ="personMB" class="ch.genidea.ofac.web.jsf.Person" &lt;strong&gt;scope="request"&lt;/strong&gt;&gt;
&lt;property name= ... /&gt;
&lt;/bean&gt;
</pre>

Usually the **scope** property is set as _singleton_ (default). For the web application you have 3 possibilities:

  * request: Scopes a single bean definition to the lifecycle of a single HTTP request; that is each and every HTTP request will have its own instance of a bean created off the back of a single bean definition.
  * session: Scopes a single bean definition to the lifecycle of a HTTP <tt class="interfacename">Session</tt>.
  * global session: Scopes a single bean definition to the lifecycle of a global HTTP <tt class="interfacename">Session</tt>. Typically only valid when used in a portlet context.

**page.jsp**

In your page you can call the bean directly :

<pre class="brush: xml; title: ; notranslate" title="">&lt;h:outputText value="First name"/&gt; &lt;h:inputText value="#{personMB.firstName}" id="personName"/&gt;
&lt;h:outputText value="Family name"/&gt; &lt;h:inputText value="#{personMB.lastName}" id="personFamilyName"/&gt;
</pre>