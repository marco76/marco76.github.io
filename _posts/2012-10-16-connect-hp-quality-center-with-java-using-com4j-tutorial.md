---
id: 188
title: 'Connect to HP Quality Center using Java and com4j &#8211; Tutorial (part 1)'
date: 2012-10-16T15:00:46+00:00
author: Marco Molteni
layout: post
guid: http://molteni.wordpress.com/?p=188
permalink: /2012/10/16/connect-hp-quality-center-with-java-using-com4j-tutorial/
jabber_published:
  - "1350396046"
publicize_twitter_user:
  - moltenma
dsq_thread_id:
  - "5565861297"
categories:
  - HP Quality Center
  - java
---
I try to add some new information about how to connect to Quality Center using Java.

I will complete a step by step tutorial from the creation of the basic classes to the querying of the bugs.

In this first tutorial I show how to create the classes needed to connect your program to QC using the OtaClient.dll. OtaClient is not a java library and cannot used directly by our program, we need to create a wrapper that translate our request to a native language. To do this the fastest method is to use Com4j

**1. Find the OtaClient.dll**

You should find the OtaClient.dll in the following folder:

c:Program Files (x86)Common FilesMercuryInteractiveQuality Center

[<img class="alignnone size-full wp-image-190" title="qc_Ota_dll" alt="" src="http://molteni.files.wordpress.com/2012/10/qc_ota_dll1.png?resize=577%2C111" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/10/qc_ota_dll1.png?resize=577%2C111)

If you cannot find the library try to use the search features of Windows. This library is required to communicate to the server.

**2. Download com4j**

<http://java.net/projects/com4j/downloads>

When you downloaded the library you have to unzip it. As example I unzipped it in the dev folder.

**3. Use com4j to generate a library with the wrapped classes**

Because com4j is not compatible with 64 bit systems, use a 32 bit JRE or you will get:

[<img class="alignnone size-full wp-image-185" title="qc_error" alt="" src="http://molteni.files.wordpress.com/2012/10/qc_error.png?resize=595%2C257" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/10/qc_error.png?resize=595%2C257)

Execute the tblimp.jar with a 32-bit JRE, you can use the following parameters:

-o is for the destination directory

-p is for the java package of destination

[<img class="alignnone size-full wp-image-184" title="exe" alt="" src="http://molteni.files.wordpress.com/2012/10/exe.png?resize=595%2C45" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/10/exe.png?resize=595%2C45)

You could have few warning:

[<img class="alignnone size-full wp-image-187" title="qc_result" alt="" src="http://molteni.files.wordpress.com/2012/10/qc_result.png?resize=343%2C357" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/10/qc_result.png?resize=343%2C357)

but at the end you should have your the sources ready under hpqc/com/qc:

[<img class="alignnone size-full wp-image-186" title="qc_java" alt="" src="http://molteni.files.wordpress.com/2012/10/qc_java.png?resize=324%2C638" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/10/qc_java.png?resize=324%2C638)

You can import these sources to connect and use QC from your application.

More details in the next tutorial …