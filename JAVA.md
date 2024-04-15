# Java

## JavaSE

### Java基础语法

#### 标识符和关键字

#### 数据类型

- 基本数据类型：
  整型 byte short int long 
  浮点型 float double 
  字符型 char 
  boolean型
- 引用数据类型：
  类
  接口
  数组

#### 类型转换

#### 变量，常量，作用域

- 类变量
- 实例变量
- 局部变量

```java
public class Demo08 {
    // 类变量 static
    static double salary=2500;

    // 实例变量：从属于对象
    String name;
    int age;

    // main方法
    public static void main(String[] args) {
        // 局部变量
        int i = 10;
    }

    public void add(){

    }
}
```

#### 运算符

#### 包机制

#### JavaDoc生成文档

命令行
javadoc 参数 java文件

### Java流程控制

#### 用户交互Scanner

next() 只读取有效字符，不能得到空格

nextline() 以enter为结束符，可以获得空格

```java
import java.util.Scanner;

public class Demo02 {
    public static void main(String[] args) {
        Scanner s =  new Scanner(System.in);

        System.out.println("使用nextline方式接收：");

        if(s.hasNextLine()){
            String str = s.nextLine();
            System.out.println("输入的内容为:"+str);
        }

        s.close();
    }
}
```

#### 顺序结构

#### if选择结构

#### switch选择结构

#### while选择结构

#### dowhile

#### for循环、增强for循环

#### break、continue、goto



### Java方法

#### 方法的定义和调用

#### 方法的重载

#### 可变参数

```java
    public void test(int... i){
        System.out.println(i);
    }
```



### Java数组

#### 数组的声明和创建

```java
int[] nums; //声明一个数组
int nums2[]; 

nums = new int[10]; //创建一个数组

//给数组元素赋值
nums[0] = 1;
nums[1] = 2;
```

#### 三种初始化

```java
	//静态初始化
	int[] a = {12,3,4,5,3,6};
	Man[] mans = {new Man(),new Man()};

    //动态初始化
    int[] b = new int[10];

//数组的默认初始化
/*数组是引用类型，它的元素相当于类的实例变量，因此数组一经分配空间，其中的每个元素也被按照实例变量同样的方式被隐式初始化。*/
```

#### 多维数组

```java
int[][] array = {{1,2},{2,3},{3,4},{3,4}};

for (int i = 0; i < array.length; i++) {
    for (int j = 0; j < array[i].length; j++) {
        System.out.println(array[i][j]);
    }
```

#### Arrays类

数组的工具类java.util.Arrays

```java
//打印数组元素Arrays.toString
System.out.println(Arrays.toString(a));

//排序Arrays.sort
Arrays.sort(a);
```

#### 稀疏数组



### 面向对象

#### 创建与初始化对象

```java
Student xiaoming = new Student();
xiaoming.name="xiao名";
```

#### 构造器

构造方法

#### 封装

#### 继承

extends

##### super

```java 
    public Student(){
        super();//隐藏代码：调用父类的构造器
        //如果父类只有有参构造，这里只能调用父类的有参
        //this("alien");//构造器只能调用一个
        System.out.println("student无参构造");
    }
    public Student(String name) {
        this.name = name;
    }

super.print();//调用父类方法
super.name;//调用父类变量
```

##### 方法重写

//静态的方法和非静态的方法区别很大

静态方法：方法的调用只和左边，定义的数据类型有关
非静态：重写

#### 多态

```java
//一个对象的实际类型是确定的
//new Student();
//new Person();

//可以指向的引用类型就不确定了，父类的引用指向子类
Student s1 = new Student();
Person s2 = new Student();
Object s3 = new Student();
```

##### instanceof

instanceof关键字

```java

System.out.println(X instanceof Y); //能不能编译通过 - 有没有父子关系
//是否为true - X是不是Y的子类型

```

**类型转换**

```java
//高                    低
Person obj = new Student();
//Person转换为Student类型
Student s1 = (Student)obj;
s1.go(); 
// ((Student)obj).go();

//子类转换为父类，可能丢失自己的本来的一些方法
Student student = new Student();
student.go();
Person person = student;
```

#### static

```java
{
    //代码块（匿名代码块）
}

static {
    //静态代码块
}
```

**final** 变量

#### 抽象类

abstract

```java
//抽象类
public abstract class Action {

    //抽象方法
    public abstract void doSomething();

    //1.不能new这个抽象类
    //2.抽象类里可以写普通方法
    //3.抽象方法必须在抽象类中
    public void hello(){}
}
```

#### 接口	

interface

```java
//interface定义的关键字，接口都需要有实现类
public interface UserService {
    //接口中的所有定义其实都是抽象的 public abstract
    void add(String name);
}

public interface TimeService {
    void timer();
}

//利用接口实现多继承
public class UserServicelmpl implements UserService,TimeService{
    @Override
    public void add(String name) {}

    @Override
    public void timer() { }
}
```

#### N种内部类



### 异常处理

#### Error和Exception

#### 捕获和抛出异常

try、catch、finally、throw、throws

```java
        try {
            System.out.println(a / b);
        } catch (ArithmeticException e) {
            System.out.println("除数不能为0");
        } finally {
            System.out.println("Done");
        }
```

