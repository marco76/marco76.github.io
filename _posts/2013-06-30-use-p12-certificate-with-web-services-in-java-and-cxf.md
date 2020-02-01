---
id: 391
title: Use a p12 certificate with Web Services / SSL in Java and CXF
date: 2013-06-30T18:24:41+00:00
author: Marco Molteni
layout: post
guid: http://marco.dev/?p=391
permalink: /2013/06/30/use-p12-certificate-with-web-services-in-java-and-cxf/
video_type:
  - '#NONE#'
categories:
  - java
  - Java 6
  - Java EE
  - Uncategorized
  - Web Services
tags:
  - certificate
  - cxf
  - java
  - p12
  - security
  - web services
---
Problem : I need to access a webservice of a provider. The provider gave to me a **.cert** file and a **.p12** file.

The private key p12 is not easy to manage in java. The default Key Store (cacert) doesn’t manage private keys.

I’m using [apache cxf](http://cxf.apache.org/) to access the webservice.

Solution:

1. _Create a new Key Store for your private key and import the p12 file_

<pre class="brush: xml; title: ; notranslate" title="">keytool -importkeystore -srckeystore privateKeyFile.p12 -srcstoretype PKCS12 -destkeystore personalKeyStore.jks
</pre>

The .cert file should be imported in the cacert file:

<http://docs.oracle.com/javase/1.5.0/docs/tooldocs/solaris/keytool.html>

You can easily access the certificate and the personal key from cxf using the following configuration.
  
For the private.jks store you need **two passwords**:
  
One password to access the file private.jks: ‘storePassword’
  
One password to access the private key inside the private.jks: ‘personalKeyPassword’ the provider gave to you this password.

<div class="oembed-gist">
  <noscript>
    View the code on <a href="https://gist.github.com/marco76/5895736">Gist</a>.
  </noscript>
</div>