# Deploy your Angular application in a Java artifact

For new projects I suggest that you separate your Angular application from your backend artifact (.war, .jar or different).

If for some reasons (legacy, architecture etc.) you can only deploy a single artifact here you can find a solution that I adopted for the first version of this website.


## One WAR with Spring Boot or Java EE
Your project contains 3 modules, the parent and 2 children (frontend and backend).

In this example we use Spring Boot, the same applys for a Java EE project.

### parent pom.xml

The parent pom simply declare the 2 modules to build. We import the maven-war-plugin to generate the final war.

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>spring-ng-demo</groupId>
    <artifactId>parent</artifactId>
    <packaging>pom</packaging>
    <version>0.0.5-SNAPSHOT</version>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.0.RELEASE</version>
    </parent>
    <modules>
        <module>frontend</module>
        <module>backend</module>
    </modules>
    <build>
    <pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.2.0</version>
            </plugin>
        </plugins>
    </pluginManagement>
    </build>

    <repositories>
    [...]
    </repositories>
</project>
```

## Frontend pom.xml

The frontend module contains only TypeScript code in a `./src` directory.
Before to execute the code you need to install `npm` and `@angular/cli` (if you are using CLI).


With the `exec-maven-plugin` we install the npm's dependencies and we build the final javascript application.

The webapp is saved in the `./src/build/` directory.

Finally, maven build a `.war` writing the JS files in the `META-INF/resources`.

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>parent</artifactId>
        <groupId>spring-ng-demo</groupId>
        <version>0.0.5-SNAPSHOT</version>

    </parent>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>frontend</artifactId>
    <packaging>war</packaging>
    <build>
        <plugins>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.6.0</version>
                <executions>
                    <execution>
                        <id>install npm</id>
                        <configuration>
                            <workingDirectory>./src/</workingDirectory>
                            <executable>npm</executable>
                            <arguments>
                                <argument>install</argument>
                            </arguments>
                        </configuration>
                        <phase>generate-resources</phase>
                        <goals>
                            <goal>exec</goal>
                        </goals>
                    </execution>
                    <execution>
                        <id>build js</id>
                        <configuration>
                            <workingDirectory>./src</workingDirectory>
                            <executable>ng</executable>
                            <arguments>
                                <argument>build</argument>
                                <argument>--target=production</argument>
                                <argument>--environment=prod</argument>
                                <argument>--extract-css=false</argument>
                            </arguments>
                        </configuration>
                        <phase>generate-resources</phase>
                        <goals>
                            <goal>exec</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.1.0</version>
                <configuration>
                    <webResources>
                        <resource>
                            <directory>src/dist</directory>
                            <targetPath>META-INF/resources</targetPath>
                        </resource>
                    </webResources>
                    <failOnMissingWebXml>false</failOnMissingWebXml>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

## The Java Backend pom.xml

Here a simplified version of the backend pom.xml

You have to import the frontend module, the frontend code will be included in your final package.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>parent</artifactId>
        <groupId>spring-ng-demo</groupId>
        <version>0.0.5-SNAPSHOT</version>
    </parent>
    <packaging>war</packaging>
    <modelVersion>4.0.0</modelVersion>
    
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.build.outputEncoding>UTF-8</project.build.outputEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <failOnMissingWebXml>false</failOnMissingWebXml>
        <demo.version>0.0.4-SNAPSHOT</demo.version>
    </properties>

    <artifactId>backend</artifactId>

    <dependencies>

        <dependency>
            <groupId>spring-ng-demo</groupId>
            <artifactId>frontend</artifactId>
            <version>0.0.5-SNAPSHOT</version>
            <type>war</type>
        </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>1.5.7.RELEASE</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```