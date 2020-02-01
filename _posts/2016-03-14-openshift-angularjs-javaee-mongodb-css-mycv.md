---
id: 570
title: OpenShift + Keycloak + AngularJS + JavaEE + MongoDB + CSS = MyCV
date: 2016-03-14T17:15:13+00:00
author: Marco Molteni
layout: post
guid: http://marco.dev/?p=570
permalink: /2016/03/14/openshift-angularjs-javaee-mongodb-css-mycv/
dsq_thread_id:
  - "5565926367"
categories:
  - AngularJS
  - EJB
  - Java EE
  - JSON
  - mongodb
  - REST
  - tutorial
  - Uncategorized
  - Web Services
tags:
  - java ee
  - JAX-RS
  - mongodb
  - OpenShift
  - REST
  - tutorial
---
&#8212;&#8212; Update Jul 25, 2016 &#8212;&#8212;
  
OpenShift is a great enterprise service. Unfortunately the resources needed by MongoDB and WildFly require some medium gears for an expense of $30 per month and the expense is not justified for a simple example website.
  
For this reason I rented a VPS, I installed everything from scrath and I moved to the  application to the new server. I will explain how to configure the server in a new post.
  
I used OVH as server provider but the procedure is the same with DigitalOcean. The goal was to spend less than $5/month and learn how to do it.
  
Preview: CentOS, WildFly, Java 8, MongoDB, Apache, Multiple websites.
  
&#8212;&#8212; END &#8212;&#8212;

Website: <a href="http://mycv.host" target="_blank">http://mycv.host</a>
  
GitHub: <a href="https://github.com/marco76/myCv" target="_blank">https://github.com/marco76/myCv</a>

I&#8217;ve a bit of time in this period and I&#8217;m updating (or, better, upgrading) my knowledge with the new trends on the market (recently I had to work with jsp and db2 ;))

I published a website that uses the following technologies:<img class="wp-image-588 alignleft" src="{{site.baseurl}}/assets/img/uploads/2016/03/stack.png?resize=173%2C183" alt="stack" data-recalc-dims="1" />

  * **OpenShift** as PaaS (it manages all this architecture for &#8230; free &#8230;)
  * **WildFly** 10 + RESTful (RESTeasy)
  * **MongoDB** for the data
  * [**Keycloak**](http://keycloak.jboss.org/) for the **Oauth2** security (google, github, facebook access)
  * **AngularJS** as JavaScript framework
  * HTML5 + **CSS** for the pages views

&nbsp;

The goal of the website is to glue all these technologies together and try to shrink my CV to only one page (fr<img class="size-full wp-image-571 alignright" src="https://i1.wp.com/marco.dev/wp-content/uploads/2016/03/screencapture-www-mycv-host-1457966794473.png?resize=198%2C295" alt="screencapture-www-mycv-host-1457966794473" data-recalc-dims="1" />om the original 5 pages).

The formatting of the CV is done using almost only &#8216;<div>&#8217; combined with CSS (the original template is not mine).

The data that fill the CV comes from a MongoDB server that contains the values. The data is in JSON format, a Java EE rest service send the data to AngularJS that fill the template.
  
<a href="https://i1.wp.com/marco.dev/wp-content/uploads/2016/03/2016-03-14_16-06-01_mongo-1.png" rel="attachment wp-att-573"><img class="alignnone size-medium wp-image-573" src="https://i1.wp.com/marco.dev/wp-content/uploads/2016/03/2016-03-14_16-06-01_mongo-1.png?resize=300%2C154" alt="2016-03-14_16-06-01_mongo" data-recalc-dims="1" /></a>

For each request to the CV a document with the information of the visitor is created in MongoDB an @Asynchronous method retrieve the visitor geographic information from [freegeoip.net](http://freegeoip.net).

Using only CSS is possible to change the CV if the recruiter decide to print it. In this case the border are eliminated from the document and the social icon (links) are replaced by the email address.

<a href="https://i1.wp.com/marco.dev/wp-content/uploads/2016/03/2016-03-14_16-18-19_print.png" rel="attachment wp-att-578"><img class="alignnone size-medium wp-image-578" src="https://i1.wp.com/marco.dev/wp-content/uploads/2016/03/2016-03-14_16-18-19_print.png?resize=300%2C87" alt="2016-03-14_16-18-19_print" data-recalc-dims="1" /></a>

I don&#8217;t add code in this Post, you can find it on github. The code is changing frequently. I just show how easy is to connect to the DB on OpenShift:

<pre class="brush: java; title: ; notranslate" title="">private MongoClient mongoClient = null;
 if (System.getenv("OPENSHIFT_MONGODB_DB_URL") == null){
    //localhost
     mongoClient = new MongoClient();
} else {
     // Openshift URL
     mongoClient = new MongoClient(new MongoClientURI(System.getenv("OPENSHIFT_MONGODB_DB_URL")));
 };
</pre>

**Security with Keycloak (and Google Oauth2)
  
** I initially started implementing the security using Oauth2 (you can find it at this endpoint <http://mycv.host/rest/login>) but I found a better solution : [Keycloak](http://keycloak.jboss.org/). It handles almost everything (email verification, social auth, password policies etc). The feature are impressive and allow to avoid a lot of risky plumbing on the security side.
  
Email verification request:
  
<a href="https://i0.wp.com/marco.dev/wp-content/uploads/2016/03/2016-03-17_10-46-32.png" rel="attachment wp-att-584"><img class="alignnone size-medium wp-image-584" src="https://i0.wp.com/marco.dev/wp-content/uploads/2016/03/2016-03-17_10-46-32.png?resize=300%2C127" alt="2016-03-17_10-46-32" data-recalc-dims="1" /></a>

Supported providers:
  
<a href="https://i1.wp.com/marco.dev/wp-content/uploads/2016/03/2016-03-17_11-32-48.png" rel="attachment wp-att-586"><img class="alignnone size-full wp-image-586" src="https://i1.wp.com/marco.dev/wp-content/uploads/2016/03/2016-03-17_11-32-48.png?resize=237%2C263" alt="2016-03-17_11-32-48" data-recalc-dims="1" /></a>

Next steps:

<li style="text-align: left;">
  Angular 2
</li>
<li style="text-align: left;">
  Bootstrap or Material Design
</li>
<li style="text-align: left;">
  Multiple users
</li>
<li style="text-align: left;">
  Min js
</li>
<li style="text-align: left;">
  &#8230;
</li>