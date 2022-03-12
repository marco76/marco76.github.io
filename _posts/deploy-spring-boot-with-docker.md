# How to deploy Spring Boot with Docker

## Dockerfile

For our image we use the [alpine](https://alpinelinux.org/) linux listribution. This is a minimalistic distribution ideal for our deployment.

We copy the jar file to the /opt directory as suggested by the [FHS](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)

```
## alpine linux with JRE
FROM openjdk:8-jre-alpine

## copy the spring jar
COPY target/*.jar /opt/myApp.jar

CMD ["/usr/bin/java", "-jar", "/opt/myApp.jar"]
```

To have a runnable jar you need to repackage the default jar.

Here you can find the official documentation:
[spring-boot:repackage](https://docs.spring.io/spring-boot/docs/current/maven-plugin/repackage-mojo.html)

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
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
```

## Build and run

You can build your Dockerfile with e.g.:

`docker build -t spring-example .`

and run it with:

`docker run -it -p 8080:8080 spring-example`

## Error without repackaging
If you get this error `no main manifest attribute, in [name of your app]` something went wrong with the `spring-boot-maven-plugin`. 

Verifiy that it is correctly configured.

## Alpine image size

The alpine image size is smaller compared to other distributions.

The version with JRE (8-jre-alpine) is 82MB, the JDK's version is 102MB.