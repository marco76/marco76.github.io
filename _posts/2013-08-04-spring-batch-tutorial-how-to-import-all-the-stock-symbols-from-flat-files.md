---
id: 411
title: 'Spring Batch tutorial: how to import all the stock symbols from flat files'
date: 2013-08-04T17:53:06+00:00
author: Marco Molteni
layout: post
guid: http://javaee.ch/?p=411
permalink: /2013/08/04/spring-batch-tutorial-how-to-import-all-the-stock-symbols-from-flat-files/
categories:
  - Uncategorized
tags:
  - Equity Symbols
  - java
  - Spring Batch
  - Stock symbols
---
This tutorial shows how to implement a Spring Batch to load all the stock symbols in a database.

Here you can find the github project: <https://github.com/marco76/SpringBatchEquity>

In the project I&#8217;ve included the files containing the list of symbols
  

  
<img alt="Batch1" src="https://i2.wp.com/javaee.ch/wp-content/uploads/2013/08/batchEquitiesbatch1.png?resize=182%2C103" border="0" data-recalc-dims="1" />

You can download the updated version of these files from the [nasdaq.com](http://www.nasdaq.com/screening/company-list.aspx) web site.

Example of the content:
  
<img alt="Batch2" src="https://i1.wp.com/javaee.ch/wp-content/uploads/2013/08/batchEquitiesbatch2.png?resize=600%2C37" border="0" data-recalc-dims="1" />

The goal of the tutorial is to read the files and transform the content in Java Objects to save in a MySQL database.

In spring-batch.xml we define how the batch has to work:
  
1. Load multiple files

SpringBatch easily reads data from more than 1 file. You have to implement a MultiResourceItemReader. The reading action is delegated to symbolItemReader:

<pre class="brush: xml; title: ; notranslate" title="">&lt;bean id="multiResourceReader" class="org.springframework.batch.item.file.MultiResourceItemReader"&gt;
        &lt;property name="resources" value="classpath*:/*.csv"/&gt;
        &lt;property name="delegate" ref="symbolItemReader"/&gt;
&lt;/bean&gt;
</pre>

<pre class="brush: xml; title: ; notranslate" title="">&lt;!-- spring already has a standard flat file reader --&gt;
    &lt;bean id = "symbolItemReader" class="org.springframework.batch.item.file.FlatFileItemReader"&gt;
        &lt;!-- the name of the file in the resources directory that contains the symbols --&gt;

        &lt;!-- the first line contains the name of the columns, skip it --&gt;
        &lt;property name="linesToSkip" value="1"/&gt;
        &lt;!-- we use a default line mapper, each line is transformed in a predefined bean --&gt;
        &lt;property name="lineMapper"&gt;
            &lt;bean class = "org.springframework.batch.item.file.mapping.DefaultLineMapper"&gt;
                &lt;!-- map of the fields --&gt;
                &lt;property name="fieldSetMapper" ref="symbolFieldSetMapper"/&gt;
                &lt;property name="lineTokenizer" ref="symbolLineTokenizer"/&gt;
            &lt;/bean&gt;
       &lt;/property&gt;
    &lt;/bean&gt;
</pre>

symbolItemReader uses the already existing FlatFileItemReader. We tells to the ItemReader that the first line contains the titles and has not to be read (linesToSkip=1).

_lineMapper_ defines how the file lines have to be transformed.
  
_lineTokenizer_ defines what is the content of the file. In this case we simply copy the name of the columns as defined in the file (you can change it) and we define as delimiter the &#8220;,&#8221;:

<pre class="brush: xml; title: ; notranslate" title="">&lt;!-- here we can define which is the delimiter of the values in the file and the name of the columns --&gt;
    &lt;!-- the names of the columns can change from the original names in the file --&gt;
    &lt;bean id = "symbolLineTokenizer" class="org.springframework.batch.item.file.transform.DelimitedLineTokenizer"&gt;
        &lt;property name="delimiter" value=","/&gt;
        &lt;property name="names" value="Symbol, Name, LastSale, MarketCap, ADR TSO, IPOyear, Sector, Industry, Summary Quote, empty"/&gt;
        
    &lt;/bean&gt;

</pre>

_fieldMapper_ copy the content of each line of the file and convert it in a java object. The columns are the same declared in the lineTokenizer:

<pre class="brush: java; title: ; notranslate" title="">public class EquityFieldSetMapper implements FieldSetMapper {

    @Override
    public Equity mapFieldSet(FieldSet fieldSet) throws BindException {
        Equity equityImporter = new Equity();
        equityImporter.setSymbol(fieldSet.readString("Symbol"));
        equityImporter.setName(fieldSet.readString("Name"));
        equityImporter.setIpoYear(fieldSet.readString("IPOyear"));
        equityImporter.setSector(fieldSet.readString("Sector"));
        equityImporter.setIndustry(fieldSet.readString("Industry"));
        equityImporter.setSummaryQuote(fieldSet.readString("Summary Quote"));

        return equityImporter;
    }
}
</pre>

To write to the database we can use JpaItemWriter:

<pre class="brush: xml; title: ; notranslate" title="">&lt;bean id="jpaBatchWriter" class = "org.springframework.batch.item.database.JpaItemWriter"&gt;
        &lt;property name="entityManagerFactory" ref="entityManagerFactory"/&gt;
    &lt;/bean&gt;
</pre>

We declared how where to read, how to read and how to write. We need to connect all the pieces together in a step.
  
We had to define an itemProcessor (needed to transform the data) but it doesn&#8217;t do anything, it&#8217;s just a requirement.
  
The _transactionManager_ has nothing special, here it&#8217;s important to note the _commitInterval_ that defines the frequency of the database commits.

<pre class="brush: xml; title: ; notranslate" title="">&lt;!-- here we glue together all the pieces --&gt;
    &lt;bean id = "simpleStep" class="org.springframework.batch.core.step.factory.SimpleStepFactoryBean"&gt;
        &lt;property name="transactionManager" ref="transactionManager"/&gt;
        &lt;property name="jobRepository"  ref="jobRepository"/&gt;
        &lt;property name="itemReader" ref="multiResourceReader"/&gt;
        &lt;property name="itemProcessor"  ref="simpleProcessor"/&gt;
        &lt;property name="itemWriter" ref="jpaBatchWriter"/&gt;
        &lt;property name="commitInterval" value="10"/&gt;
    &lt;/bean&gt;
</pre>

The step has to be part of a Job:

<pre class="brush: xml; title: ; notranslate" title="">&lt;bean id="importJob" parent="simpleJob"&gt;
        &lt;property name="steps"&gt;
            &lt;list&gt;
                &lt;ref bean="simpleStep"/&gt;
            &lt;/list&gt;
        &lt;/property&gt;
    &lt;/bean&gt;
</pre>

You can launch the job from your Java code:

<pre class="brush: java; title: ; notranslate" title="">@Autowired
    @Qualifier(value = "importJob")
    Job job;

    @Test
    public void testJob() throws Exception{
         JobParametersBuilder builder = new JobParametersBuilder();
        JobExecution jobExecution = jobLauncher.run(job, builder.toJobParameters());
        assertEquals("COMPLETED", jobExecution.getExitStatus().getExitCode());
        }
</pre>