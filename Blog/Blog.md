# Blog

# 1、需求与功能

## 1.1 用户故事

 用户故事模板：

* As a (role of user), I want (some feature) so that (some business value).
* 作为一个(某个角色)使用者，我可以做(某个功能)事情，如此可以有(某个商业价值)的好处。



角色、功能、商业价值



举例：

* 作为一个招聘网站<u>注册用户</u>，我想<u>查看最近3天发布的招聘信息</u>，以便于<u>了解最新的招聘信息</u>。
* 作为公司，可以张贴新工作。



个人博客系统的用户故事：

角色：普通访客、管理员

访客：

* 可以分页查看所有的博客；
* 可以快速查博客数最多的6个分类；
* 可以查看所有的分类；
* 可以查看某个分类下下的博客列表；
* 可以快速查看标记博客最多的1.个标签；
* 可以查看所有的标签；
* 可以查看某个标签下的博客列表；
* 可以根据年度时间线查看博客列表；
* 可以快速查看最新的推荐博客；
* 可以用关键字全局搜索博客；
* 可以查看单个博客内容；

管理员：

* 可以发布新博客；
* 可以对博客进行分类；
* 可以对博客打标签；
* 可以修改博客；
* 可以删除博客；
*  可以查询博客；
* 可以管理博客分类
  * 可以新增、修改、删除、查询分类；

* 可以管理博客标签
  * 可以新增、修改、删除、查询标签；

# 2、框架搭建

## 2.1 构建与配置

1、引入Spring Boot模块：

* web
* Thymeleaf
* JPA
* MySQL
* Aspects
* DevTools

## 2.2 异常处理

1、错误页面：

* 404
* 500
* error

2、全局处理异常

统一处理异常：

```java
@ControllerAdvice
public class ControllerExceptionHandler {
	
    private Logger logger = LoggerFactory.getLogger(this.getClass());

    @ExceptionHandler(Exception.class)
    public ModelAndView exceptionHandler(HttpServletRequest request,Exception e){
        
        logger.error("Request URL : {},Exception : {}",request.getRequestURI(),e);

        if(AnnotationUtils.findAnnotation(e.getClass(), ResponseStatus.class) !=null){
            throw e;
        }
        
        ModelAndView mv = new ModelAndView();
        mv.addObject("url",request.getRequestURI());
        mv.addObject("exception",e);
        mv.setViewName("error/error");
        return mv;
    }
}
```

3、资源找不到异常

```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class NotFoundException extends RuntimeException{
    public NotFoundException() {
    }

    public NotFoundException(String message) {
        super(message);
    }

    public NotFoundException(String message, Throwable cause) {
        super(message, cause);
    }
}

```



## 2.3 日志处理

1、记录日志内容

* 请求 url
* 访问者 ip
* 调用方法 classMethod
* 参数 args
* 返回内容



2、记录日志类

```java
@Aspect
@Component
public class LogAspect {
    private Logger logger =LoggerFactory.getLogger(this.getClass());

    //定义切面
    @Pointcut("execution(* com.blog.controller.*.*(..))")
    public void log(){}

    @Before("log()")
    public void doBefore(JoinPoint joinPoint){
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();
        String url = request.getRequestURI();
        String ip = request.getRemoteAddr();
        String classMethod = joinPoint.getSignature().getDeclaringTypeName()+"."+joinPoint.getSignature().getName();
        Object args = Arrays.toString(joinPoint.getArgs());
        RequestLog requestLog = new RequestLog(url,ip,classMethod,args);

        logger.info("Request : {}",requestLog);
    }

    @After("log()")
    public void doAfter(){
        logger.info("-------doAfter----------");
    }

    @AfterReturning(returning = "result",pointcut = "log()")
    public void doAfterRuturn(Object result){
        logger.info("-------Result : {}----------",result);
    }

    private class RequestLog{
        private String url;
        private String ip;
        private String classMethod;
        private Object args;

        public RequestLog(String url, String ip, String classMethod, Object args) {
            this.url = url;
            this.ip = ip;
            this.classMethod = classMethod;
            this.args = args;
        }

        @Override
        public String toString() {
            return "RequestLog{" +
                    "url='" + url + '\'' +
                    ", ip='" + ip + '\'' +
                    ", classMethod='" + classMethod + '\'' +
                    ", args=" + args +
                    '}';
        }
    }
}
```

# 3、对象

## 3.1 对象设计

实体类：

* 博客Blog
* 博客分类Type
* 博客标签Tag

Blog

```java
@Entity
@Table(name = "t_blog")
public class Blog {

    @Id
    @GeneratedValue
    private Long id;
    private String title;
    private String content;
    private String flag;
    private boolean published;

    @Temporal(TemporalType.TIMESTAMP)
    private Date createTime;
    @Temporal(TemporalType.TIMESTAMP)
    private Date updateTime;

    @ManyToOne
    private Type type;

    @ManyToMany(cascade = {CascadeType.PERSIST})
    private List<Tag> tags = new ArrayList<>();
}
```

Type

```java
@Entity
@Table(name = "t_type")
public class Type {

    @Id
    @GeneratedValue
    private Long id;
    private String name;

    @OneToMany(mappedBy = "type")
    private List<Blog> blogs = new ArrayList<>();
}
```

Tag

```java
@Entity
@Table(name = "t_tag")
public class Tag {

    @Id
    @GeneratedValue
    private Long id;
    private String name;

    public Tag(){
    }

    @ManyToMany(mappedBy = "tags")
    private List<Blog> blogs = new ArrayList<>();
}
```