### 常用类学习

### 集合框架详解

### I/O

### 多线程

### 网络编程

### GUI编程

### 注解和反射

### JUC并发编程

### JVM



## JDBC

### 数据库驱动



## JavaWeb

### 3、Web服务器-Tomcat

网站根目录在 apache-tomcat-9.0.64\webapps\XXX

### 4、HTTP

HTTP/1.0
HTTP/1.1

http请求、http响应、响应码

### 5、Maven

#### Maven架构管理工具

方便导入jar包

Maven的核心思想：**约定大于配置**

#### 本地仓库

建立一个本地仓库 localRepository

### 6、Servlet

#### 6.2、HelloServlet

Servlet接口Sun公司有两个默认的实现类：HttpServlet，GenericServlet

1. 构建一个普通的Maven项目，删除src目录，以后在这个项目里面建立Moudel；这个空的工程就算Maven主项目；

2. 关于Maven父子工程的理解：

   父项目中会有

   ```java
   <modules>
       <module>servlet-01</module>
   </modules>
   ```

   子项目会有

   ```java
   <parent>
   ```

   父项目中的java子项目可以直接使用

   ```java
   son extends father
   ```

3. Maven环境优化
   1. 修改web.xml为最新的
   2. 将maven的结构搭建完整
   
4. 编写一个servlet
   1. 编写一个普通类
   
   2. 实现Servlet接口，这里继承HttpServlet
   
      ```java
      public class HelloServlet extends HttpServlet {
          //由于get post只是请求实现的不同的方式，可以相互调用，业务逻辑都一样
      
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              //ServletOutputStream outputStream = resp.getOutputStream();
              PrintWriter writer = resp.getWriter(); //响应流
              writer.println("Hello,Servlet");
          }
      
          @Override
          protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              doGet(req, resp);
          }
      }
      ```

5. 编写Servlet的映射

   为什么需要映射：我们写的是AVA程序，但是要通过浏览器访问，而浏览器需要连接Web服务器，所以我们需要在web服务中注册我们写的Servlet,还需给他一个浏览器能够访问的路径；

6. 配置Tomcat

7. 启动测试

#### 6.3、Servlet原理

![servlet](./Image\Java\servlet.png)

#### 6.4、Mapping问题

1. 一个Servlet可以指定一个映射路径

   ```java
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/hello</url-pattern>
   </servlet-mapping>
   ```

2. 一个Servlet可以指定多个映射路径

   ```java
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/hello</url-pattern>
   </servlet-mapping>
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/hello2</url-pattern>
   </servlet-mapping>
   ```

3. 一个Servlet可以指定通用映射路径

   ```java
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/hello/*</url-pattern>
   </servlet-mapping>
   ```

4. 默认请求路径

   ```java
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/*</url-pattern>
   </servlet-mapping>
   ```

5. 指定一些后缀或者前缀等...

   ```java
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>*.do</url-pattern>
   </servlet-mapping>
   ```

6. 优先级问题

   指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求；

#### 6.5、ServletContext

web容器在启动的时候，它会为每个web程序都创建一个对应的ServletContext对象，它代表了当前的web应用；

<img src="./Image\Java\ServletContext.png" alt="ServletContext" style="zoom: 50%;" />

##### 1、共享数据

我在这个Servlet中保存的数据，可以在另外一个servlet中拿到；

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //this.getInitParameter() 初始化参数
    //this.getServletConfig() servlet配置
    //this.getServletContext() servlet上下文
    ServletContext context = this.getServletContext();

    String username = "埃列娜"; //数据
    context.setAttribute("username",username); //将一个数据保存在了ServletContext中，名字为username 值username
}
```

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    ServletContext context = this.getServletContext();
    String username = (String) context.getAttribute("username");

    resp.setContentType("text/html");
    resp.setCharacterEncoding("utf-8");
    resp.getWriter().print("名字："+username);
}
```

```xml
<servlet>
    <servlet-name>hello</servlet-name>
    <servlet-class>com.alien.servlet.HelloServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>hello</servlet-name>
    <url-pattern>/hello</url-pattern>
</servlet-mapping>

<servlet>
    <servlet-name>getc</servlet-name>
    <servlet-class>com.alien.servlet.GetServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>getc</servlet-name>
    <url-pattern>/getc</url-pattern>
</servlet-mapping>
```

测试访问结果；

##### 2、获取初始化参数

```xml
<!--    配置一些web应用的初始化参数-->
<context-param>
    <param-name>url</param-name>
    <param-value>jdbc:mysql://localhost:3306/mybatis</param-value>
</context-param>
```

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    ServletContext context = this.getServletContext();

    String url = context.getInitParameter("url");
    resp.getWriter().print(url);
}
```

##### 3、请求转发

```java

    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();

//        RequestDispatcher requestDispatcher = context.getRequestDispatcher("/gp"); //转发的请求路径
//        requestDispatcher.forward(req,resp); //调用forward()实现请求转发

        context.getRequestDispatcher("/gp").forward(req,resp);
    }
```

##### 4、读取资源文件





## SSM框架



