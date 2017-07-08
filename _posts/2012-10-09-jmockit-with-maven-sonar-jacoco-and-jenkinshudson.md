---
id: 164
title: JMockit with Maven, Sonar, JaCoCo and Jenkins/Hudson
date: 2012-10-09T08:21:54+00:00
author: Marco Molteni
layout: post
guid: http://molteni.wordpress.com/?p=164
permalink: /2012/10/09/jmockit-with-maven-sonar-jacoco-and-jenkinshudson/
jabber_published:
  - "1349767315"
categories:
  - java
  - Java 6
  - Jenkins
  - JMockit
  - Maven
  - Sonar
  - Uncategorized
---
We deployed our fresh new tests based on JMockit in our Jenkins/Sonar environment and surprise &#8230; no unit test works anymore. Even the tests that don&#8217;t use JMockit (simple JUnit) stopped to work. It was strange that during the build phase with test there was no problem but in the Sonar test phase the first JUnit test looped permanently.

It took a while to figure out the problem.

There is an incompatibility between JaCoCo and JMockit.

To solve the issue we added the jmockit javaagent to the maven-surefire-plugin:

<pre class="brush: xml; title: ; notranslate" title="">&lt;argLine&gt;-javaagent:"${settings.localRepository}"/com/googlecode/jmockit/jmockit/0.999.15/jmockit-0.999.15.jar&lt;/argLine&gt;
</pre>

Here how it looks like the plugin:

<pre class="brush: xml; title: ; notranslate" title="">&lt;plugin&gt;
 &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
 &lt;artifactId&gt;maven-surefire-plugin&lt;/artifactId&gt;
 &lt;version&gt;2.12&lt;/version&gt;
 &lt;configuration&gt;
&lt;argLine&gt;-javaagent:"${settings.localRepository}"/com/googlecode/jmockit/jmockit/0.999.15/jmockit-0.999.15.jar&lt;/argLine&gt;
 &lt;/configuration&gt;
&lt;/plugin&gt;</pre>

After this change the test are executed correctly and the incompatibility between JaCoCo and JMockit solved!