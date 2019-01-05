---
title: 'Use PostgreSQL with WildFly'
description: ''
date: 2019-01-04T01:41:48+00:00
author: Marco Molteni
layout: post
main-class: 'javaee'
color: '#7AAB13'
permalink: /use-postgresql-with-wildfly
categories:
  - JavaEE, PostgreSQL
tags:
  - JavaEE, PostgreSQL
 
image: '/assets/img/'

introduction: 'Configure PostgreSQL in WildFly'
---

Tested with WildFly 15 and PostgreSql 11

# Use PostgreSQL with WildFly
## Download the PostgreSQL driver:
Download a compatible drive with your instance:
[https://jdbc.postgresql.org/download.html](https://jdbc.postgresql.org/download.html)
e.g. PostgreSQL JDBC 4.2 Driver, 42.2.5

## Add PostgreSQL to WildFly
In `[WILDFLY_HOME]/modules`
create the directory:
`/org/postgresql/main`
and copy the jdbc file.

## Create the module in WildFly
In `/org/postgresql/main`
create the `module.xml` file

```xml
<?xml version='1.0' encoding='UTF-8'?>

<module xmlns="urn:jboss:module:1.1" name="org.postgresql">

    <resources>
    <!--the name of your driver -->
        <resource-root path="postgresql-42.2.5.jar"/>
    </resources>

    <dependencies>
        <module name="javax.api"/>
        <module name="javax.transaction.api"/>
    </dependencies>
</module>
```
- change the filename according to your jdbc driver

In alternative you can use the Wildfly CLI:
`[WILDFLY_HOME]/bin/jboss-cli.sh`
`module add --name=org.postgresql --resources=[JDBC_FILE_PATH]postgresql-42.2.5.jar --dependencies=javax.api,javax.transaction.api`

## Add the datasource
In our case we use the standalone instance of WildFly.
- Open [WILDFLY_HOME]/standalone/configuration/standalone.xml 
- Locate the existing datasources:

```xml
<subsystem xmlns="urn:jboss:domain:datasources:5.0">
    <datasources>
```

- add the postgresql datasource, update according to your configuration


```xml
<drivers>
    <driver name="postgresql" module="org.postgresql">
    <!-- 1. chose your connection driver -->
        <driver-class>org.postgresql.Driver</driver-class>
    </driver>
</drivers>

<!-- 2. use your configuration -->
<datasource jndi-name="java:/PostgresDS" pool-name="PostgresDS">
    <connection-url>jdbc:postgresql://localhost:5432/YourDatabase</connection-url>
    <driver-class>org.postgresql.Driver</driver-class>
    <driver>org.postgresql</driver>
    <security>
        <user-name>YourUserName</user-name>
        <password>YourPassword</password>
    </security>
```
                    
In the section `<datasources><drivers>` add
```xml
 <driver name="org.postgresql" module="org.postgresql"/>
```
Restart the server and enjoy!

As alternative you can use the WildFly UI:
- add your user to your instance
`[WILDFLY_HOME]/bin/add-user.sh`

Add the Datasource in: `http://127.0.0.1:9990`

## Errors
 
__`WFLYJCA0047: Connection is not valid`__

The inclusion of the `datasource-class` in your configuration e.g.

`<datasource-class>org.postgresql.ds.PGSimpleDataSource</datasource-class>`

could throw the following error: WFLYJCA0047: Connection is not valid

here some references about this problem:
[https://issues.jboss.org/browse/WFLY-6157](https://issues.jboss.org/browse/WFLY-6157)
[https://superuser.com/questions/1371142/wildfly-14-connect-to-remote-postgresql-wflyjca0047-connection-is-not-valid](https://superuser.com/questions/1371142/wildfly-14-connect-to-remote-postgresql-wflyjca0047-connection-is-not-valid)