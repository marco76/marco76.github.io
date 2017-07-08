---
id: 334
title: Configure a DataSource for MySQL and TomEE tutorial
date: 2013-03-25T19:54:38+00:00
author: Marco Molteni
layout: post
guid: http://molteni.wordpress.com/?p=262
permalink: /2013/03/25/configure-a-datasource-for-mysql-and-tomee/
jabber_published:
  - "1364237678"
publicize_reach:
  - 's:63:"a:2:{s:7:"twitter";a:1:{i:1879338;i:3;}s:2:"wp";a:1:{i:0;i:0;}}";'
publicize_twitter_user:
  - moltenma
categories:
  - java
  - Java EE
  - JPA
  - TomEE
---
Copy the driver of MySql in the TomEE lib directory:

<img alt="Mysql" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2013/03/tomeemysql.png?resize=600%2C208" border="0" data-recalc-dims="1" />

You have to modify the config file tomEE/conf/tomee.xml

In the file you have to add your mysql configuration:

<img alt="Tomee" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2013/03/tomeetomee.png?resize=600%2C210" border="0" data-recalc-dims="1" />

<pre class="brush: xml; title: ; notranslate" title="">&lt;Resource id="pmone" type="DataSource"&gt;
JdbcDriver com.mysql.jdbc.Driver
JdbcUrl jdbc:mysql://localhost:3306/pm
UserName user
Password secret
JtaManaged true
&lt;/Resource&gt;
</pre>

Your persistence xml has to use the datasource defined in tomee.xml (in this case &#8216;phone&#8217;)

<pre class="brush: xml; title: ; notranslate" title="">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;persistence version="2.0" xmlns="http://java.sun.com/xml/ns/persistence"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://java.sun.com/xml/ns/persistence
 
http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd"&gt;
 
&lt;persistence-unit name="pmWebPU"&gt;
&lt;jta-data-source&gt;pmone&lt;/jta-data-source&gt;
&lt;exclude-unlisted-classes&gt;false&lt;/exclude-unlisted-classes&gt;
 
&lt;properties&gt;
 
&lt;/properties&gt;
&lt;/persistence-unit&gt;
&lt;/persistence&gt;
</pre>

Done 🙂