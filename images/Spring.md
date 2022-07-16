# Spring

### 1.1简介



### 1.2优点

- Spring是一个开源免费的框架（容器）！
- Spring是一个轻量级、非侵入式的框架！
- 控制反转（IOC），面向切面编程（AOP）！*
- 支持事物的处理，对框架整合的支持！

总结：Spring就是一个轻量级的控制反转（IOC）和面向切面编程（AOP）的框架！



### 1.3组成

![image-20220620143729624](Spring.assets/image-20220620143729624.png)



### 1.4拓展

在Spring的官网有这个介绍：现代化的Java开发！说白了就是基于Spring的开发！

![image-20220620144109116](Spring.assets/image-20220620144109116.png)

- SpringBoot

  一个快速开发的脚手架。

  基于SpringBoot可以快速开发单个微服务。

  约定大于配置！

- SpringCloud

  SpringCloud是基于SpringBoot实现。

应为现在大多数公司都在使用SpringBoot进行快速开发，学习SpringBoot的前提是，需要完全掌握Spring及SpringMVC！

**弊端：发展太久之后，违背了原来的理念！配置十分繁琐，人称“配置地狱！”**

## 2、IOC理论推导

1.UserDao接口

2.UserDaoImpl实现类

3.UserService业务接口

4.UserSericeImpl业务实现类

在我们之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修 改源代码！如果程序代码量十分大，修改一次的成本代价十分昂贵！

![image-20220620215028396](Spring.assets/image-20220620215028396.png)

我们使用一个Set接口实现，已经发生了革命性的变化！

```java
private UserDao userDao;
    //利用set经行动态实现的注入！
    public void setUserDao(UserDao userDao) {
          this.userDao = userDao;
    }
```

- 之前，程序是主动创建对象！控制权在程序手上！
- 使用了set注入后，程序不再具有主动性，而是变成被动接收对象！



这种思想，从本质上解决了问题，我们程序员不用再去管理对象的创建了。系统的耦合性大大降低~，可以更加专注在业务的实现上！这是IOC的原型！

![image-20220620215115256](Spring.assets/image-20220620215115256.png)

### IOC本质

控制反转IOC（Inversion of Control)，是一种设计思想，ID（依赖注入）是实现IOC的一种方法，也有人认为DI只是程序IOC的另一种说法。没有IOC的程序中，我们使用面向对象编程，对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。



采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而是采用注解的方式可以把二者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。



**控制反转是一种通过描述（XML或注解）并通过第三方去生产获取第三方特定对象的方式。在Spring中实现控制反转的是IOC容器，其实现方法是依赖注入（Dependency Injection，DI）。**



## 3、HelloSpring

```java
import com.jingyu.pojo.Hello;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        //获取spring上下文对象！
       // ApplicationContext context = new ClassPathXmlApplicationContext("services.xml","daos.xml");
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //我们的对象现在都在spring中的管理了，我们要使用，直接去里面取出来就可以了！
       Hello hello =(Hello) context.getBean("hello");
        System.out.println(hello.toString());
    }
}

```

### 思考问题？

- Hello对象是谁创建的？

  hello对象是由Spring创建的

- Hello对象的属性是怎么设置的？

  hello对象的属性是由Spring容设置的，这个过程叫控制反转：

控制：谁来控制对象的创建，传统应用程序的对象是由程序本身控制创建的，使用Spring后，对象是由Spring来创建的。

反转：程序本身不创建对象，而变成被动的接收对象。

依赖注入：就是利用set方法来进行注入的。

IOC是一种编程思想，由主动的编程变成被动的接收。

可以通过newClassPathXmlApplicationContext去浏览一下底层源码。

**OK，到了现在，我们彻底不用在程序中去改动了，要实现不同的操作，只需要在xml配置文件中进行修改，所谓的IOC，一句话搞定：对象由Spring来创建，管理，配装！**



## 4、IOC创建对象方式

1、使用无参构造创建对象，默认！

2、假设我们使用有参构造创建对象。

​	1.下标给值

