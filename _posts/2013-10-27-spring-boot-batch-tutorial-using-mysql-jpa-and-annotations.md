---
title: 'Spring Boot : Batch tutorial using MySQL, JPA and annotations'
date: 2013-10-27T17:32:51+00:00
author: Marco Molteni
layout: post
permalink: /2013/10/27/spring-boot-batch-tutorial-using-mysql-jpa-and-annotations/
dsq_thread_id:
  - "5565997809"
categories:
  - Hibernate
  - java
  - JPA
  - Spring
  - Spring Batch
  - Spring Boot
tags:
  - java
  - JPA
  - Spring Batch
  - spring boot
---
In this example I show how to use Spring Boot to create a simple Spring Batch application.
  
I used the same architecture in a production software. Spring Boot is great to improve productivity but the documentation is still sparse (the framework is still in 0.5 version).

You can find the complete example here:
  
<http://github.com/marco76/springBootBatch>

The only requirement is to have a mysql database installed.
  
You can find more information in the source code.

If you start a new project in an IDE, you can simply create a simple new maven project (without Spring wizards or similar).

## Spring Boot

Spring boot offers to the developer some great feature to improve his productivity:

  * bootable jar even for webapps
  * auto-import of the required libraries
  * autoconfiguration (convention over configuration)

The main difficulties I encountered:

  * the configuration by annotation is still &#8216;immature&#8217; in Spring (ex. cli parameters issue)
  * the auto configuration hides some parameters to the developer and, because of this, there are some surprising results

## Example

For the example we load a fixed line flat file in a MySql database.
  
Here the content of the file:
[<img src="{{site.baseurl}}/assets/img/uploads/2013/10/spring_boot_personData.png"/>]({{site.baseurl}}/assets/img/uploads/2013/10/spring_boot_personData.png)

Here the result in the DB:
[<img src="{{site.baseurl}}/assets/img/uploads/2013/10/spring_boot_mysqlresult.png"/>]({{site.baseurl}}/assets/img/uploads/2013/10/spring_boot_mysqlresult.png)

Directory structure:

[<img src="{{site.baseurl}}/assets/img/uploads/2013/10/spring_boot_classes.png"/>]({{site.baseurl}}/assets/img/uploads/2013/10/spring_boot_classes.png)

### pom.xml

We have to declare a parent, this is mandatory.
  
The spring-boot-starter-parent has a reference to all the boot modules:

``` xml

<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>0.5.0.M5</version>
</parent>

```

Because we use Spring Batch, JPA and Mysql we declare the following dependencies:

``` xml
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-batch</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.26</version>
        </dependency>
    </dependencies>
```

### Input flat file and FixedLengthTokenizer

The input fixed length flat file defines has this structure:
  - Columns 1-30: first name
  - Columns 31-60: family name
  - Columns >61: year

To read correctly tho file we created a PersonFixedLengthTokenizer:

``` java
 
RangeArrayPropertyEditor range = new RangeArrayPropertyEditor();
// we defines how to split the text line, from column 1 to 30 assign the content to 'firstName'
range.setAsText("1-30,31-60,61-");
// names have to be the same as the properties in the model class
setNames(new String[]{"firstName", "familyName", "year"});
setColumns((Range[]) range.getValue());

```

The tokenizer assigns the 30 chars at the beginning of the line to the &#8216;firstName&#8217; attribute, the chars from 31 to 60 to &#8216;familyName&#8217; and so on. Because we passed the Person class to the ItemReader, the tokenizer fill automatically the Person attributes with the attributes found in the line.

### import.sql

We created an import.sql script to avoid a current limitation of Spring Batch that doesn&#8217;t work well in reading parameters passed to the CommandLineJobRunner.

``` sql

drop table BatchDB.Batch_JOB_EXECUTION_CONTEXT;
drop table BatchDB.Batch_JOB_EXECUTION_PARAMS;
drop table BatchDB.Batch_JOB_EXECUTION_SEQ;
drop table BatchDB.Batch_JOB_SEQ;
drop table BatchDB.Batch_STEP_EXECUTION_CONTEXT;
drop table BatchDB.Batch_STEP_EXECUTION_SEQ;
drop table BatchDB.Batch_STEP_EXECUTION;
drop table BatchDB.Batch_JOB_EXECUTION;
drop table BatchDB.Batch_JOB_INSTANCE;

```
The script (automatically found by Hibernate) delete all the tables created by Spring to update the batch status allowing us to restart the batch without using the -next parameter avoiding the following error:

