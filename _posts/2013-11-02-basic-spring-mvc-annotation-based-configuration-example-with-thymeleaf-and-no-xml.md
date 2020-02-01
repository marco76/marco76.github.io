---
id: 439
title: Basic Spring MVC example with Thymeleaf and no XML (annotation based configuration)
date: 2013-11-02T23:52:22+00:00
author: Marco Molteni
layout: post
guid: http://marco.dev/?p=439
permalink: /2013/11/02/basic-spring-mvc-annotation-based-configuration-example-with-thymeleaf-and-no-xml/
categories:
  - java
  - Maven
  - Spring
  - Spring MVC
  - Springframework
  - Thymeleaf
  - tutorial
tags:
  - java
  - spring mvc
  - Thymeleaf
  - tutorial
---
Since the version 3.x Springframework allows to write applications without any use of xml. It&#8217;s quite funny the idea that Spring the &#8216;xml oriented framework&#8217; now is completely xml-free. 

I tried to develop a website (a simple page to be honest) using the annotation configuration and the new &#8216;kid on the block&#8217;: Thymeleaf.
  
I think I&#8217;ll develop these example to build a new real website. 

As usual you can find the source on github:
  
<https://github.com/marco76/minimvc>

[<img src="https://i1.wp.com/marco.dev/wp-content/uploads/2013/11/mini_mvc_classes.png?resize=222%2C300" alt="mini_mvc_classes" class="alignnone size-medium wp-image-446" data-recalc-dims="1" />](https://i1.wp.com/marco.dev/wp-content/uploads/2013/11/mini_mvc_classes.png)

## ThymeleafConfig.java

We created a class that contains the Thymeleaf configuration : templateResolver, templateEngine, viewResolver

<pre class="brush: java; title: ; notranslate" title="">@Configuration
@PropertySource("classpath:thymeleaf.properties")
public class ThymeleafConfig {


    @Bean
    public TemplateResolver templateResolver(){
        ServletContextTemplateResolver templateResolver = new ServletContextTemplateResolver();
        templateResolver.setPrefix("/WEB-INF/templates/");
        templateResolver.setSuffix(".html");
        templateResolver.setTemplateMode("HTML5");

        return templateResolver;
    }

    @Bean
    public SpringTemplateEngine templateEngine(){
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateResolver(templateResolver());
        return templateEngine;
    }

    @Bean
    public ViewResolver viewResolver(){
        ThymeleafViewResolver viewResolver = new ThymeleafViewResolver() ;
        viewResolver.setTemplateEngine(templateEngine());
        viewResolver.setOrder(1);

        return viewResolver;
    }
</pre>

You have to declare a ViewResolver that allow Spring to find and create the answer page. In the case of a &#8216;traditional&#8217; jsp the ViewResolver would be the following:

<pre class="brush: java; title: ; notranslate" title="">//Defines the ViewResolver that Spring will use to render the views.
    @Bean
    public ViewResolver viewResolver() {
        System.out.println("Inside View Resolver...");
        InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
        viewResolver.setViewClass(JstlView.class);
        viewResolver.setPrefix("/WEB-INF/views/");
        viewResolver.setSuffix(".jsp");

        return viewResolver;
    }
</pre>

## AppConfiguration.java

The AppConfiguration class is the main class of the application. The _@EnableWebMvc_ tells to Spring that we are developing a web application. 

_@Import_ it&#8217;s the equivalent of &#8216;<import resource= ... >&#8216; in XML. It allows us to use the beans declared in other configuration files.
  
As you can see the AppConfiguration can be empty. 

<pre class="brush: java; title: ; notranslate" title="">@Configuration
@EnableWebMvc
@ComponentScan(basePackages = "ch.javaee.simpleMvc")
@Import({ WebInitializer.class, DispatcherConfig.class})
public class AppConfiguration {
}
</pre>

## WebInitializer.java

The WebInitializer implements the WebApplicationInitializer that configures the ServletContext.
  
You can find more information in the [Spring official documentation](http://docs.spring.io/spring/docs/3.1.x/javadoc-api/org/springframework/web/WebApplicationInitializer.html).
  
In brief, this class is bootstrapped by the application server (Servlet v.3.0) and it registers the dispatcher. 

<pre class="brush: java; title: ; notranslate" title="">@Configuration
public class WebInitializer implements WebApplicationInitializer {
    @Override
    public void onStartup(ServletContext container) throws ServletException {
        
        // Create the 'root' Spring application context
        AnnotationConfigWebApplicationContext rootContext =
                new AnnotationConfigWebApplicationContext();
        rootContext.register(AppConfiguration.class);

        // Manage the lifecycle of the root application context
        container.addListener(new ContextLoaderListener(rootContext));

        // Create the dispatcher servlet's Spring application context
        AnnotationConfigWebApplicationContext dispatcherContext =
                new AnnotationConfigWebApplicationContext();
        dispatcherContext.register(DispatcherConfig.class);

        // Register and map the dispatcher servlet
        ServletRegistration.Dynamic dispatcher =
                container.addServlet("dispatcher", new DispatcherServlet(dispatcherContext));
        dispatcher.setLoadOnStartup(1);
        dispatcher.addMapping("/");
    }
}
</pre>

## Conclusion

There is no need of any xml file. We don&#8217;t use any web.xml or any configuration.xml file. Spring allow to combine the use of xml files with _@Configuration_ annotations simplifying the integration of legacy code.
  
The integration of Thymeleaf is straightforward. As declared in the templateResolver bean the web pages are found in the /WEB-INF/templates/ directory.
  
The _@Controller_ assign the value &#8220;Hello world!&#8221; to the attribute &#8220;title&#8221; and pass it to the &#8220;main&#8221; page.

<pre class="brush: java; title: ; notranslate" title="">@RequestMapping(method = RequestMethod.GET)
	public String printWelcome(ModelMap model) {
		model.addAttribute("title", "Hello world!");
		return "main";
	}
</pre>

Eventually, the &#8220;main.html&#8221; page receives and shows the value of the attribute.

<pre class="brush: xml; title: ; notranslate" title="">&lt;body&gt;
&lt;img src="../../images/jt_net.png" th:attr="src=@{images/jt_net.png}"/&gt;
    &lt;h2 th:text="${title}"&gt;Title&lt;/h2&gt;
&lt;/body&gt;
</pre></p> 

Here you can see the result:
  
[<img src="https://i1.wp.com/marco.dev/wp-content/uploads/2013/11/mini_mvc_result.png?resize=300%2C110" alt="mini_mvc_result" class="alignnone size-medium wp-image-445" data-recalc-dims="1" />](https://i1.wp.com/marco.dev/wp-content/uploads/2013/11/mini_mvc_result.png)