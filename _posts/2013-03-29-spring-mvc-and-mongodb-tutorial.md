---
id: 278
title: Spring MVC and MongoDB tutorial
date: 2013-03-29T19:04:47+00:00
author: Marco Molteni
layout: post
guid: https://molteni.wordpress.com/?p=278
permalink: /2013/03/29/spring-mvc-and-mongodb-tutorial/
jabber_published:
  - "1364580292"
  - "1364580292"
publicize_twitter_user:
  - moltenma
  - moltenma
publicize_reach:
  - 'a:2:{s:7:"twitter";a:1:{i:1879338;i:3;}s:2:"wp";a:1:{i:0;i:0;}}'
  - 'a:2:{s:7:"twitter";a:1:{i:1879338;i:3;}s:2:"wp";a:1:{i:0;i:0;}}'
video_type:
  - '#NONE#'
categories:
  - java
  - Java 6
  - mongodb
  - nosql
  - Spring
  - Spring MVC
tags:
  - mongodb
  - spring data
  - spring mvc
---
This tutorial (not completed yet) has the goal to show how can be easy to create a new Web application based on Spring and MongoDB.

You can find the code of this small application here:
  
<https://github.com/marco76/mvcMongoExample>

The prerequisite is to have MongoDB installed and started.
  
Spring has a module to import all the necessary for MongoDB:

<pre class="brush: xml; title: ; notranslate" title="">&lt;dependency&gt;
    &lt;groupId&gt;org.springframework.data&lt;/groupId&gt;
    &lt;artifactId&gt;spring-data-mongodb&lt;/artifactId&gt;
    &lt;version&gt;1.1.1.RELEASE&lt;/version&gt;
&lt;/dependency&gt;
</pre>

In the servlet-context we configure the connection to the database and we create the template that we will use to interact with it:

<pre class="brush: xml; title: ; notranslate" title="">&lt;mongo:mongo id="mongo" host="localhost" port="27017"/&gt;

   &lt;beans:bean id="mongoTemplate" class="org.springframework.data.mongodb.core.MongoTemplate"&gt;
       &lt;beans:constructor-arg ref="mongo"/&gt;
       &lt;beans:constructor-arg name="databaseName" value="jobfinder"/&gt;
   &lt;/beans:bean&gt;

   &lt;mongo:repositories base-package="ch.genidea.mvcMongoExample.repository"/&gt;

</pre>

Here you can see how simple his our class structure:

<img alt="Screen Shot 2013-03-29 at 19.35.57.png" src="http://molteni.files.wordpress.com/2013/03/screen-shot-2013-03-29-at-19-35-571.png?resize=269%2C189" border="0" data-recalc-dims="1" />

We use a Person class to store the data in the database:

<pre class="brush: java; title: ; notranslate" title="">import org.bson.types.ObjectId;
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;

@Document
public class Person {

    @Id
    private ObjectId id;
    private String username;
    private String password;

    public Person() {
    }
</pre>

For MongoDB we use the annotation @Document in place of the more traditional @Entity used for traditional databases.

In MainMongo.java you can find an example about how to use the Person class.

<pre class="brush: java; title: ; notranslate" title="">public class MainMongo {

    private static final Logger log = LoggerFactory.getLogger(MainMongo.class);

    public static void main(String[] args) throws Exception {

        MongoOperations mongoOperations = new MongoTemplate(new Mongo(), &amp;quot;database&amp;quot;);

        mongoOperations.insert(new Person(&amp;quot;marco&amp;quot;, &amp;quot;marco&amp;quot;));

        Person person = mongoOperations.findOne(new Query(Criteria.where(&amp;quot;username&amp;quot;).is(&amp;quot;marco&amp;quot;)), Person.class);

        log.debug(&amp;quot;Person found : &amp;quot; + person.getUsername());

        mongoOperations.dropCollection(&amp;quot;person&amp;quot;);
    }
}
</pre>

To update the database we can leverage the power of spring data. It&#8217;s enough to create an interface that extends CrudRepository to have basic db features magically available:

<pre class="brush: java; title: ; notranslate" title="">import ch.genidea.mvcMongoExample.model.Person;
import org.springframework.data.repository.CrudRepository;

import java.util.List;

public interface PersonRepository extends CrudRepository&amp;lt;Person, Long&amp;gt; {
    List&amp;lt;Person&amp;gt; findByUsername(String username);
}
</pre>

The interface is used in our HomeController to retrieve the list of people records in the database:

<pre class="brush: java; title: ; notranslate" title="">@Autowired
    private PersonRepository personRepository;

    /**
     * Home page renderer
     */
    @RequestMapping(value = &amp;quot;/&amp;quot;, method = RequestMethod.GET)
    public String home(Locale locale, Model model) {

        logger.info(&amp;quot;Home page&amp;quot;);

        Iterable&amp;lt;Person&amp;gt; personList = personRepository.findAll();

        model.addAttribute(&amp;quot;personList&amp;quot;, personList);

        return &amp;quot;home&amp;quot;;
    }
</pre>

Here the result:

<img alt="Screen Shot 2013-03-29 at 19.47.23.png" src="http://molteni.files.wordpress.com/2013/03/screen-shot-2013-03-29-at-19-47-23.png?resize=272%2C207" border="0" data-recalc-dims="1" />

Alternatively we can use MongoTemplate as in the MainMongo class or in the PersonController where we mix MongoTemplate and CrudRepository just to show the possibilities that Spring give to the developer:

We have a very poor page that allows to add a new Person:
  
<img alt="Screen Shot 2013-03-29 at 20.20.43.png" src="http://molteni.files.wordpress.com/2013/03/screen-shot-2013-03-29-at-20-20-431.png?resize=237%2C149" border="0" data-recalc-dims="1" />

when the user click on &#8216;Save&#8217; the method addPerson is called:

<pre class="brush: java; title: ; notranslate" title="">@Autowired
    MongoTemplate mongoTemplate;
    @Autowired
    PersonRepository personRepository;

    @RequestMapping(value = &amp;quot;/savePerson&amp;quot;, method = RequestMethod.POST)
    public String addPerson(@ModelAttribute(&amp;quot;person&amp;quot;) Person person, BindingResult result) {

        List personList = personRepository.findByUsername(person.getUsername());

        if (personList.size() &amp;gt; 0) {
            System.out.println(&amp;quot;Error username already exists&amp;quot;);
        } else {
            System.out.println(&amp;quot;Ok: new username&amp;quot;);
            mongoTemplate.save(person);
        }

        return &amp;quot;redirect:/&amp;quot;;
    }
</pre>