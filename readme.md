# Purpose
> This workshop will focus on own create Rest Services with spring boot and gradle from scratch
---
# Requirements
- install JDK 8
- install Gradle 3.5
### Install gradle
* unzip in c:\dev the folder gradle-3.5 from the zip file
* add system variable GRADLE_HOME=c:\dev\gradle-3.5
* add to your PATH system variable %GRADLE_HOME%\bin
* test your install from the command line
    ```sh
    $ gradle -v
    ------------------------------------------------------------
    Gradle 3.5
    ------------------------------------------------------------
    ...
    ```
---
# Create your application tree
### From windows command line
```sh
c:\>mkdir \workspaces\otw\hello\src\main\java\com\open\hello
```
> If the command mkdir not working use
    ```
    c:\>setlocal enableextensions
    ```
---
# Create a simple web application
Now you can create a web controller for a simple web application.
> src/main/java/hello/HelloController.java
```java
package hello;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;

@RestController
public class HelloController {

    @RequestMapping("/")
    public String index() {
        return "Greetings from Open Technical Workshop!";
    }

}
```
# Create an Application class
Here you create an Application class with the components:
> src/main/java/hello/Application.java
```java
package hello;

import java.util.Arrays;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public CommandLineRunner commandLineRunner(ApplicationContext ctx) {
        return args -> {

            System.out.println("Let's inspect the beans provided by Spring Boot:");

            String[] beanNames = ctx.getBeanDefinitionNames();
            Arrays.sort(beanNames);
            for (String beanName : beanNames) {
                System.out.println(beanName);
            }

        };
    }

}
```
---
# Build your application and run it
### create your build file
> root directory of your application
> \gradle.build
```gradle
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:1.5.2.RELEASE")
    }
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'idea'
apply plugin: 'org.springframework.boot'

jar {
    baseName = 'hello'
    version =  '0.1.0'
}

repositories {
    mavenCentral()
}

sourceCompatibility = 1.8
targetCompatibility = 1.8

dependencies {
    // tag::jetty[]
    compile("org.springframework.boot:spring-boot-starter-web") {
        exclude module: "spring-boot-starter-tomcat"
    }
    compile("org.springframework.boot:spring-boot-starter-jetty")
    // end::jetty[]
    // tag::actuator[]
    compile("org.springframework.boot:spring-boot-starter-actuator")
    // end::actuator[]
    testCompile("junit:junit")
}
```
### Test your build
```sh
c:\>gradle build
```
### Install the gradle warper
```sh
c:\>gradle wrapper --gradle-version 3.5
```
### Build with wrapper and run application
```sh
c:\>gradlew build && java -jar build/libs/hello-0.1.0.jar
```
# References
https://spring.io/guides/gs/spring-boot/#scratch
https://spring.io/guides/gs/gradle/
	
# More
install docker?