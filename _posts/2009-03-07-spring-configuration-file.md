---
id: 24
title: 'Spring &#8211; Configuration file'
date: 2009-03-07T13:16:38+00:00
author: Marco Molteni
layout: post
guid: http://molteni.wordpress.com/?p=24
permalink: /2009/03/07/spring-configuration-file/
redirect_to:
- https://www.ngjava.com/spring-configuration-file/
categories:
  - java
  - Spring
---
**Problem**
  
Spring is managing the database connection of my application. I want to extract the parameters outside my applicationContext.xml file

<pre class="brush: xml; title: ; notranslate" title="">&lt;bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource"&gt;
&lt;property name="driverClassName" value="com.mysql.jdbc.Driver" /&gt;
&lt;property name="url" value="jdbc:mysql://localhost/database" /&gt;
&lt;property name="username" value="user"/&gt;
&lt;property name="password" value="password"/&gt;
  &lt;/bean&gt;
</pre>

**Solution**
  
Add the PropertyPlaceholderConfigurer bean to your ApplicationContext with the name of the file(s) containing the data.

<pre class="brush: xml; title: ; notranslate" title="">&lt;bean id="propertyConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer"&gt;
&lt;property name="location"&gt;
        &lt;value&gt;WEB-INF/data.properties&lt;/value&gt;
    &lt;/property&gt;
&lt;/bean&gt;
</pre>

If you have more property files you can use the <list> tag:

<pre class="brush: xml; title: ; notranslate" title="">&lt;property name="locations"&gt;
	&lt;list&gt;
&lt;value&gt;data.properties&lt;/value&gt;
&lt;value&gt;...&lt;/value&gt;
&lt;/list&gt;
&lt;/property&gt;
</pre>

The data.properties should be like this:

<pre class="brush: java; title: ; notranslate" title="">database.url= jdbc:mysql://localhost/CheckNames
database.username=username
database.password=password
</pre>

To use these properties in your applicationContext.xml file:

<pre class="brush: xml; title: ; notranslate" title="">&lt;bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource"&gt;
&lt;property name="driverClassName" value="com.mysql.jdbc.Driver" /&gt;
&lt;property name="url"&gt;&lt;value&gt;${database.url}&lt;/value&gt;&lt;/property&gt;
&lt;property name="username"&gt;&lt;value&gt;${database.username}&lt;/value&gt;&lt;/property&gt;
&lt;property name="password"&gt; &lt;value&gt;${database.password}&lt;/value&gt;&lt;/property&gt;
  &lt;/bean&gt;
</pre>

[Spring reference](http://static.springframework.org/spring/docs/2.5.x/api/org/springframework/beans/factory/config/PropertyPlaceholderConfigurer.html)