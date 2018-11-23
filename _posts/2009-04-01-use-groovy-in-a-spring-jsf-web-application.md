---
id: 55
title: Use Groovy has Spring bean in a JSF web application
date: 2009-04-01T08:25:43+00:00
author: Marco Molteni
layout: post
guid: http://molteni.wordpress.com/?p=55
permalink: /2009/04/01/use-groovy-in-a-spring-jsf-web-application/
categories:
  - Groovy
  - java
  - Javaserver Faces
  - JSF
  - Spring
---
It&#8217;s easy and powerful use Groovy classes in your Java web application using the Spring Framework. Here you can find a tutorial.

In my last project I had to scan a complex xml file and copy the information to a database. A Java class (using org.wc3.dom.* and DocumentBuilder) did the work but the hundreds lines of code were complex and not easy to maintain.

Ex.

<pre class="brush: java; title: ; notranslate" title="">DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
DocumentBuilder        docBuilder        = docBuilderFactory.newDocumentBuilder();
doc = docBuilder.parse(fileName);
doc.getDocumentElement().normalize();
NodeList listOfEntries = doc.getElementsByTagName("entry");
for (int s = 0; s &lt; listOfEntries.getLength(); s++) {
  Node firstEntityNode = listOfEntries.item(s);
  if (firstEntityNode.getNodeType() == Node.ELEMENT_NODE) {
  ListEntry entry              = new ListEntry();
  Element   firstEntityElement = (Element) firstEntityNode;
  NodeList  uidList            = firstEntityElement.getElementsByTagName("uid");
  ...
&#91;/sourcecode&#93;

For this reason I decided to integrate groovy in the Spring-JSF application, Groovy has the XMLSlurper class that allows to easily access an XML file like if it is a collection of classes.

You can see &lt;a href="http://www.theregister.co.uk/2008/01/11/groovy_xml_part_two/" target="_blank"&gt;here&lt;/a&gt; a tutorial for SMLSlurper.

In my application the role of the first Groovy class were the following:
&lt;ul&gt;
	&lt;li&gt;accept 2 parameters from a traditional java class (xml file address and an integer parameter)&lt;/li&gt;
	&lt;li&gt;process the xml file&lt;/li&gt;
	&lt;li&gt;use 2 existing Java classes&lt;/li&gt;
	&lt;li&gt;return an ArrayList of custom objects with the values found in the list&lt;/li&gt;
&lt;/ul&gt;
&lt;strong&gt;Tasks:&lt;/strong&gt;

&lt;strong&gt;1. add the groovy jar to the libraries of your application&lt;/strong&gt;

&lt;strong&gt;2. create a traditional Java Interface&lt;/strong&gt;


package ch.genidea.checknames.importer;
import java.util.List;

import ch.genidea.checknames.model.ListEntry;
import ch.genidea.checknames.model.SourceList;

public interface Parser {
    List parse();
    void setFilename(String filename);
    void setSourceList(SourceList sourceList);
}
</pre>

I had to declare the setters for the 2 variables to import in the groovy class.

**3. create the groovy class in the classpath**. If you are using maven add it to the src/main/resources directory.

Ex.

<pre class="brush: java; title: ; notranslate" title="">package ch.genidea.checknames.importer

import java.util.List;
import ch.genidea.checknames.model.ListEntry;
import ch.genidea.checknames.model.SourceList;

public class ParserImpl implements ch.genidea.checknames.importer.Parser{
    String filename
    List &lt;ListEntry&gt; result
    SourceList sourceList

    public List&lt;ListEntry&gt; parse(){
        def pers=new XmlSlurper().parse(new File(filename))

        List &lt;ListEntry&gt; result = new ArrayList&lt;ListEntry&gt;()
        def allEntry = pers.Entry

        allEntry.each{
                 result.add(addEntry(it))
                 it.akaList.aka.each{
                 result.add(addEntry(it))
                 }
        }
        return result
    }
    ListEntry addEntry(def it)
    {
            ListEntry entity = new ListEntry()
             entity.uid = (it.uid.text() as Integer)
             entity.sourceList=sourceList
             entity.familyName = it.lastName.text()
             entity.firstName = it.firstName.text()
             return entity
    }

    void setFilename(String filename)
    {
        this.filename = filename
    }
    void setSourceList(SourceList sourceList)
    {
        this.sourceList = sourceList
    }

}

</pre>

In few lines the class did the same work of hundreds of lines of code of the previous implementation 🙂

In this groovy class we use ListEntry and SourceList that are traditional java classes and we return a List<ListEntry> object

**4. Declare the bean in Spring**

<pre class="brush: xml; title: ; notranslate" title="">&lt;beans xmlns="http://www.springframework.org/schema/beans"
xmlns:lang="http://www.springframework.org/schema/lang"
 ...
http://www.springframework.org/schema/lang http://www.springframework.org/schema/lang/spring-lang-2.5.xsd"
...
&gt;
&lt;lang:groovy id="parser" script-source="classpath:ch/genidea/checknames/importer/ParserImpl.groovy" /&gt;

&lt;bean name = "listImportService" class="ch.genidea.checknames.lists.service.ListImportServiceImpl"&gt;
&lt;property name="parser" ref="parser"&gt;&lt;/property&gt;
&lt;/bean&gt;

 </pre>

**5. use the groovy class in your sourcecode 🙂**

<pre class="brush: java; title: ; notranslate" title="">parser.setFilename(fileSource);
parser.setSourceList(sl);
List&lt;ListEntry&gt; list = parser.parse();
</pre>

The only drawbacks I had are:

  * with Eclipse is not easy to debug groovy, but I used the groovyconsole to test rapidly the changes
  * 4MB added using the groovy-all.jar

Small issues compared to the great benefits of having less and more readable codeeasy a