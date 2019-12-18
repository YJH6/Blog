# 反射（Reflect）

## 一、Class类

* 类是对象，类是java.lang.Class类的实例对象，这个对象称为该类的类类型；

* 任何一个类都是Class的实例对象，类的表达式：

  * Class c1 = int.Class;

  * Class c2 = c1.getClass();

  * Class c3 =null; c3 = Class.forName ("com.imooc.reflect.Foo");

* 任何一个类都有一个隐含的静态成员变量；

* 创建类类型创建该类的实例对象：

　　c1.newInstance();//需要进行强制类型转换，前提是需要有无参数的构造方法

* Class.forName("类的全称")不仅表示了类的类类型，还表示了动态加载类；

* 编译时刻加载类是静态加载类，运行时刻加载类是动态加载类；

* 通过new创建对象是静态加载类，在编译时刻就需要加载所有的可能使用到的类；

## 二、方法的反射

* 如何获得方法

　　方法名和方法的参数列表才能唯一决定某个方法；

* 方法的反射操作

　　method.invoke(Object,fields);//和直接调用的效果一样

* 为什么要用到方法的反射

　　指定方法名调用方法，例如通过标准的javabean的属性名获取其属性值

* 通过Class，Method来认识泛型的本质：

　　反射的操作都是编译后的操作，编译之后集合的泛型是去泛型化的

　　Java中集合的泛型是防止错误输入的，只在编译阶段有效，绕过编译就无效了（绕过编译就绕过了泛型）

## 三、成员变量的反射

获取成员变量信息

* 成员变量也是对象，Java.lang.reflect.Field

* Field类封装了关于成员变量的操作

* getFields()方法获取的是所有的Public的成员变量的信息

* getDeclaredFields()获取的是该类自己声明的成员变量的信息

## 四、构造方法的放射

* 构造函数也是对象，java.lang.Constructor中封装了构造函数的操作

* getConstructors获取所有的public的构造方法

* getDeclaredConstructors获取的是所有的构造函数

* 上面获取的构造函数，Constructor.getParameterTypes()得到的是参数列表的类类型

## 五、Java类的加载机制

* 类名.getName()，获取类的类类型的全程；

* 类名.getSimpleName()，获取类类型名称，不含包名

* getDeclaredMethods()获取的是所有该类自己声明的方法

* Method类，方法对象，一个成员方法就是一个Method对象

* gerMethods()获取类的方法，取的是所有public的函数，包括从父类继承而来的，不问访问权限

* getReturnType()获取方法的返回值的类类型

* getParameterTypes()得到参数列表的的类型的类类型



```java
public class ReflectTest {
    public static void main(String args[]){
        try {
            Class class1 = String.class;//创建对象class1
            //Class class2 = float.class;//创建对象class1
             
            Field[] field=class1.getFields();//获取class1的所有成员变量
            Method[] methods =class1.getMethods();//获取class1的所有方法
            Constructor[] constructor=class1.getConstructors();//获取class1的所有构造方法
             
            System.out.println("=======class1的成员变量======");
            for (Field field2 : field) {
                System.out.println(field2.getName());
            }
             
            System.out.println("=======class1的所有方法======");
             
            for (Method m : methods) {//遍历class1的所有方法
                System.out.print(m.getName()+"(");
                Parameter[] pms=m.getParameters();
                for (Parameter p : pms) {//遍历方法的参数列表
                    System.out.print(p.getType()+",");
                }
                //m.invoke(obj, args);
                System.out.println(")");
            }
            System.out.println("=======class1的所有构造方法======");
            for (Constructor constructor2 : constructor) {//遍历class1的所有构造方法
                System.out.print(constructor2.getName()+"(");
                Parameter[] pm2 = constructor2.getParameters();
                for (Parameter pc : pm2) {//遍历构造方法的参数列表
                    System.out.print(pc.getType().getName()+",");
                }
                System.out.println(")");
            }
             
        } catch (Exception e) {
            e.printStackTrace();
        }
         
    }
}
```

