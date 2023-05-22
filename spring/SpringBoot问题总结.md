# Springboot问题总结

- [Springboot问题总结](#springboot问题总结)
  - [Spring Boot相对Spring的优点主要有两个](#spring-boot相对spring的优点主要有两个)
    - [起步依赖](#起步依赖)
    - [自动装配](#自动装配)
  - [讲述一下 SpringBoot 自动装配原理](#讲述一下-springboot-自动装配原理)
    - [什么是 SpringBoot 自动装配](#什么是-springboot-自动装配)
    - [SpringBoot 是如何实现自动装配的](#springboot-是如何实现自动装配的)
  - [如何实现一个 Starter](#如何实现一个-starter)

## Spring Boot相对Spring的优点主要有两个

### 起步依赖

会将很多jar包按照功能合并成stater整体进行版本管理和引用，解决Spring集成其他框架时jar版本管理问题

### 自动装配

引入相关的jar包后SpringBoot会自动注册一些比较关键的bean，并进行默认配置，不用我们进行特殊配置，解决Spring重量级XML配置问题。比如整合Mybatis时的SqlSessionFactory

## 讲述一下 SpringBoot 自动装配原理

### 什么是 SpringBoot 自动装配

- 自动装配简单来说就是**自动将第三方的组件的bean装载到IOC容器内**，不需要再去写bean相关的配置，符合约定大于配置理念。
- Spring Boot基于约定大于配置的理念，配置如果没有额外的配置的话，就给按照默认的配置使用约定的默认值，按照约定配置到IOC容器中，无需开发人员手动添加配置，加快开发效率

自动装配可以简单理解为：通过注解或者一些简单的配置就能在 Spring Boot 的帮助下实现某块功能

### SpringBoot 是如何实现自动装配的

1. @SpringBootApplication是个组合注解包含
   1. 四个元注解
      1. @Target(ElementType.TYPE)
      2. @Retention(RetentionPolicy.RUNTIME)
      3. @Documented
      4. @Inherited
   2. @SpringBootConfiguration、
   3. @EnableAutoConfiguration、
   4. @ComponentScan
@EnableAutoConfiguration：启用 SpringBoot 的自动配置机制

@Configuration：允许在上下文中注册额外的 bean 或导入其他配置类

@ComponentScan： 扫描被@Component (@Service,@Controller)注解的 bean，注解默认会扫描启动类所在的包下所有的类 ，可以自定义不扫描某些 bean。如下图所示，容器中将排除TypeExcludeFilter和AutoConfigurationExcludeFilter。

原理流程汇总

从上面查看的源码，可以知道Spring Boot自动配置主要是@EnableAutoConfiguration实现的

- @EnableAutoConfiguration注解导入AutoConfigurationImportSelector类,
- 通过selectImports方法调用SpringFactoriesLoader.loadFactoryNames()扫描所有含有META-INF/spring.factories文件的jar包，
- 将spring.factories文件中@EnableAutoConfiguration对应的类注入到IOC容器中。
- 这些属性自动配置到IOC之后就无需自己手动配置bean了，Spring Boot中的约定大于配置理念，约定是将需要的配置以约定的方式添加到IOC容器中

所以spring.factories里面并不是所有的bean都会装配到IOC容器中，只会按需配置对应的bean。

## 如何实现一个 Starter
