## Springboot的核心注解是什么？
@SpringApplication，由三个核心注解组成：
- @SpringBootConfiguration：组合了 @Configuration 注解，实现配置文件的功能。
- @EnableAutoConfiguration：打开自动配置的功能，也可以关闭某个自动配置的选项，如关闭数据源自动配置功能
- @ComponentScan：Spring组件扫描。


## Springboot的启动过程
加载Spring Boot的主配置类：Spring Boot应用的入口是通过主配置类（通常是使用@SpringBootApplication注解的类）来启动的。在启动过程中，Spring Boot会通过Java的反射机制加载主配置类。

执行Spring Boot的自动配置：Spring Boot的自动配置机制可以根据classpath下的依赖、配置文件等信息，自动配置Spring应用所需的各种组件。这些自动配置类都会被加载到Spring的ApplicationContext中。

执行Spring Boot的启动器：Spring Boot的启动器是通过spring-boot-starter模块提供的依赖来实现的。启动器模块会根据应用的需求自动引入相应的依赖，比如spring-boot-starter-web会自动引入Spring MVC、Tomcat等相关的依赖。

启动Spring应用程序上下文：Spring Boot会创建并启动一个Spring应用程序上下文（ApplicationContext），并将自动配置的各种组件注册到上下文中。在上下文准备就绪后，Spring Boot会触发各种事件，比如ApplicationStartedEvent、ApplicationReadyEvent等。

启动内嵌的Web服务器：如果应用程序是一个Web应用程序，Spring Boot会自动启动一个内嵌的Web服务器（如Tomcat、Undertow等），并将Spring MVC的DispatcherServlet注册到服务器中。这样，应用程序就可以接收HTTP请求并作出相应。