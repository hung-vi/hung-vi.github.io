---
layout: post
title: Serialize/deserialize java 8 datetime with Jackson JSON mapper
categories: [java]
tags: [jackson, JSON, springboot, java8, datetime, UTC, format]
fullview: true
---

Serialize/deserialize java 8 datetime with Jackson JSON mapper

### Problem

Assuming that there is a `UserView` model:

```java
public class UserView {

    private String id;
    private String email;
    private String firstName;
    private String lastName;
    private OffsetDateTime createdAt;
    private OffsetDateTime updatedAt;
}
```

Then we expect to receive a JSON response with `createdAt` and `updatedAt` formatted as `yyyy-MM-dd'T'HH:mm:ss'Z'`
Example:

```json
{
    "id": "1",
    "email": "ngochung.vi90@gmail.com",
    "firstName": "hung",
    "lastName": "vi",
    "createdAt": "2019-05-27T12:30:01Z",
    "updatedAt": "2019-05-27T12:30:01Z"
}
```

### Solution

There is no need to implement a custom serializer for `java.time.OffsetDateTime`.
Just use [Jackson-modules-java8](https://github.com/FasterXML/jackson-modules-java8)

ApiConfiguration.java

```java
@Configuration
@EnableWebMvc
public class ApiConfiguration implements WebMvcConfigurer {

    @Bean
    public ObjectMapper objectMapper() {
        final ObjectMapper objectMapper = new ObjectMapper()
            .enable(SerializationFeature.INDENT_OUTPUT)
            .registerModule(new Jdk8Module())
            .registerModule(new JavaTimeModule())
            .configure(SerializationFeature.WRITE_DATE_KEYS_AS_TIMESTAMPS, false);
        return objectMapper;
    }

    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        ObjectMapper objectMapper = objectMapper();
        converters.add(new MappingJackson2HttpMessageConverter(objectMapper));
        converters.add(new StringHttpMessageConverter(StandardCharsets.UTF_8));
    }

}
```

build.gradle

```gradle
dependencies {
    compile group: 'com.fasterxml.jackson.datatype', name: 'jackson-datatype-jdk8', version: '2.8.4'
    compile group: 'com.fasterxml.jackson.datatype', name: 'jackson-datatype-jsr310', version: '2.8.4'
}
```