Caused by: org.springframework.batch.core.repository.JobInstanceAlreadyCompleteException: A job instance already exists and is complete for parameters={}.  If you want to run this job again, change the parameters.
``` xml
Caused by: org.springframework.batch.core.repository.JobInstanceAlreadyCompleteException: A job instance already exists and is complete for parameters={}.  If you want to run this job again, change the parameters.
```

### application.properties

Here we define the database parameters.
  
Very important in our example, we have to define:

``` xml
spring.jpa.hibernate.ddl-auto=create
spring.jpa.show-sql=false
```

If ddl-auto is not defined by the developer then Spring will use by default &#8216;create-drop&#8217; deleting the tables at the end of the batch.

### Application.java

This class simply call the BatchConfiguration.class containing the batch configuration.

```java

import org.springframework.boot.SpringApplication;

public class Application {
    public static void main(String args[]) {
        SpringApplication.run(BatchConfiguration.class, args);
    }
}

```

### BatchConfiguration.java

The BatchConfiguration class defines the basic beans to execute the batch.
  
The annotation

``` java
@EnableAutoConfiguration`
```


is specific to Spring Boot, you can read more about this annotation here:[Spring Boot auto configure](http://github.com/spring-projects/spring-boot/tree/97cb7f096798ecd016de71f892fa55585d45f5eb/spring-boot-autoconfigure#spring-boot---autoconfigure)

``` java
@Configuration
@EnableBatchProcessing
@ComponentScan
//spring boot configuration
@EnableAutoConfiguration
// file that contains the properties
@PropertySource("classpath:application.properties")
public class BatchConfiguration {

    /**
     *Load the properties
     */
    @Value("${database.driver}")
    private String databaseDriver;
    @Value("${database.url}")
    private String databaseUrl;
    @Value("${database.username}")
    private String databaseUsername;
    @Value("${database.password}")
    private String databasePassword;

    /**
     * We define a bean that read each line of the input file.
     */
    @Bean
    public ItemReader reader() {
        // we read a flat file that will be used to fill a Person object
        FlatFileItemReader reader = new FlatFileItemReader();
        // we pass as parameter the flat file directory
        reader.setResource(new ClassPathResource("PersonData.txt"));
        // we use a default line mapper to assign the content of each line to the Person object
        reader.setLineMapper(new DefaultLineMapper() {
            // we use a custom fixed line tokenizer
            setLineTokenizer(new PersonFixedLineTokenizer());
            // as field mapper we use the name of the fields in the Person class
            setFieldSetMapper(new BeanWrapperFieldSetMapper() {
                setTargetType(Person.class); // we create an object Person
            });
        });
        return reader;
    }

    /**
     * The ItemProcessor is called after a new line is read and it allows the developer
     * to transform the data read
     * In our example it simply return the original object
     */
    @Bean
    public ItemProcessor<Person, Person> processor() {
        return new PersonItemProcessor();
    }

    /**
     * Nothing special here a simple JpaItemWriter
     */
    @Bean
    public ItemWriter writer() {
        JpaItemWriter writer = new JpaItemWriter();
        writer.setEntityManagerFactory(entityManagerFactory().getObject());

        return writer;
    }

    /**
     * This method declare the steps that the batch has to follow
     */
    @Bean
    public Job importPerson(JobBuilderFactory jobs, Step s1) {

        return jobs.get("import")
                .incrementer(new RunIdIncrementer()) // because a spring config bug, this incrementer is not really useful
                .flow(s1)
                .end()
                .build();
    }

    /**
     * Step
     * We declare that every 1000 lines processed the data has to be committed
     */

    @Bean
    public Step step1(StepBuilderFactory stepBuilderFactory, ItemReader reader,
                      ItemWriter writer, ItemProcessor<Person, Person> processor) {
        return stepBuilderFactory.get("step1")
                .<Person, Person>chunk(1000)
                .reader(reader)
                .processor(processor)
                .writer(writer)
                .build();
    }

    /**
     * As data source we use an external database
     *
     * @return
     */

    @Bean
    public DataSource dataSource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName(databaseDriver);
        dataSource.setUrl(databaseUrl);
        dataSource.setUsername(databaseUsername);
        dataSource.setPassword(databasePassword);
        return dataSource;
    }

    @Bean
    public LocalContainerEntityManagerFactoryBean entityManagerFactory() {

        LocalContainerEntityManagerFactoryBean lef = new LocalContainerEntityManagerFactoryBean();
    [...]

return lef;
}
`
@Bean
public JpaVendorAdapter jpaVendorAdapter() {
    HibernateJpaVendorAdapter jpaVendorAdapter = new HibernateJpaVendorAdapter();
    [...]
    return jpaVendorAdapter;
}

```