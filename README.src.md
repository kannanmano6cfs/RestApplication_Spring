Getting Started: Consuming RESTful Web Services with Spring
=========================================

This guide walks you through the process of using Spring's `RestTemplate` to consume a RESTful web service.

To help you get started, here's an initial project structure in GitHub:

```sh
$ git clone https://github.com/springframework-meta/gs-consuming-rest-core.git
```

Before you can write the REST endpoint itself, there's some initial project setup that's required. Or, you can skip straight to the [fun part]().

Select dependencies
----------------------
The sample in this guide uses Spring and the Jackson JSON processor. Thus you need to declare the following library dependencies in your build:

  - org.springframework:spring-web:3.2.2.RELEASE
  - org.codehaus.jackson:jackson-mapper-asl:1.9.9

Click here for details on how to map these dependencies to your specific build tool.

Invoke REST services with the RestTemplate
----------------------------
Spring provides a convenient template class called the `RestTemplate`. The `RestTemplate` makes interacting with most RESTful services a one-line incantation. The following example establishes a few variables and then makes a request to the Twitter search service. The result comes back - in this case - as a String that we can print to the console. 


```java
package hello;

import org.codehaus.jackson.JsonNode;
import org.springframework.http.HttpHeaders;
import org.springframework.http.ResponseEntity;
import org.springframework.web.client.RestTemplate;

public class Main {
    public static void main(String args[]) throws Throwable {

        String searchUrl = "http://search.twitter.com/search.json?q={query}";
        String searchQueryPathVariableValue = "@gopivotal";

        RestTemplate restTemplate = new RestTemplate();

        String bodyOfResponse = restTemplate.getForObject( 
                      searchUrl, String.class, searchQueryPathVariableValue);
        // work with the response body
    }
}
```

This is not as useful as you'd think, because the Twitter search service returns JSON-structured data. It's easy enough to get meaningful payloads from this, however. The `RestTemplate` supports configuration of `HttpMessageConverter` objects that can convert request and response payloads as appropriate. If the Jackson JSON processing library is on the classpath, you can use it to convert the requested data into your custom domain objects, or at a minimum, the Jackson `JsonNode` base type.

Add the following incantations at the bottom of the `main` method.

>__TODO__: briefly talk about what message converters do and list the ones that come out of the box with Spring}

```java
        JsonNode rootNode = restTemplate.getForObject(searchUrl, JsonNode.class, searchQueryPathVariableValue);
```

Now you can iterate over the results in terms of nodes in the JSON structure. If you want to inspect the HTTP headers of the response, or the status code, use the `*forEntity` variant methods of the `RestTemplate` that return the payload, as before, along with an envelope (`ResponseEntity`) that has details about headers, status code, and so forth. Add this to the bottom of your `main` method:

```java
        ResponseEntity<JsonNode> response = restTemplate.getForEntity(
                    searchUrl, JsonNode.class, searchQueryPathVariableValue);
        HttpHeaders httpHeaders = response.getHeaders();
```

Thus far, you've only used the HTTP verb `GET` to make calls, but you could use `POST`, `PUT`, and so on.

Build and run the client
--------------------------------------
To invoke the code and see the results of the search, simply run it from the command line:

```sh
$ gradle run
```
	
This command compiles the `main` method and then run it.


Next steps
----------
Congratulations! You have just developed a simple REST client using Spring.  

There's more to building and working with REST APIs than is covered here. You may want to continue your exploration of Spring and REST with the following Getting Started guides:

* **Consuming REST Services on Android**
* Handling POST, PUT, and GET requests in REST endpoints
* Creating self-describing APIs with HATEOAS
* Securing a REST endpoint with HTTP Basic
* Securing a REST endpoint with OAuth
* Consuming REST APIs
* Testing REST APIs


