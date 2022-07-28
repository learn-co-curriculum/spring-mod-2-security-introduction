# Base API Implementation

## Learning Goals

- Explain why security is important.
- Set up a project base for the next lessons.

## Why Security?

The sample service we've stood up so far is simplistic in 2 ways:

1. It returns a single string with very little processing. In real life, each
   service will be responsible for more business logic (functionality) and will
   return its results in some type of structured format, most likely JSON (
   JavaScript Object Notation) or XML.
2. It is open to the entire world to see access. That is, once it's deployed on
   a system that is exposed to the internet, anyone with access to the internet
   would be able to access our service. This is desirable in some cases (many
   APIs are public by nature), but there are also a lot of scenarios in which we
   might want to control access our APIs.

When we want to control access to our APIs, there are 2 fundamental ways in
which this is achieved:

1. Authentication: this is the process of validating that you are who you say
   you are
2. Authorization: this is the process of validating that your user is authorized
   to perform the specific action they are trying to perform

You can implement authentication without authorization: it basically means all
users in your system have the same rights, and all you care about is that you're
able to recognize who they are (i.e. they authenticate themselves).

You cannot implement authorization without authentication, because you have to
validate that a user is who they say they are before you can actually worry
about whether they're allowed to perform the specific action they are trying to
perform.

Let's look at how to implement both Authentication (AuthN) and Authorization
(AuthZ)

## Base API Implementation

Let's revisit our sample API implementation from the previous section.

Here is the controller:

```java
package com.flatiron.spring.FlatironSpring;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello(@RequestParam(name = "targetName", defaultValue = "Stephanie") String name) {
        return String.format("Hello %s", name);
    }

}
```

Here is the custom security configuration we put in place to work around the
default login functionality that Spring Boot puts in place (depending on the
version of Spring Boot you're using):

```java
package com.flatiron.spring.FlatironSpring;

import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
      http.authorizeRequests()
              .anyRequest().permitAll();
    }
}
```

Let's take a close look at what this `SecurityConfiguration` class does:

- The `@Configuration` and `@EnableWebSecurity` annotations tell the Spring
  Framework that this class should be used for configuring the security of this
  Spring application
- The `'configure()` method:
  - Takes an `HttpSecurity` object as a parameter
  - Calls the `authorizeRequests()` method to specify how different requests
    must be authorized

## Adding Basic Security

Let's remove the `configure()` method from the `SecurityConfiguration` class:

```java
package com.flatiron.spring.FlatironSpring;

import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {
}
```

That will put the default Spring Framework security implementation into effect
again. Let's have a look at what that does to our endpoint:

![Spring Framework default simple authentication](https://curriculum-content.s3.amazonaws.com/java-spring-2/spring-testing-simple-auth.png)

Note: if the default Spring Framework security is not in place for your sample
project, you may need to add spring security to your classpath explicitly, which
can be done as follows:

- Gradle: Add the following dependencies to your `build.gradle` file:

```json
dependencies {
        // ...
	implementation 'org.springframework.boot:spring-boot-starter-security'
}
```

- Maven: Add the following dependencies to your `pom.xml` file:

```xml
<dependencies>
	<!-- ... -->
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-security</artifactId>
	</dependency>
</dependencies>
```

Now that we have the default implementation in place, let's look at ways to
customize it in the next section.