```xml
<!-- 第一种，下表给值！ -->
<bean id="user" class="com.jingyu.pojo.User">
   <constructor-arg index="0" value="哈哈哈"/>
</bean>
```

​	2.类型

```xml
 <!-- 第二种方式：通过类型创建，不建议使用！ -->
 <bean id="user" class="com.jingyu.pojo.User">
    <constructor-arg type="java.lang.String" value="哈哈哈"/>
 </bean>

```

​	3.参数名

```xml
    <!-- 第三种直接通过参数名来设置 -->
    <bean id="user" class="com.jingyu.pojo.User">
        <constructor-arg name="name" value="哈哈哈"/>
    </bean>
```

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了！

## 5、Spring配置

### 5.1别名

```xml
<!-- 别名,如果添加了别名，我们也可以使用别名获取这个对象 -->
<alias name="user" alias="one"/>
```

### 5.2Bean的配置

```xml
<bean id="userT" class="com.jingyu.pojo.UserT" name="userT3 userT2,t2;t3">
        <property name="name" value="嘻嘻嘻"/>
</bean>
<!--
   id:bean 的唯一标识符，也是就是相当于我们学的对象名
   class:bean 对象所对应的权限定名：包名+类型
   name：也是别名，而且name可以同时取多个别名
 -->
```

### 5.3import

这个import，一般用于团队开发使用，他可以将多个配置文件，导入合并为一个

applicationContext.xml

```xml
<import resource="beans.xml"/>
<import resource="beans2.xml"/>
<import resource="beans3.xml"/>
```

使用的时候，直接使用总配置就可以了

## 6、依赖注入

### 6.1构造器注入

以前已经说过了

### 6.2Set方式注入【重点】

依赖注入：set注入！

​	依赖：bean对象的创建依赖于容器！

​	注入：bean对象中的所有属性，由容器来注入！

【环境搭建】

1.复杂类型

```java
package com.jingyu.pojo;

public class Address {
    private String address;


    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}

```



2.真实测试对象

```java
package com.jingyu.pojo;

import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.Set;

public class Student {
    private String name;
    private Address address;
    private String [] books;
    private List<String> hobbys;
    private Map<String,String> card;
    private Set<String> games;
    private String wife;
    private Properties info;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String[] getBooks() {
        return books;
    }

    public void setBooks(String[] books) {
        this.books = books;
    }

    public List<String> getHobbys() {
        return hobbys;
    }

    public void setHobbys(List<String> hobbys) {
        this.hobbys = hobbys;
    }

    public Map<String, String> getCard() {
        return card;
    }

    public void setCard(Map<String, String> card) {
        this.card = card;
    }

    public Set<String> getGames() {
        return games;
    }

    public void setGames(Set<String> games) {
        this.games = games;
    }

    public String getWife() {
        return wife;
    }

    public void setWife(String wife) {
        this.wife = wife;
    }

    public Properties getInfo() {
        return info;
    }

    public void setInfo(Properties info) {
        this.info = info;
    }
}

```

3.beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="student" class="com.jingyu.pojo.Student">
        <!-- 第一种，普通值注入，value -->
        <property name="name" value="哈哈哈"/>
    </bean>



</beans>
```

4.测试类

```java
import com.jingyu.pojo.Student;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        Student student = (Student) context.getBean("student");
        //System.out.println(student.getName());
        System.out.println(student.toString());
        /*
        Student{
            name='哈哈哈',
            address=Address{address='长沙'},
            books=[红楼梦, 西游记, 三国演义, 水浒传],
            hobbys=[听歌, 吃饭, 碎觉, 敲代码],
            card={身份证=431199999999999999, 银行卡=673997981298793719},
            games=[LOL, COC, BOB], wife='null',
            info={password=123456, url=20220, driver=202206029, username=root}}
         */
    }
}

