# JavaScript学习

JavaScript用于网页和用户之间的交互，比如提交的时候，进行用户名是否为空的判断。
完整的javascript由语言基础，BOM和DOM组成。

## 基础

* \<script>标签

  javascript都是放在script标签中的，一旦加载，就会执行（如果使用的是外部.js文件则不需要加标签）

* var声明变量

  然而事实上不使用var也可以，但为了规范性还是写吧

* alert(1)调试

  会弹出一个对话框，里面的内容是1。换句话说，如果弹出了1,这个位置以上的代码，都是可以运行的。
  你不停的把alert(1)向下移动，当移动到某一行之后，就不再弹出，那么就证明那一行运行有问题

  当然这只是自己使用的一种调试方法，一般浏览器的控制台会有调试功能的

* 基本数据类型

  * javascript的基本数据类型：Boolean、Number、String、var

  * Boolean:true、false

  * Number:javascript中的Number可以表示十进制，八进制，十六进制整数，浮点数，科学记数法，虽然使用document.write()输出时都会转为十进制

  *  String:javascript不分字符和字符串，且单引号和双引号作用相同

  * var:如第2点所说的，只是一个变量的声明，而变量实际是什么类型需要看变量实际的值；且如果变量只声明了却未赋值则为'underfined'

  * typeof＋变量名可得到变量类型

* 数据类型转换

  * 无论是Number,Boolean还是String都有一个toString和String()方法，用于转换为字符串。Number的toString()方法可加参数，意思为转为几进制；如a.tostring(2)，不加则默认十进制。区别在于对null的处理，String()会返回字符串"null"，toString() 就会报错，无法执行
  * 字符串可使用parseInt()、parseFloat()和Number()转为数字，当转换的内容包含非数字的时候，Number() 会返回NaN(Not a Number)，parseInt() 要看情况，如果以数字开头，就会返回开头的合法数字部分，如果以非数字开头，则返回NaN
  * Number,String,Object可使用Boolean()转换为Boolean值

* 函数

  写在\<script>标签中，function用于定义一个函数

* 错误处理

  JavaScript提供了一种try catch的错误处理机制，当有错误抛出的时候，可以catch住

  ```javascript
  try{
      f1();//调用不存在的函数
  }
  catch(e){
      document.write(e.message);
  }
  ```

  

## BOM

BOM即 浏览器对象模型(Browser Object Model)

* Window

  一旦页面加载，就会自动创建window对象，所以无需手动创建window对象。

  * 获取文档显示区域的高度和宽度window.innerWidth/window.innerHeight

  * 获取浏览器的高度和宽度window.outerWidth/window.outerHeight

  * 打开一个新的窗口window.open("url")

* Navigator

  即浏览器对象，提供浏览器相关的信息

  * navigator.appName:浏览器产品名称

  * navigator.appVersion:浏览器版本号

  * navigator.appCodeName:浏览器内部代码

  * navigator.platform:操作系统

  * navigator.cookieEnabled:是否启用Cookies

  * navigator.userAgent:浏览器的用户代理报头

* Screen

  表示用户的屏幕相关信息

  * 用户屏幕分辨率:screen.width/screen.height

  * 可用区域大小:screen.availWidth/screen.availHeight

* History

  History用于记录访问历史

  * history.back();

  * history.go(-1);

* Location

  浏览器中的地址栏

  * 刷新页面 location.reload();

  * 跳转页面 location.assign("/");

  * 协议 location.protocol
    主机名 location.hostname
    端口号 (默认是80，没有即表示80端口)location.port
    主机加端口号 location.host
    访问的路径  location.pathname
    锚点 location.hash
    参数列表 location.search

* 弹出框

  浏览器上常见的弹出框有
  警告框，确认框，提示框 这些都是通过调用window的方法实现的

  * 警告框alert("String")，常用于消息提示，比如注册成功等等

  * 确认框confirm("String")，常用于危险性操作的确认提示。 比如删除一条记录的时候，弹出确认框
    confirm返回基本类型的Boolean true或者false
  * 输入框prompt("String")，用于弹出一个输入框，供用户输入相关信息。 因为弹出的界面并不好看，很有可能和网站的风格不一致，所以很少会在实际工作中用到

* 计时器

    etTimeout:只执行一次
   
  setInterval:不停地重复执行
  
clearInterval:终止重复执行
  
  document.write():不要在setInterval调用的函数中使用document.write()，这是因为document.write，会创建一个新的文档，而新的文档里，只有打印出来的时间字符串，并没有setInterval这些javascript调用，所以只会看到执行一次的效果



## DOM

DOM是Document Object Model(文档对象模型)的缩写。
DOM是把html里面的各种数据当作对象进行操作的一种思路。

> 2019.11.12更新