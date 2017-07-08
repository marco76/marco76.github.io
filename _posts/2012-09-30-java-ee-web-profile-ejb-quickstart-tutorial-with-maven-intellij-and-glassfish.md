---
id: 126
title: Java EE Web Profile (EJB) quickstart tutorial with Maven, Intellij and Glassfish
date: 2012-09-30T12:52:53+00:00
author: Marco Molteni
layout: post
guid: http://molteni.wordpress.com/?p=126
permalink: /2012/09/30/java-ee-web-profile-ejb-quickstart-tutorial-with-maven-intellij-and-glassfish/
jabber_published:
  - "1349005979"
categories:
  - EJB
  - Glassfish
  - Intellij
  - java
  - Java 6
  - Java EE
  - Javaserver Faces
  - JSF
  - Maven
---
This short tutorial has the goal to show how to start as fast as possible a new EJB project using IntelliJ and Maven.

1. Create a new project from scratch

[<img class="alignnone size-medium wp-image-127" title="start1" src="http://molteni.files.wordpress.com/2012/09/start11.png?w=300&#038;resize=300%2C138" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/start11.png)

2. Complete the form, you have to choose the Maven module

[<img class="alignnone size-medium wp-image-155" title="start2" src="http://molteni.files.wordpress.com/2012/09/start2.png?w=300&#038;resize=300%2C157" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/start2.png)

3. Complete the Maven information

[<img class="alignnone size-medium wp-image-136" title="Untitled" src="http://molteni.files.wordpress.com/2012/09/untitled.jpg?w=300&#038;resize=300%2C88" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled.jpg)

4. Your project is created according to Maven standards

[<img class="alignnone size-medium wp-image-137" title="maven_4" src="http://molteni.files.wordpress.com/2012/09/untitled2.jpg?w=300&#038;resize=300%2C237" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled2.jpg)

5. Add Java EE dependencies, this step is necessary to create a Web Profile project, Intellij will download automatically the new artifact

[<img class="alignnone size-medium wp-image-139" title="Maven_step_5" src="http://molteni.files.wordpress.com/2012/09/untitled4.jpg?w=300&#038;resize=300%2C92" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled4.jpg)

We have to define the type of packaging we want in our maven config. Without declaring a packaging of type war the application won&#8217;t be deployed.

[<img class="alignnone size-medium wp-image-154" title="Untitled29" src="http://molteni.files.wordpress.com/2012/09/untitled29.jpg?w=300&#038;resize=300%2C48" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled29.jpg)

6. Create a new Class, this class is of type Stateless (a Service for who comes from Spring)

[<img class="alignnone size-full wp-image-132" title="Untitled 3" src="http://molteni.files.wordpress.com/2012/09/untitled-3.jpg?resize=207%2C99" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled-3.jpg?resize=207%2C99)

7. Edit the class, we do something very easy: a classical HelloWorld bean.
  
With the EE web profile we don&#8217;t need to create interfaces if we don&#8217;t access the stateless bean (service) from a different JVM.

[<img class="alignnone size-medium wp-image-133" title="Untitled 12" src="http://molteni.files.wordpress.com/2012/09/untitled-12.jpg?w=300&#038;resize=300%2C238" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled-12.jpg)

8. We create our web configuration. Add a web.xml file under WEB-INF

[<img class="alignnone size-full wp-image-146" title="Untitled12" src="http://molteni.files.wordpress.com/2012/09/untitled12.jpg?resize=187%2C134" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled12.jpg?resize=187%2C134)

9. The web.xml file is quite traditional, you can use one of the many templates on the web

[<img class="alignnone size-medium wp-image-147" title="Untitled13" src="http://molteni.files.wordpress.com/2012/09/untitled13.jpg?w=271&#038;resize=271%2C300" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled13.jpg)

9. We can create now the managed bean, it&#8217;s the controller who manage the communication between the View (jsp) and the Model (EJB)

[<img class="alignnone size-full wp-image-157" title="11" src="http://molteni.files.wordpress.com/2012/09/111.jpg?resize=215%2C119" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/111.jpg?resize=215%2C119)

10. The code of the controller is simple. The class is declared as @ManagedBean to allow the injection in the view. The EJB (Stateless class) is injected using the annotation. The getText method is exposed to the view.

[<img class="alignnone size-medium wp-image-133" title="Untitled 12" src="http://molteni.files.wordpress.com/2012/09/untitled-12.jpg?w=300&#038;resize=300%2C238" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled-12.jpg)

11. We can now create facelets page

[<img class="alignnone size-medium wp-image-134" title="Untitled 13" src="http://molteni.files.wordpress.com/2012/09/untitled-13.jpg?w=300&#038;resize=300%2C119" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled-13.jpg)

12. We call the view Hello.xhtml and we place it in the main webapp folder

[<img class="alignnone size-full wp-image-135" title="Untitled 14" src="http://molteni.files.wordpress.com/2012/09/untitled-14.jpg?resize=149%2C81" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled-14.jpg?resize=149%2C81)

13. The interesting part of the code is the call to the helloMB managed bean. The @ManagedBean annotation previously used allow us to access easily to the bean.

[<img class="alignnone size-medium wp-image-138" title="Untitled3" src="http://molteni.files.wordpress.com/2012/09/untitled3.jpg?w=300&#038;resize=300%2C223" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled3.jpg)

14. We connect to a local GlassFish server

[<img class="alignnone size-medium wp-image-158" title="glassfish" src="http://molteni.files.wordpress.com/2012/09/glassfish.png?w=300&#038;resize=300%2C163" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/glassfish.png)

15.  We use the web explosed facet to deploy our application

[<img class="alignnone size-medium wp-image-151" title="Untitled26" src="http://molteni.files.wordpress.com/2012/09/untitled26.jpg?w=300&#038;resize=300%2C95" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled26.jpg)

[<img class="alignnone size-medium wp-image-149" title="Untitled23" src="http://molteni.files.wordpress.com/2012/09/untitled23.jpg?w=300&#038;resize=300%2C116" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled23.jpg)

16. We have to choose to which domain deploy our application (e.g. domain1)

[<img class="alignnone size-medium wp-image-152" title="Untitled27" src="http://molteni.files.wordpress.com/2012/09/untitled27.jpg?w=300&#038;resize=300%2C208" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled27.jpg)

17. We can start the server. The application is deployed.

[<img class="alignnone size-full wp-image-153" title="Untitled28" src="http://molteni.files.wordpress.com/2012/09/untitled28.jpg?resize=156%2C44" alt="" data-recalc-dims="1" />](http://molteni.files.wordpress.com/2012/09/untitled28.jpg?resize=156%2C44)