```

完善注入信息

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="address" class="com.jingyu.pojo.Address">
        <property name="address" value="长沙"/>
    </bean>
    <bean id="student" class="com.jingyu.pojo.Student">
        <!-- 第一种，普通值注入，value -->
        <property name="name" value="哈哈哈"/>
        <!-- bean注入，ref -->
        <property name="address" ref="address"/>
        <!-- 数组注入，ref -->
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>西游记</value>
                <value>三国演义</value>
                <value>水浒传</value>
            </array>
        </property>
        <!-- list -->
        <property name="hobbys">
            <list>
                <value>听歌</value>
                <value>吃饭</value>
                <value>碎觉</value>
                <value>敲代码</value>
            </list>
        </property>
        <!-- Map -->
        <property name="card">
            <map>
                <entry key="身份证" value="431199999999999999"/>
                <entry key="银行卡" value="673997981298793719"/>
            </map>

        </property>
        <!-- Set -->
        <property name="games">
            <set>
                <value>LOL</value>
                <value>COC</value>
                <value>BOB</value>
            </set>
        </property>

        <!--  null -->
        <property name="wife">
            <null/>
        </property>
        <!--  properties -->
        <property name="info">
            <props>
                <prop key="driver">202206029</prop>
                <prop key="url">20220</prop>
                <prop key="username">root</prop>
                <prop key="password">123456</prop>
            </props>
        </property>

    </bean>



</beans>
```



### 6.3拓展方式注入

我们可以使用p命名空间和c命名空间进行注入

官方解释：

