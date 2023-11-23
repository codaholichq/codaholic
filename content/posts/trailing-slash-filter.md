---
title: "How to Handle Trailing Slashes in Spring Boot"
slug: trailing-slash-filter
description: "Navigate the nuances of trailing slashes in Spring Boot API endpoints with practical tips for effective handling and a seamless user experience."
date: 2023-08-20T15:09:10+01:00
draft: false
cover:
  image: img/00-trailing-slash-filter.webp
  alt: 'Trailing Slash Filter'
  caption: 'Trailing Slash Filter'
---

### Introduction
Two types of slashes exist, the backward slash and the forward slash. A good way to differentiate the two is that a backward slash leans backward(\), while a forward slash leans forward(/)

The slash we want to handle in [Spring Boot](/spring-boot-guide) is the forward slash. We want to handle `trailing` forward slash at the end of an API endpoint.
<br/>

### The problem
When you send a request to this API endpoint `api/users/add` it will return a state code of `200`, but when you send a request to this API endpoint `api/users/add/` it will return a status code of `404`.
Now we want a situation where `api/users/add/` will redirect or point to the same resource that `api/users/add` points to.
To fix this problem, In Spring Boot 2.7.x we have to add the following configuration.

```code
@Configuration
class WebConfiguration implements WebFluxConfigurer {

    @Override
    public void configurePathMatching(PathMatchConfigurer configurer) {
        configurer.setUseTrailingSlashMatch(true);
    }
}
```

Sadly, in Spring Boot v3.x.x `setUseTrailingSlashMatch(true);` was deprecated and if you check this [GitHub issue](https://github.com/spring-projects/spring-framework/issues/28552) you see a lot of people who complained of wasting a lot of time trying to resolve the issue of a trailing slash returning a status code of `404` and also you see the Spring Team defending why they deprecated this method.
<br/>

### The Solution
Kindly note that this is not a hack or a workaround.
Now what this solution does is to remove `trailing slashes` from API endpoints or URLs, to do this issue we have to create a custom filter.

{{< code-snippets 635dcef446700d90ca4937c936f33df3 >}}

The code above is a Spring component that implements a servlet filter, this is designed to remove trailing slashes from URLs. Here's a step-by-step explanation of how this filter works:

1. The `TrailingSlashFilter` class is annotated with `@Component`, which means it's a Spring component and will automatically get detected during classpath scanning. This filter is designed to intercept HTTP requests and perform some processing before they reach the controller.

2. The `doFilter` method is overridden from the `Filter` interface. This method is called by the servlet container each time a request/response pair is passed through the chain due to a client request for a resource at the end of the chain. The `doFilter` method takes three parameters: `ServletRequest`, `ServletResponse`, and `FilterChain`. The `ServletRequest` and `ServletResponse` are the request and response objects, and the `FilterChain` is used to pass the request and response objects to the next filter in the chain.

3. Inside the `doFilter` method, the `ServletRequest` is cast to an `HttpServletRequest`, which allows access to HTTP-specific methods. The request URI is then retrieved and checked if it ends with a trailing slash.

4. If the URI ends with a trailing slash, a new URI without the trailing slash is created. A new `HttpRequestWrapper` object is created with the original request and the new URI. This wrapper overrides the `getRequestURI` and `getRequestURL` methods to return the new URI and URL.

5. The `doFilter` method of the `FilterChain` is then called with the new request and response objects. If the URI does not end with a trailing slash, the `doFilter` method of the `FilterChain` is called with the original request and response objects.

6. The `HttpRequestWrapper` is a private static class that extends `HttpServletRequestWrapper`. It overrides the `getRequestURI` and `getRequestURL` methods to return the new URI and URL. This is necessary because once a request has been wrapped, the original URI cannot be changed.

In essence, the filter above removes trailing slashes from URLs. It carries this function by creating a new request with the modified URI and passing this new request to the next filter in the chain. If the URI does not end with a trailing slash, the original request is passed to the next filter. This filter does not affect the [performance](/make-maven-faster) of your API
<br/>

### Don't Try This
When I was trying to implement this solution, I removed the `else` statement, because I thought it was redundant.

{{< code-snippets fc1fbe282991260e971f2ee51155ecbe >}}

This code modification caused my application to break at runtime, you can find it in this [GitHub Issue](https://github.com/codaholichq/meetona/issues/25)

I had to return the `else` statement

{{< code-snippets 69240d96712198af711c730d562fb614 >}}

<br/>

### Frequently Asked Questions
**How do I fix a trailing slash in a URL?**
To fix a trailing slash with an .htaccess file, you can utilize mod_rewrite to modify the address. Keep in mind that the RewriteRule command triggers a redirection from the original address to the modified one. If you wish to skip this redirection, you can omit the R=301 flag from the RewriteRule command.

**What is the trailing slash rule?**
A trailing slash is the / at the end of a web address. It helps differentiate between a folder (with the slash) and a file (without). While it's a helpful practice, it's not a strict rule, just a good suggestion for clarity.

**What is the problem with trailing slash?**
Having a trailing slash at the end of your website's URL might lead to duplicate content problems. In simple terms, Google doesn't appreciate identical content spread across various pages.

**What is the best practice for trailing slash URL?**
it's best not to have multiple versions of the same webpage—one with a trailing slash and one without. If you choose to use a trailing slash, make sure that when someone accesses the URL without it, they are automatically redirected to the correct page instead of seeing an error or a new page. This helps maintain a consistent and user-friendly experience on your website.