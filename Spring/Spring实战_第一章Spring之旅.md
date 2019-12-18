# Spring实战

## 第一章 Spring之旅

 		最初Spring的创建目的主要是用来替代企业级Java技术，尤其是EJB(Enterprise JavaBean)。Spring提供了更加轻量级和简单的编成模型，它增强了简单老式Java对象(Plain Old Java object,POJO)的功能，使其具备了企业级才具备的功能。

### 1.1 简化Java开发

​		Spring是为了解决企业级应用开发的复杂性而创建的。但Spring不仅仅局限于服务器端开发，任何Java应用都能在简单性、可测试性和松耦合等方面从Spring中获益。

* **Spring最根本的使命:简化Java开发**

​		这是不是很个十分狂傲的承诺，我第一次看见的时候都不敢相信，现在我还是没懂，毕竟我才开始学习Spring，接下来就让我们一起来看下这Spring是牛\#还是傻\#。

* 为了降低Java开发的复杂性，Spring采取了以下四种关键策略：
  * 基于POJO的轻量级和最小侵入性编程；
  * 通过依赖注入(Dependency Injection,DI)和面向接口实现松耦合；
  * 基于切面和惯例进行声明式编程；
  * 通过切面和模板减少样板式代码；

  是不是蒙了，我也是，不过我相信我们会在接下来的学习中逐渐体会到这四个策略。



#### 1.1.1 激发POJO的潜能

​		Spring竭力避免因自身的API而弄乱你的应用代码。在基于Spring构建的应用中，它的类通常没有任何痕迹表明你使用了Spring。

		> 在《Spring实战》上有这样一句话:
		>
		> 最坏的情景是，一个类或许会只用Spring注解，但它依旧是POJO 
		>
		> 原谅我理解能力不行，这个'最坏'没懂，我希望在之后我懂了再来更改这篇文章。

​		可以举个例子，请参考下面的HelloWorldBean类:	

```java
public class HelloWorldBean{
    public String Hello(){
        return "Hello World!";
    }
}
```

​		可以看到，这是一个POJO，Spring的非侵入编程可以让上面这个类在Spring应用和非Spring应用中都可以发挥同样的作用。然而你可以十分明显的看出，上面这个类没有任何一个地方与Spring有关。



​		尽管POJO十分简单，似乎没有什么特别的用处，但是Spring可以赋予POJO特别的用处，其一就是通过DI来装配它们。接下来就是看看DI如何让应用对象彼此之间保持松耦合的了。

#### 1.1.2 依赖注入

​		首先我们都经常听到一个对应用很有用的词语:解耦合。

​		但是耦合具有两面性。紧密耦合的代码不用说了，难测试、难复用、难理解，并且表现出"打地鼠"式的bug特性。另一方面，一定的耦合又是一个应用程序所必须的，为了完成有实际意义的功能，不同的类必须以适当的方式进行交互。当然，上面的HelloWorld排除。

​		通过DI，对象的依赖关系将由系统中负责协调各对象的第三方组件在创建对象的时候进行设定。对象无需自行创建或管理它们的依赖关系。依赖注入会将所依赖的关系自动交给目标对象，而不是让对象自己去获取依赖。

​		如果一个对象只通过接口(而不是具体的实现或初始化过程)来表明依赖关系，那么这种依赖就能够在对象本身毫不知情的情况下，用不同的具体实现进行替换，这就是松耦合。而DI最大的收益就是松耦合，DI将依赖关系交给目标对象后，目标对象并不需要知道所依赖的关系内部的不同。

​		创建应用组件之间协作的行为通常称为装配(wiring)。Spring有多种装配bean的方式，采用XML是很常见的一种装配方式:

```xml
<？xml version =“1.0”encoding =“UTF-8”？> 
<beans  xmlns = “http://www.springframework.org/schema/beans” 
    xmlns：xsi = “http://www.w3.org / 2001 / XMLSchema-instance“ 
    xsi：schemaLocation = ”
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd“ >

    <--  bean定义在这里 -- >

 </beans>
```

