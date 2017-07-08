---
id: 537
title: 'RESTful web service with Java EE (JAX-RS) and JSON: Hello World tutorial'
date: 2016-02-05T18:36:31+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=537
permalink: /2016/02/05/restful-web-service-with-java-ee-jax-rs-and-json-beginners-tutorial/
categories:
  - Glassfish
  - java
  - Java EE
  - JSON
  - Netbeans
  - REST
  - Web Services
tags:
  - Glassfish
  - java
  - JAX-RS
  - JSON
  - Netbeans
  - REST
  - WebServices
---
You can find the java code here: <a href="https://github.com/marco76/simpleRestExample" target="_blank">https://github.com/marco76/simpleRestExample</a>

For this example we use Netbeans 8.1 (with the new and nice darkula theme :)) that comes with Glassfish Server out-of-the-box.
  
The goal is to create and show the structure of a basic REST service in Java.
  
In the tutorial:
  
1. basic REST application with Java EE (no Spring)
  
2. response to a get with : text, list on JSON objects
  
3. simple integration test using Client

## File structure

For this example we create 4 java classes (2 for the code, 1 for the unit test, 1 for the integration test).

<a href="https://i0.wp.com/javaee.ch/wp-content/uploads/2016/02/2016-02-05_17-48-28.png" rel="attachment wp-att-554"><img src="https://i0.wp.com/javaee.ch/wp-content/uploads/2016/02/2016-02-05_17-48-28.png?resize=300%2C270" alt="2016-02-05_17-48-28" class="alignnone size-medium wp-image-554" data-recalc-dims="1" /></a>

## Configuration

In RestConfig.java we tell to the server that there is a REST service and his address (endpoint).

<pre class="brush: java; title: ; notranslate" title="">import javax.ws.rs.ApplicationPath;
import javax.ws.rs.core.Application;

/**
 *
 * This class is used to tell to the server that there are REST services.
 * An alternative is to declare a servlet in the web.xml.
 * 
 * @author marco
 */

@ApplicationPath("rest") // the 'rest' adress is mapped to the REST services
public class RestConfig extends Application{ // a javax.ws.rs.core.Application must be extended
    
}

</pre>

We can configure the service with an annotation like in our example or declaring a Servlet in web.xml. In this example we don&#8217;t even create web.xml.
  
We leave the code of the class empty because we implements the methods in a second class: RestHelloWorld.java

## Service class

### Simple text answer

<pre class="brush: java; title: ; notranslate" title="">// @Provider tell the server that this is a REST class
@Provider  
// @Path defines the path for this class: [server]/rest/examples
@Path("examples") 
public class RestHelloWorld {

</pre>

@Provider: tells to the server that this is a service class
  
@Path: part of the URL used to call the service

In this class we declare 1 method that return a simple text:

<pre class="brush: java; title: ; notranslate" title="">// path used to call the method: [server]/rest/examples/examples/helloWorld
    @Path("helloWorld") 
    // answer only to a http get request
    @GET 
    // return a simple string (text/plain by default)
    public String hello(){  
        return "Hello World"; // string to be returned
    }
 
</pre>

@Path: is the url to be used to call the service. It is added to the server URL + @ApplicationPath + @Path of the class. The name of the method is not important.
  
@GET : reads the calls with the http get method (ex. url written directly in the address bar of the browser)

This method return a simple text (no HTML) that is shown in the browser:

<a href="https://i2.wp.com/javaee.ch/wp-content/uploads/2016/02/2016-02-05_17-01-34.png" rel="attachment wp-att-542"><img class="alignnone size-medium wp-image-542" src="https://i2.wp.com/javaee.ch/wp-content/uploads/2016/02/2016-02-05_17-01-34.png?resize=300%2C39" alt="2016-02-05_17-01-34" data-recalc-dims="1" /></a>

### Java List to JSON Array answer

The second method is more interesting because it return a list of String in JSON format

<pre class="brush: java; title: ; notranslate" title="">@Path("helloJSON")
@GET
@Produces("application/json")
public List&lt;String&gt; helloJSONList(){
    List&lt;String&gt; jsonList = new ArrayList&lt;String&gt;();
    jsonList.add("Hello World");
    jsonList.add("Bonjour monde");
        
    return jsonList;           
}
 
</pre>

@Produces: defines in which format the answer should be returned. It automatically transform our object (List of strings) in JSON format.

<a href="https://i1.wp.com/javaee.ch/wp-content/uploads/2016/02/2016-02-05_17-05-36.png" rel="attachment wp-att-543"><img class="alignnone size-medium wp-image-543" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2016/02/2016-02-05_17-05-36.png?resize=300%2C41" alt="2016-02-05_17-05-36" data-recalc-dims="1" /></a>

Netbeans recognize the services declared in this class and show them in the project structure:

<a href="https://i2.wp.com/javaee.ch/wp-content/uploads/2016/02/2016-02-05_17-29-33.png" rel="attachment wp-att-547"><img src="https://i2.wp.com/javaee.ch/wp-content/uploads/2016/02/2016-02-05_17-29-33.png?resize=293%2C109" alt="2016-02-05_17-29-33" class="alignnone size-full wp-image-547" data-recalc-dims="1" /></a>

To transform Java Objects in JSON and vice versa we need to import some converters in our project to avoid some MessageBodyWriter or MessageBodyReader media type error:

<pre class="brush: xml; title: ; notranslate" title="">&lt;!-- JSON support --&gt;
        &lt;!-- List to JSON --&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;com.sun.jersey&lt;/groupId&gt;
            &lt;artifactId&gt;jersey-json&lt;/artifactId&gt;
            &lt;version&gt;1.19&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;!-- JSON to List - MessageBodyReader media type --&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.glassfish.jersey.media&lt;/groupId&gt;
            &lt;artifactId&gt;jersey-media-json-jackson&lt;/artifactId&gt;
            &lt;version&gt;2.22.1&lt;/version&gt;
        &lt;/dependency&gt;
</pre>

## Integration Test

We can test our services directly using an explorer or, better, with some integration tests.

The class RestHelloWorldTestIT.java is used to test the REST service if it is up and running.
  
In the following code the test try to read the JSON list from the server converting it in a java String:

<pre class="brush: java; title: ; notranslate" title="">@Test
 public void testIntegrationHelloJSON(){
     // the client connect to the REST service
     Client client = ClientBuilder.newClient();
        
     List&lt;String&gt; helloWorldString = client.target(helloWorldURL+"helloJSON") // connection to the pre-defined URL
                 .request()
                 .get(ArrayList.class); // we call the 'get' method and we transform the answer in a String
        assertEquals(2, helloWorldString.size());
 }
 
</pre>

Because of the name convention Maven recognize that this is an integration test and it doesn&#8217;t execute it with the unit tests.
  
You need the failsafe plugin to profit from this feature:

<pre class="brush: xml; title: ; notranslate" title="">&lt;plugin&gt;
                &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
                &lt;artifactId&gt;maven-failsafe-plugin&lt;/artifactId&gt;
      ...    
</pre>