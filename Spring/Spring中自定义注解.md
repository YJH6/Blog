# Spring中自定义注解

在学习Spring的时候，我们常常会使用一些注解，比如什么@RequestMapping、@Override、@Controller，那么问题来了，注解这么好用，你想不想来个呢？来来来，我们来个

* 首先我们先创建我们自定义的注解类

  ```java
  @Documented //注解信息会被添加到Java文档中
  @Retention(RetentionPolicy.RUNTIME) //注解的生命周期，表示注解会被保留到什么时候
  @Target(ElementType.METHOD) //注解作用位置
  public @interface MyLog {
      String value() default "";
  }
  ```

* 然后写个方法加上我们才写的注解

  ```java
  @MyLog("这是注解内容")
  @RequestMapping("user/{id}")
  public User findUser(@PathVariable("id") Integer id) {
  	return userService.findUserById(id);
  }
  ```

  好了，加上我们写的注解了！可惜，这个注解目前还没用不起，因为如果你看下上面的代码，你就会发现我们所做的其实仅仅是对注解进行声明（嗯，我这地方有个这个注解，但是，这注解什么时候使用？怎么使用？）

* 我们创建切面类

  ```java
  @Component
  @Aspect
  public class MyLogAspect {
      
      @Pointcut("@annotation(com.example.demo.annotation.MyLog)")
      private void pointcut() {}
      
      @Before("pointcut() && @annotation(logger)")
      public void advice(MyLog logger) {
          System.out.println("--- 日志的内容为[" + logger.value() + "] ---");
      }
  }
  ```

  其中@Pointcut声明了切点（这里的切点是我们自定义的注解类），@Before声明了通知内容，在具体的通知中，我们通过@annotation(logger)拿到了自定义的注解对象，所以就能够获取我们在使用注解时赋予的值了

  OK，现在我们就写出了属于我们的第一个注解。
  
  在Web项目（这里特指Spring项目）中使用自定义注解开发，其原理还是依赖于Spring的AOP机制，这一点就与我们普通的Java项目有所区别。当然，如果是开发其他框架而需要使用自定义注解时，则需要自己实现一套机制，不过原理本质上都是大同小异，无非是将一些模板操作进行了封装