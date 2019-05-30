---
layout: post
title: Running spring boot app on Jetty server
categories: [gradle]
tags: [springboot, application-server, embedded-server, tomcat, jetty, gradle]
comments: true
---

Gradle configuration to run spring boot app on Jetty server

### Gradle configuration to enable Jetty server

The default embedded application server used by spring boot is Tomcat. The embedded tomcat instanced is included to `spring-boot-starter-web` dependency of a spring boot starter project. To use Jetty server, add `spring-boot-starter-jetty` dependency. Also, exclude `spring-boot-starter-tomcat` dependency from `spring-boot-starter-web`

**build.gradle**

```gradle
buildscript {
    repositories {
        mavenCentral()
    }
}

plugins {
    id 'org.springframework.boot' version '2.1.5.RELEASE'
    id 'java'
    id 'war'
}

apply plugin: 'io.spring.dependency-management'

version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
    mavenCentral()
}

dependencies {
    compile('org.springframework.boot:spring-boot-starter-web') {
        exclude group: 'org.springframework.boot', module: 'spring-boot-starter-tomcat'
    }
    compile('org.springframework.boot:spring-boot-starter-jetty')
    testCompile('org.springframework.boot:spring-boot-starter-test')
}
```

Another way to configure **build.gradle** file

```gradle
...
configurations {
	providedRuntime
	compile.exclude module: "spring-boot-starter-tomcat"
	compile.exclude group: "org.apache.tomcat.embed", module: "tomcat-juli"
}

dependencies {
    compile('org.springframework.boot:spring-boot-starter-web')
    compile('org.springframework.boot:spring-boot-starter-jetty')
    testCompile('org.springframework.boot:spring-boot-starter-test')
}
```

### Common configuration of Jetty server

**application.properties**

```properties
# modify the default port on which Jetty listens for requests
server.port = 9090
 
# modify the default context path
server.context-path= /hello
 
# Number of acceptor threads to use.
server.jetty.acceptors= 5
 
# Maximum size in bytes of the HTTP post or put content.
server.jetty.max-http-post-size=1000000
 
# Number of selector threads to use.
server.jetty.selectors=10
```

> **Selector** threads manage events on connected sockets. Every time a socket connects, the **acceptor** threads accepts the connection and assign the socket to a selector, chosen in a round robin fashion.
[Acceptor vs selector](https://www.eclipse.org/lists/jetty-users/msg04751.html)
