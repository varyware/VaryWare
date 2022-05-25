---
layout: markdown
title: A Simple Document Of Post Layout Test, Lub Uub Dub Dub Dib Dob Tog Dog
date: 2019-10-24 09:55:16 +0800
author: VaryWare
categories: Java
show_excerpt_image: true
---

At its core, Spring is about making things a little simpler. Common sense dictates that simplicity often requires rules to deal with complexity. There is no need to elevate learning a framework to a philosophical perspective, so when learning Spring's core philosophy you only need to consider a little bit of "is what I'm learning and understanding making things simple", if not then you are probably learning from the wrong perspective.

Spring uses interfaces to reduce coupling and build a network of component dependencies based on the developer's configuration. Spring matches the basic components needed for complex components against a pre-configured network of relationships and assembles them (context) so that developers can request them directly from Spring when they need them (context.getBean).

## Prepare to start learning
Configure a Spring Framework project that can run quickly and display results.

## Spring Configuration
Spring configuration means human-defined component dependencies for Spring to initialize and for Spring to provide when a component is needed. There is a Java configuration and an XML configuration. For Java configuration `@Component @ComponentScan @Configuration @Bean @Named @Autowired @Import @ImportResource` is the common annotation.

## Auto-assembly and Dependency Injection
Dependency injection is not fully automated and still requires someone to configure what to "inject" the first time you use it. The core value is to reduce the coupling between the code through interfaces, ~~ to allow more different possibilities for the same component, and ~~ to benefit from the first configuration when creating new components, Spring has the ability to automatically find the dependencies the component needs.

Using Spring's dependency injection philosophy, what you end up with is a web of dependencies between different components, which are configured by the developer. When a component is needed, that component is no longer put together by the developer, Spring automatically generates the component through the dependency network, also known as auto-assembly.

## Handling auto-assembly ambiguities
If more than one bean can satisfy a dependency, Spring will throw an exception indicating that it did not explicitly specify which bean to select for auto-assembly. So Spring provides some mechanism to specify a bean to be injected.

> If more than one bean can satisfy the dependency, Spring will throw an exception indicating that it did not explicitly specify which bean to select for auto-assembly.   

The most basic thing to do in conventional thinking is to name them differently, but it still doesn't satisfy the "which to assemble" problem, and manual configuration defeats the purpose of auto-assembly.

In practice Spring uses something close to what I suspected, defining a priority `@Primary` for the component with an additional qualifier `@Qualifier("beanname")` to narrow the scope, which can be stacked multiple times, but the qualifier is defined using text that is unstable. annotations, which can also be used as a way to handle ambiguity.

## How to not create a singleton for Spring managed components
> By default, beans in Spring are singleton, and Spring intercepts calls to beanCreating() and ensures that the bean returned is the one created by Spring, which is the one created by Spring itself when calling beanCreating(). bean instance.   

For volatile components, data changes are synchronized to all places where the component is used. spring provides several scopes to define the creation of bean via `@Scope`, and you can see the supported scopes at ConfigurableBeanFactory. For XML configuration the scope can be set in the bean element. The proxy option proxyMode can be used to handle the case of prototype injection singleton, such as shopping cart and store services.

## Spring environment variables
In addition to configuring data sources, Spring itself has further support for Environments, which can be injected at runtime by adding data to the Environment. Some properties are introduced by specifying a properties file via the `@PropertySource` annotation. (But intuitively this is not the right way to introduce data, this feature is better suited for defining environment variables)

1. Some Orderd List Item 00001
1. Some Orderd List Item 00001
    1. Some Orderd List Item 00001
    1. Some Orderd List Item 00001
    1. Some Orderd List Item 00001
        1. Some Orderd List Item 00001
        1. Some Orderd List Item 00001
    1. Some Orderd List Item 00001
        1. Some Orderd List Item 00001
    1. Some Orderd List Item 00001
        1. Some Orderd List Item 00001
            1. Some Orderd List Item 00001
1. Some Orderd List Item 00001
1. Some Orderd List Item 00001
1. Some Orderd List Item 00001

## SpEL
SpEL is contained in `#{...}`, and can be used to call some types of methods such as System using T(), to directly reference properties and methods of a bean using bean.propertyName? and to reference system or environment properties using systemProperties['key']. systemProperties['key']. The `[]` operator not only drinks array elements, but also references letters in strings `#{'Hello'[3]}`, and for a collection `#{songs. [artists eq 'Leehom']}` can filter out the elements of the specified condition, `#{. ^[]} / #{. $[]}` is used to get the first or last matching item, `#{.! []}` puts an attribute of an element of a collection in a new set of strings, and supports the continuous use of `#{songs.? [artists eq 'Leehom'].! [title]}`.

## Tangent-oriented programming
```
@AspectJ --- Tangent
@Pointcut("execution(* *.methodInvoke(...))") --- cutpoints
@Before("pointcut() && within(package.*)") --- point in time
public class doSomething(){} --- Notification
@DeclareParents(value="Basic+",defaultImpl = ExtraImpl.class) --- add functionality
```

At a specific point in time, the cutscene gets the context of the program and does some processing such as logging, permission validation, adding information to add methods, etc. With tangents, the original program is kept simple and clean.

A tangent is a combination of one or more tangents, but tangents with similar functionality are not required to be placed in the same tangent, and notifications are triggered when the circumstances defined by the tangent are met.

* Some List Item 000001
    * Some List Item 000001
    * Some List Item 000001
    * Some List Item 000001
        * Some List Item 000001
            * Some List Item 000001
            * Some List Item 000001
                * Some List Item 000001
* Some List Item 000001
* Some List Item 000001

If you want to implement tangent-oriented programming, one possible way is to "spice up" the source code (or compilation) during compilation or other cycles of the program. Wrap the flow of the cutpoint configuration and do the corresponding processing before running, after running, and when an error is thrown. To make Spring recognize a cutpoint, you first need to add the cutpoint as a bean to Spring's dependency network, and enable `@EnableAspectJAutoProxy`.

You can refer to the official document [Aspect Oriented Programming with Spring](https://docs.spring.io/spring/docs/4.3.15.RELEASE/spring-framework-reference/html/ aop.html)

Spring supports a cutpoint expression language including `arg() @args() excution() this() target() @target() within() @within() @annotation @bean()`. The conjunction && is interchangeable with and.

Common notifications are `@Before @After @AfterThrowing @AfterReturing`, and for `@Around` round robin notifications, the notification method needs to use the ProceedingJoinPoint argument (not sure if it can be combined with the arg() syntax here), which is combined with the try ... catch syntax combined with jp.proceed() to define actions at different points in time.

A typical cutpoint description.
```
Notify when a method of any location, of any return type, with any argument is called
execution(* com.location.of.Class.method(...))
Notification is triggered only in specific packages
execution(* com.location.of.Class.method(...)) && within(com.spetial.*)
Trigger notifications other than the specified bean
execution(* com.location.of.Class.method(...)) and !bean('beanName')
Define common cutpoints via Pointcut
@Pointcut("execute(** com.location.of.Class.method(String)) && args(param)")
public void method(String param){}
```

## Read the data defined in Properties
There are two ways to make the call:
``` java
@Configuration
@PropertySource("classpath:application.properties")
public class AppConfig {
    @Bean
    public PropertyValue getVik(@Value("${vik}") String value){ 
        return new PropertyValue(value); 
    }
}

@Configuration
@PropertySource("classpath:application.properties")
public class AppConfig {
    @Autowired
    private Environment env;     

    @Bean
    public PropertyValue getVik(){ 
        return new PropertyValue(env.getProperty("vik")); 
    }
}
```
