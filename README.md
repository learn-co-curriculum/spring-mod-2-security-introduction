# Spring Security Introduction

## Learning Goals

- Explain why security is important.
- Set up a project base for the next lessons.

## Why Security?

So far, the APIs we have stood up are open to the entire world to access. That
is, once we deploy the application on a system that is exposed to the internet,
anyone with access to the internet would be able to access our service. This is
desirable in some cases (many APIs are public by nature), but there are also a
lot of scenarios in which we might want to control access our APIs.

When we want to control access to our APIs, there are 2 fundamental ways in
which this is achieved:

1. **Authentication**: This is the process of validating that a user is who
   they claim to be.
2. **Authorization**: this is the process of validating that a user is
   allowed to perform the specific action they are trying to perform.

We can implement authentication without authorization. That basically means all
users in the system have the same rights, and all we care about is that we are
able to recognize who they are (i.e. they authenticate themselves).

However, we cannot implement authorization without authentication, because
we have to validate that a user is who they say they are before we can actually
worry about whether they're allowed to perform the specific action they are
trying to perform.

Let's look at how to implement both Authentication (AuthN) and Authorization
(AuthZ).

## Code Along: Create a Spring Security Project

Let's create a new Spring project to demonstrate the Spring security features.

1. Go to [https://start.spring.io/](https://start.spring.io/).
2. Select the project properties.
    1. Select "Maven Project", as we will use Maven as the build tool.
    2. Select "Java" as the language.
    3. Select the most recent version of Spring Boot 2. (Make sure it does not
       have "SNAPSHOT" listed after it.)
    4. Change the "Artifact" metadata field to "spring-security-demo". (This
       will update the "Name" and "Package name" metadata fields too).
    5. Change the "Description" metadata field to "Demo for Spring Security".
    6. Select the appropriate Java JDK version.
3. Add dependencies.
    1. Click "ADD DEPENDENCIES".
    2. Search for "spring web".
    3. Select "Spring Web" from the list.
    4. Click on "ADD DEPENDENCIES" again and add  "Lombok" from the list.
    5. Click on "ADD DEPENDENCIES" again and add "Spring Security" from the list.
4. Click on the "Generate" button on the bottom. This will download a zip file
   containing the Spring Boot Project.
5. Unzip the archive and open it in IntelliJ or a preferred code editor.

![spring-security-project-initializr](https://curriculum-content.s3.amazonaws.com/spring-mod-2/security2/spring-initialzr-security-project.png)

Once you have created the new project, let's create a few packages and classes.

Create a package called `controller` and `config` under the
`com.example.springsecuritydemo` directory. In the `controller` directory,
create a Java class called `DemoController`. In the `config` directory, create
a Java class called `SecurityConfiguration`.

Consider the following project structure and ensure yours looks the same:

```text
├── HELP.md
├── mvnw
├── mvnw.cmd
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── example
    │   │           └── springsecuritydemo
    │   │               ├── SpringSecurityDemoApplication.java
    │   │               ├── config
    │   │               │   └── SecurityConfiguration.java
    │   │               └── controller
    │   │                   └── DemoController.java
    │   └── resources
    │       ├── application.properties
    │       ├── static
    │       └── templates
    └── test
        └── java
            └── org
                └── example
                    └── springsecuritydemo
                        └── SpringSecurityDemoApplicationTests.java
```

Once the project structure is set up, add the following code to the
`DemoController` class:

```java
package com.example.springsecuritydemo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class DemoController {

    @GetMapping("/hello")
    public String hello(@RequestParam(defaultValue = "person") String name) {
        return String.format("Hello %s", name);
    }
}
```

## Running the Application

As we can see, we've created a very simple Spring Boot application with only one
endpoint defined in our controller class. Let's go ahead and run the application
now:

![user-generated-password](https://curriculum-content.s3.amazonaws.com/spring-mod-2/security/application-user-generated-security-password-intellij.png)

Now that we have included the Spring Security dependency in our project, Spring
Security has auto-generated a user password for us. By default, all the
endpoints are now protected under Spring Security.

As noted in the console message, the auto-generated password that has been given
to us, is for development and testing purposes only. Later on, we'll learn how to
configure this for an application running in production.

Since we only have one endpoint, let's open up Postman and try to send the GET
request. Make sure the headers include "Content-Type: application/json" before
sending the request as well:

![401-unauthorized-attempt](https://curriculum-content.s3.amazonaws.com/spring-mod-2/security/unauthorized-postman.PNG)

Notice what we get back is a "401 Unauthorized" status message. This is because
we are not authenticated and our endpoint, by default with the Spring Security
dependency, is now secured.

So how can we reach our endpoint now?

In Postman, navigate to the "Authorization" tab. Once there, choose "Basic Auth"
from the "Type" dropdown:

![basic-auth](https://curriculum-content.s3.amazonaws.com/spring-mod-2/security/postman-basic-auth.PNG)

When we select "Basic Auth", we will see a prompt for a username and password
off to the right. The default username is "user" and then we'll make use of the
auto-generated password that Spring provided us with in the console.

![basic-auth-credentials](https://curriculum-content.s3.amazonaws.com/spring-mod-2/security/postman-basic-auth-credentials.PNG)

It should be noted that the password here in this lesson is different from your
password if you are following along with this lesson. This is because each
password is unique and will change with every time we restart the application.
In the screenshot above, we have selected "Show Password" just to demonstrate
that the password is the same as the auto-generated password we saw in our
console earlier. You do not have to have this box checked - as this was for
demonstration purposes.

By performing these steps here, we are now able to pass a set of credentials
along with our GET request to say that we are authenticated and now authorized
to view the endpoint. Let's send our GET request again and see what happens:

![hit-hello-endpoint](https://curriculum-content.s3.amazonaws.com/spring-mod-2/security/postman-hello-person.PNG)

This process of authentication is known as **HTTP Basic**.

## References

- [Spring Security Fundamentals - Lesson 1 - First Steps](https://www.youtube.com/watch?v=nSu9ElsnNtY&list=PLEocw3gLFc8X_a8hGWGaBnSkPFJmbb8QP)
