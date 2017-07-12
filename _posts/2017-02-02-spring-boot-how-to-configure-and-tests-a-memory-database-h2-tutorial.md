---
title: 'Spring Boot and JPA. How to configure and test a memory database (H2) &#8211; Tutorial'
date: 2017-02-02T00:36:12+00:00
author: Marco Molteni
layout: post
permalink: /2017/02/02/spring-boot-how-to-configure-and-tests-a-memory-database-h2-tutorial/
categories:
  - example
  - Hibernate
  - java
  - JPA
  - Spring
  - Spring Boot
  - tutorial
tags:
  - Database
  - H2
  - JPA
  - junit
  - spring boot
  - Unit Test
---
I want to add a blog feature to my reference Angular / Java application. For this I need to write and read the database.

I decided to use H2 database and access it with the Spring Repositories and JPA. Spring Boot has a lot of features to easy manage our data.

The call hierarchy for the data is the following: Web controller -> Service -> Repository

You find the complete code here (Database branch): <a href="https://github.com/marco76/SpringAngular2TypeScript/tree/feature/database/server/src" target="_blank">SpringAngular2TypeScript</a>

Here the example of the <a href="https://github.com/marco76/SpringAngular2TypeScript/blob/feature/database/server/src/main/java/ch/javaee/demo/angular2/service/blog/BlogServiceImpl.java" target="_blank">Service</a>:

``` java
@Service
public class BlogServiceImpl implements BlogService {

    @Autowired
    ArticleRepository articleRepository;

    @Override
    @Transactional(readOnly = true)
    public List<Article> getArticles() {

        List<Article> articleList = new ArrayList<>();

        // lambda expression
        // findAll() retrieve an Iterator

        // method reference ...
        articleRepository.findAll().forEach(articleList::add);
        // ... it is equivalent to
        // articleRepository.findAll().forEach(article -&gt; articleList.add(article));

        return articleList;

    }
```

Interesting is the Iterable object returned by the _CrudRepository_ for the _findAll()_ method.
  
It could create a surprise to who has the habit to receive a List of objects.
  
The iterator can easily be transformed in a list using the lambda expression _forEach_ combined with the <a href="https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html" target="_blank">method reference</a> ::add

To test the code <a href="https://github.com/marco76/SpringAngular2TypeScript/blob/feature/database/server/src/test/java/ch/javaee/demo/angular2/service/blog/BlogServiceTest.java" target="_blank">BlogServiceTest.java</a>:

To fill the database for the test it&#8217;s enough to create 2 files in your resources (_src/test/resources_) directory.
  
Spring looks for those files when it starts.
  
With <a href="https://github.com/marco76/SpringAngular2TypeScript/blob/feature/database/server/src/test/resources/schema.sql" target="_blank">schema.sql</a> Spring creates the database:

``` sql
CREATE TABLE ARTICLE (ID INT PRIMARY KEY auto_increment, TITLE VARCHAR2(100), CONTENT TEXT)
```

And with <a href="https://github.com/marco76/SpringAngular2TypeScript/blob/feature/database/server/src/test/resources/schema.sql" target="_blank">import.sql</a> Spring inserts the data:

``` sql

INSERT INTO ARTICLE(TITLE, CONTENT) VALUES ('Article Example', 'Hello World from Java and H2');
INSERT INTO ARTICLE(TITLE, CONTENT) VALUES ('Blog in Angular ?', 'Maybe for fun');

```

```java

@RunWith(SpringRunner.class)
@SpringBootApplication
@EnableJpaRepositories(basePackages = ("ch.javaee.demo.angular2"))
@EntityScan(basePackages = "ch.javaee.demo.angular2")
public class BlogServiceTest {

    @Autowired
    BlogService blogService;

    @Test
    public void getAllArticlesTest() {

        List<Article> articleList = blogService.getArticles();
        assertThat(articleList).isNotNull();

        Article article = articleList.get(0);
        assertThat(article).hasNoNullFieldsOrProperties();
        assertThat(article.getId().equals(1L));
        assertThat(article.getTitle()).isEqualTo("Article Example");

        Article articleTwo = articleList.get(1);
        assertThat(articleTwo).hasNoNullFieldsOrProperties();
        assertThat(articleTwo.getId().equals(2L));
        assertThat(articleTwo.getContent()).isEqualTo("Maybe for fun");
    }
}
```

I had few problems to find the correct annotations combination. **@SpringBootTest** tried to load the WebController ending in error.

In conclusion with:
  
- 1 Service
- 1 'Empty' Repository
- 1 Entity
- 2 SQL files

You have your database. No configuration is needed because Spring Boot automatically recognise and configure H2.