![image-20220629122219420](Spring.assets/image-20220629122219420.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- p 命名空间注入，可以直接注入属性的值：property-->
    <bean id="user" class="com.jingyu.pojo.User" p:name="哈哈哈" p:age="23"/>

    <!-- c 命名空间注入，通过构造器注入：construct-args-->
    <bean id="user2" class="com.jingyu.pojo.User" c:name="嘻嘻嘻" c:age="22"/>
</beans>
```

测试

```java
  @Test
    public void test2(){
        ApplicationContext context = new ClassPathXmlApplicationContext("userbeans.xml");
        User user = context.getBean("user2",User.class);
        System.out.println(user);

    }
```

注意点：p命名和c命名空间不能直接使用，需要导入xml约束！

```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```

### 6.4bean的作用域

![image-20220629122905813](Spring.assets/image-20220629122905813.png)

1单例模式（spring默认机制）

```xml
<bean id="user2" class="com.jingyu.pojo.User" c:name="嘻嘻嘻" c:age="22" scope="singleton"/>
```

2.原型模式：每次从容其中get的时候，都会产生一个新对象！

```xml
<bean bean id="user2" class="com.jingyu.pojo.User" scope="prototype"/>
```

3.其余的request、session、application、这些个只能在web开发中使用到！

## 7、Bean的自动装配

- 自动装配是spring满足bean依赖一种方式！
- spring会在上下文中自动寻找，并自动给bean装配属性！

在spring有三种配置方式

1.在xml中显示的配置

2.在java中显示配置

3.隐式的自动装配bean【重要】

### 7.1测试

环境搭建：一个人由两个宠物！



### 7.2ByName自动装配

```xml

    <bean id="cat" class="com.jingyu.pojo.Cat"/>
    <bean id="dog" class="com.jingyu.pojo.Dog"/>

    <!--
     byName: 会自动在容器上下文中查找，和自己对象set方法后面的对应值的beanid！
     -->
    <bean id="people" class="com.jingyu.pojo.People" autowire="byName">
        <property name="name" value="哈哈哈"/>
        <!--<property name="cat" ref="cat"/>
        <property name="dog" ref="dog"/>-->
    </bean>
</beans>
```

### 7.3ByType自动装配

```xml
 <bean  class="com.jingyu.pojo.Cat"/>
 <bean  class="com.jingyu.pojo.Dog"/>
    <!--
     byName: 会自动在容器上下文中查找，和自己对象set方法后面的对应值的beanid！
     -->
    <!--
    byType：会自动获取容器上下文中查找，和自己对象属性类型相同的 beanid!
     -->
    <bean id="people" class="com.jingyu.pojo.People" autowire="byType">
        <property name="name" value="哈哈哈"/>
        <!--<property name="cat" ref="cat"/>
        <property name="dog" ref="dog"/>-->
    </bean>
```

小结：

- byname的时候，需要保证所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致！
- bytype的时候，需要保证所有bean的class唯一，并且这个bean需要和自动注入的属性的类型一致！

### 7.4使用注解实现自动装配

jdk1.5支持的注解，spring2.5就开始支持注解了！

The introduction of annotation-based configuration raised the question of whether this approach is “better” than XML.

要使用注解须知：

1.导入约束。context约束

2.配置注解的支持：<context:annotation-config/>【重要！】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

```java
@Autowired
```

直接在属性上使用即可！也可以在set方式上使用！

使用Autowired我们可以不用编写set方法了，前提是你这个自动装配的属性在IOC（spring）容器中存在，且符合名字byname！

科普：

```Xml
@Nullable 字段标记了这个注解，说明这个字段可以为null
```

```java
public @interface Autowired {
    boolean required() default true;
}
```

测试代码

```java
public class People {

    //如果显示定义了Autowired的required属性为false，说民这个对象可以为null，否则不允许为空
    @Autowired(required = false)
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
}
```

如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候、我们可以通过使用@Qualifier(value = "xxx")去配置@Autowired的使用，指定一个唯一的bean对象注入！

```java
public class People {

    @Qualifier(value = "cat1")
    private Cat cat;
    @Qualifier(value = "dog2")
    private Dog dog;
    private String name;
}
```

**@Resource注解**

```java
public class People {

    @Resource(name = "cat2")
    private Cat cat;
    @Resource
    private Dog dog;
    private String name;
}
```

小结：

- @Resource 和 @Autowired的区别：
- 都是用来自动装配的，都是可以放在属性字段上
- @Autowired 通过byname的方式实现
- @Resource默认通过byname的方式实现，如果好不到名字，则通过byType实现！如果两个都找不到的情况下，就报错！【常用】
- 执行顺序不同：@Autowired通过byType的方式实现。@Resource默认通过byname的方式实现。

## 8、使用注解开发

在Spring4之后，需要用注解开发，必须要保证aop的包导入了

![image-20220705155357443](Spring.assets/image-20220705155357443.png)

使用注解需要导入context约束，增加注解的支持！

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
    <!-- 开启注解的支持 -->
    <context:annotation-config/>
</beans>
```

1.bean

2.属性如何注入

```xml
package com.jingyu.dao;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

//等价于<ben id="user" class="com.jingyu.pojo.User"/>
//@Component 组件
@Component
public class User {

    //相当于<property name = "name" value = "hahaha"/>
    @Value("hahaha")
    public String name;

    public void setName(String name) {
        this.name = name;
    }
}

```



3.衍生的注解

@Component有几个衍生注解，我们在web开发中，会按照mvc三层架构分层！

- ​	dao	【@Repository】
- ​	service   【@Service】
- ​	controller  【@Controller】

这四个注解功能都是一样的，都是代表将某个类注册到spring中，装配Bean



4.自动装配置

```
-@Autowired :自动装配通过类型。名字
	如果Autowired 不能唯一自动装配上属性，则需要通过@Qualifier(value = "xxx")
-@Nullable :字段标记了这个注解，说明这个字段可以为null
-@Resource :自动装配通过名字。类型。
```



5.作用域

```java
package com.jingyu.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

//等价于<ben id="user" class="com.jingyu.pojo.User"/>
//@Component 组件
@Component
//value = "prototype" 原型模式 value = “singleton” 单列模式
@Scope(value = "prototype")
public class User {

    //相当于<property name = "name" value = "hahaha"/>
    @Value("hahaha")
    public String name;

    public void setName(String name) {
        this.name = name;
    }
}

```



6.小结

xml 与注解：

- ​	xml更加方便，适用于任何场合！维护简单方便
- ​	注解不是自己的类使用不了，维护相对复杂！

xml 与注解最佳实践

- xml用来管理bean；
- 注解只负责完成属性的注入；
- 我们在使用的过程中，只需要注意一个问题：必须让注解生效，就需要开启注解的支持

```xml
<!-- 指定要扫描的包，这个包下的注解就会生效 -->
<context:component-scan base-package="com.jingyu"/>
<context:annotation-config/>
```



## 9、使用Java的方式配置Spring

我们现在完全不使用Spring的xml配置了，全权交给Java来做！

JavaConfig是Spring的一个项目，在Spring4之后，它成为一个核心功能！

![image-20220705172228187](Spring.assets/image-20220705172228187.png)



实体类

```java
package com.jingyu.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

//这里这个注解的意思，就是说名这个类被spring接管了，注册到容器中
@Component
public class User {
    private String name;

    public String getName() {
        return name;
    }

    @Value("hahaha") //属性注入值
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}

```



配置文件

```java
package com.jingyu.config;

import com.jingyu.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

//这个也会spring容器托管，注册到容器中，因为他们本来就是一个@Component
// @Configuration代表这是一个配置类，就和我们之前看的beans.xml
@Configuration
@ComponentScan("com.jingyu.pojo")
@Import(jingConfig2.class)
public class Jingconfig {

    //注册一个bean,就相当于我们之前写的一个bean标签
    //这个方法的名字，就相当于dean标签中的ID属性
    //这个方法的返回值，就相当于bean标签的class属性
    @Bean
    public User getUser(){
        return new User(); //就是返回要注入到bean的对象！
    }
}

```



测试类！

```java
import com.jingyu.config.Jingconfig;
import com.jingyu.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        //如果完全使用了配置类方式去做，我们就只能通过AnnotationConfig 上下文来获取容器，通过配置类的class对象加载！
        ApplicationContext context = new AnnotationConfigApplicationContext(Jingconfig.class);
        User getUser = (User) context.getBean("getUser");
        System.out.println(getUser.getName());
    }
}

```

这种纯Java的配置方式，在SpringBoot中随处可见！



## 10、代理模式

为什么要学习代理模式？因为这就是SpringAOP的底层！【SpringAOP和SpringMVC】

代理模式分类：

​	静态代理

​	动态代理

![image-20220705205730836](Spring.assets/image-20220705205730836.png)



### 10.1静态代理

角色分析：

- ​	抽象角色：一般会使用接口或者抽象类来解决
- ​	真实角色：被代理的角色
- ​	代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作
- ​	客户：访问代理对象的人！

代理步骤：

1.接口

```java
package com.jingyu.demo01;
//租房
public interface Rent {

    public void rent();
```



2.真实角色

```java
package com.jingyu.demo01;

//房东
public class Host implements Rent {
    @Override
    public void rent() {
        System.out.println("房东要租房子！");
    }
}

```



3.代理角色

```java
package com.jingyu.demo01;

public class Proxy implements Rent{
    private Host host;

    public Proxy() {
    }

    public Proxy(Host host) {
        this.host = host;
    }

    @Override
    public void rent() {
        seeHouse();
        host.rent();
        hetong();
        fare();
    }
    //看房
    public void seeHouse(){
        System.out.println("中介带你看房");
    }

    public void hetong(){
        System.out.println("签租赁合同");
    }

    //收中介费
    public void fare(){
        System.out.println("收中介费");
    }
}

```



4.客户端访问代理角色

```java
package com.jingyu.demo01;

public class Client {
    public static void main(String[] args) {
        //房东要租房子
        Host host = new Host();
        //代理，中介房东租房子，但是呢？代理角一般会一些附属操作！
        Proxy proxy = new Proxy(host);

        //你不用面对房东，直接找中介租房即可！
        proxy.rent();
    }
}
```



代理模式的好处：

- 可以使真实角色的操作更加纯粹！不用去关注一些公共业务
- 公共也就就交给代理角色！实现了业务的分工！
- 公共业务发生扩展的时候，方便集中管理！

缺点：

- 一个真实角色就回产生一个代理角色；代码量会翻倍~开发效率会变低~



### 10.2加深理解

代码：对应08-demo02；

聊聊AOP

![image-20220705231356560](Spring.assets/image-20220705231356560.png)

### 10.3动态代理

- 动态代理和静态代理角色一样
- 动态代理的代理类是动态生成的，不是我们直接写好的！
- 动态代理分为两大类：基于接口的动态代理，基于类的动态代理
  - 基于接口---JDK动态代理【我们在这里使用】
  - 基于类：cglib
  - Java字节码实现：javasist

需要了解两个类：proxy：代理，InvocationHandler：处理



**InvocationHandler**

动态地理的好处

- 可以使用真实角色的操作更加纯粹！不用去关注一些公共的业务
- 公共也就就交给代理角色！实现了业务的分工！
- 公共业务发生扩展的时候，方便集中管理！
- 一个动态代理类代理的是一个接口，一般就是对应的一类业务
- 一个动态代理类可以代理多个类，只要是实现了同一个接口即可！

## 11、AOP

### 11.1什么是AOP

AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是利用OOP的延续，是软件开发中的一个热点，也是spring框架中一个重要内容，是函数式编程的一种衍生泛型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑部分之间的耦合度降低，提高程序的可用性，同时提高开发的效率。

![image-20220706190435258](Spring.assets/image-20220706190435258.png)

### 11.2AOP在spring中的作用

**提供声明式事务；允许用户自定义切面**

- 横切关注点：跨域应用程序多个模块的方法或者功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等......
- 切面（ASPECT）：横切关注点被模块化的特殊对象。即，它是一个类。
- 通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法。
- 目标（Target）：被通知对象。
- 代理（proxy）：向目标对象对应通知之后创建的对象。
- 切入点（PointCut）：切面通知执行的“地点”的定义。
- 连接点（JointPoint）：与切入点匹配的执行点。

![image-20220706191809718](Spring.assets/image-20220706191809718.png)

springAOP中，通过Advice定义横切逻辑，spring中支持5种类型的Advice：

![image-20220706192138256](Spring.assets/image-20220706192138256.png)

即AOP在不改变原有代码的情况下，去增加新的功能。

### 11.3使用spring实现AOP

【重点】使用AOP织入，需要导入一个依赖包！

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
	<groupId>org.aspectj</groupId>
	<artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

方式一：使用spring的api接口【主要spring api接口实现】

方式二：使用自定义类【主要是切面定义】

方式三：使用注解实现！ 



## 12、整合Mybatis

步骤：

1. 导入相关jar包

   junit

   mybatis

   MySQL数据库

   spring相关的

   aop织入

   mybatis-spring【new】

2. 编写配置文件

3. 测试



### 12.1回忆mybatis

1. 编写实体类
2. 编写配置文件
3. 编写接口
4. 编写Mapper.xml
5. 测试

### 12.2、Mybatis-spring

1. 编写数据源配置
2. SQLSessionFactory
3. sqlSessionTemplate
4. 需要给接口加实现类【】
5. 将自己的类实现，注入到Spring中
6. 测试使用即可！

## 13、声明式事务

### 1、回顾事务

- 把一组业务当成一个业务来做；要么都成功，要么都失败！
- 事务在项目开发中，十分重要，涉及到数据的一致性问题，不能马虎！
- 确保完整性和一致性；



事务ACID原则：

- 原子性
- 一致性
- 隔离性
  - 多个业务可以操作同一个资源，防止数据损坏
- 持久性
  - 事务一旦提交，无论系统发生什么问题，结果都不会再被影响，被持久化的写到存储器中！

### 2、spring中的事务管理

- 声明式事务：AOP
- 编程式事务：需要再代码中，进行事务管理

思考：

为什么要事务？

- 如果不配置事务，可能存放在数据提交不一致的情况下；
- 如果我们不在spring中去配置声明式事务，我们就需要在代码中手动配置事务！
- 事务在项目的开发中十分重要，设计到数据的一致性和完整性问题，不容马虎！