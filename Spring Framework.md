# The IoC Container

This chapter covers the Spring Framework implementation of the Inversion of Control(IoC) principle. IoC is also known as dependency injection(DI).

In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called **beans**.

A **bean** is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container. 

Otherwise, a bean is simply one of many objects in your application. Beans, and the dependencies among them, are reflected in the configuration metadata used by a container.



## Container Overview
The org.springframework.context.ApplicationContext interface represents **the Spring IoC container** and is responsible for instantiating, configuring, and assembling the beans. 
 
The container gets its instructions on what objects to instantiate, configure, and assemble by reading **configuration metadata**.

The configuration metadata is represented in XML, Java annotations, or Java code. It lets you express the objects that compose your application and the rich interdependencies between those objects.
> Spring 2.5 supports annotation-based configuration metadata.
> Spring 3.0 supports Java-based configuration. To use these new features, see the @Configuration, @Bean, @Import, and @DependsOn annotaions.  

The following example shows the basic structure of XML-based configuration metadata:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--The id attribute is a string that identifies the individual bean definition. -->
    <!--The class attribute defines the type of the bean and uses the fully qualified classname. -->
    <bean id="..." class="...">   
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>
```

The location path or paths supplied to an ApplicationContext constructor are resource strings that let the container load configuration metadata from a variety of external resources, such as the local file system, the Java CLASSPATH, and so on.
```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```


### Composing XML-based Configuration Metadata
It can be useful to have bean definitions span multiple XML files. Often, each individual XML configuration file represents a logical layer or module in your architecture.
You can use the application context constructor to load bean definitions from all these XML fragments. This constructor takes multiple Resource locations, as was shown in the previous section. Alternatively, use one or more occurrences of the <import/> element to load bean definitions from another file or files. The following example shows how to do so:
```xml
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```