关于XML的写法可以参考[**XML Schema-based configuration**](https://docs.spring.io/spring/docs/4.2.x/spring-framework-reference/html/xsd-configuration.html)

​		Spring除了XML配置外还支持使用Java来描述配置：

```java
package com.test;

import com.test.Test;
import com.test.SonTest;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class TestConfig{

    @Bean
    public Test1 test1(){
        return new SonTest(test2());
    }
    
    @Bean
    public Test2 test2(){
        return new Test2();
    }
}
```

​		不论是用XML还是Java来进行配置，DI所带来的益处都是相同的，应用组件不需要知道依赖的是谁，只需要知道is什么就行。只有Spring通过它的配置，能够了解这些组成部分是如何装配起来的。这样的话，就可以在不改变所依赖的类的情况下，修改依赖关系。

​		Spring通过应用上下文(Application Context)装配bean的定义并把他们组装起来，Spring应用上下文全权负责对象的创建和组装。Spring自带了多种应用上下文的实现，区别仅仅在于如何加载装配。

​		XML配置一般选择ClassPathXmlApplicationContext作为应用上下文；

​		Java配置一般选择AnnotationConfigApplicationContext作为应用上下文；

```java
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestMain{

    public static void main(String[] args) throws Exception{
        ClassPathXmlApplicationContext context =
                new ClassPathXmlApplicationContext(
                        "META-INF/spring/test.xml");
        Test test = context.getBean(Test.class);
        test.method();
        context.close();
    }
}
```

​		这里的main()方法基于test.xml文件创建了Spring应用上下文。随后它调用该应用上下文获取ID为test的bean。得到Test对象的引用后就可以调用方法了。



> 根据《Spring实战》的推荐，想要了解更多关于依赖注入的信息，可以阅读Dhanji R. Prasanna的《Dependency Injection》,该著作覆盖了依赖注入的所有内容。



​	现在继续了解下Spring简化Java开发的下一个理念:基于切面进行声明式编程。

#### 1.1.3 应用切面

​		DI能够让相互协作的软件组件保持松散耦合，而面向切面编程(aspect-oriented programming,AOP)允许你把遍布应用各处的功能分离出来形成可重用的组件。

​		面向切面编程往往被定义为促使软件系统实现关注点分离的一项技术。系统由许多不同的组件组成，每一个组件各负责一项特定功能。除了实现自身核心的功能之外，这些组件往往还需要进行其他的比如日记、事务管理和安全等系统服务，这些系统服务会使得这个组件复杂化。每个对象不但要知道它需要日记、事务管理和安全等，还要亲自执行这些服务。

​		AOP能够使这些服务模块化，并以声明的方式将它们应用到它们需要影响的组件中去。

​		更多关于AOP以及AOP的XML写法可以学习[**Aspect Oriented Programming with Spring**](https://docs.spring.io/spring/docs/4.2.x/spring-framework-reference/html/aop.html)



​		接下来，让我们再看看Spring简化Java开发的其他方式

#### 1.1.4 使用模块消除样版式代码

​		通常为了实现通用的和简单的任务，我们会重复编写一样的代码，这就是样板式代码。

​		或许你写过JDBC，你一定知道JDBC要连接数据库、要抛出异常，然而每一个JDBC代码都会出现这些大量的的、繁琐的、重复的代码，除了少量的代码与你真正需要的逻辑有关，其他的代码都是JDBC的样板式代码。

​		JDBC不是产生样板式代码的唯一场景。在许多编程场景中往往都会导致类似的样板式代码。

​		Spring通过模板封装来消除样板式代码。比如Spring的JdbcTemplate使得执行数据库操作时，避免传统的JDBC样板代码。



​		上面我们已经知道了Spring可以通过面向POJO编程、DI、切面和模板技术来简化Java开放中的复杂性。我们知道可以通过XML文件配置bean和切面。但是我们还只是知道了是什么，接下来我们将去了解为什么，也就是Spring容器(container)，这是应用中的所有bean所驻留的地方。

### 1.2 容纳你的Bean

 

