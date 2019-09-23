title: Spring简介
date: 2015-01-22 10:51:54
tags: [Java, Spring]
---
相对于Hibernate（冬眠），Spring（春天），具有更多的诗意与希望的感觉，是为了解决传统J2EE开发效率过低、开发商之间不统一、没有真正实现“写一次到处使用”，它的优点有如下：

> 1. 低侵入式设计，**代码污染极低**。
2. 独立于各种应用服务，真正实现**写一次到处都可以使用**。
3. 用户可选择的自由度高，用户可以**选择部分或者是全部SPRING的功能**，它并不是设计来取代其它框架，可以和其它的框架（如STRUTS、HIBERNATE）等结合极好。
4. 面向接口的编程方式，**使得代码的偶合度降到最低**。
5. 所有的BEAN默认都被会单态化，相同的BEAN只会被初使化一次，因而**节省了BEAN初使化的时间及减少垃圾回收，增加了应用效率**。

有以上的优点的结合，因而它是被广大程序员所欢迎的，因为它可以给我们带来高效、稳定的开发，很大程度的减少了程序的开发、维护周期，也就自然的减少了软件开发的费用。下面简单的介绍两个应用示例，这些示例都来自于书本，都简单易懂，我也会详细的加以说明，另外需要使用下面的示例，需要引用Spring的JAR包：Spring.jar，该JAR包包括了大部份的应用，如果暂时不需要SPRINT的其它功能，该JAR足以。
以下的程序，全部都是调试通过的。
##示例一、不同的人说不同的话
---
###1. 建立接口
工厂模式在SRPING中是随处体现，且提倡面向接口，因此首先建立接口：人
```
package test;
public interface Person {
 public void sayHello();
 public void sayBye();
}
```
###2. 建立两种具体的人：中国人、美国人
```
//中国人
package test;
public class Chinese implements Person {
 public void sayBye() {
  System.out.println("再见");
 }
 public void sayHello() {
  System.out.println("你好");
 }
}
//美国人
package test;
public class American implements Person {
 public void sayBye() {
  System.out.println("Bye");
 }
 public void sayHello() {
  System.out.println("hello");
 }
}
```
###3. 建立bean映射配置文件
bean.xml（这个文件名是什么没有关系，在初使化的时候指定给初使化程序就可以）
```
<?xml version="1.0" encoding="gb2312"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN//EN"
 "http://www.sprintframework.org/dtd/spring-beans.dtd">
<beans>
<bean id="chinese" class="test.Chinese"/>
<bean id="american" class="test.American"/>
</beans>
```
###4. 建立JAVA测试类：
```
public void doIt(){
  /*获取bean映射配置文件，配置文件放于与当前测试类在同一个目录下*/
  ApplicationContext ctx=new FileSystemXmlApplicationContext(getClass().getResource("bean.xml").toString());
  Person person=null;
  person=(Person)ctx.getBean("chinese");
  person.sayHello();
  person.sayBye();
  
  person=(Person)ctx.getBean("american");
  person.sayHello();
  person.sayBye();
 }
```
###5. 对以上示例调用的说明：
A. 对接口Person和具体实现类Chinese、American就没有什么需要说明的了，和其它的编程方式，都是先定义接口，实现类再通过继承接口实现其方法。
B. 映射类中将类的路径定义为一个id的名称，到时系统根据传到的id名称，到配置文件中去找到该类的绝对路径，再通过反射原理加载该类，并返回其对象，这个是通过getBean这个动作完成的。
C. 根据里氏代换原则，能够接收父类的地方，都可以接收子类，所以这个时候通过getBean返回的对象（如Chinese或者American），都可以赋给接口Person，这个时候接口类调用其中的方法的时候，因为这个时候父类实际上接受的是子类的对象，因而这个时候调用的就是指定子类的方法。这个时候Spring就帮我们完成了如下工作：

    Person person=new Chinese();

只是子类不是通过显示给出来的，而是通过Spring工厂通过映射配置生成的。
 
##示例二、设值注入
---
**设值注入**是**依赖注入**的一种，**依赖注入**早期叫**控制反转(IOC)**，也可以称**反射**，他们的意义都相同。当某个 Java 实例(调用者)需要另一个Java 实例(被调用者)时，在传统的程序设计过程中，通常由调用者来创建被调用者的实例。而在依赖注入的模式下，创建被调用者的工作不再由调用者来完成，通常由 Spring 容器来完成，然后注入调用者，因此称为**控制反转**，也称为**依赖注入**。
###1. 建立接口：人与斧头
```
//人，定义一个动作：
package test2;
public interface Person {
 public void useAxe();
}
//斧头，定义一个动作：
package test2;
public interface Axe {
 public void chop();
}
```
###2. 建立接口的实现类：中国人与石斧
```
//中国人
package test2;
public class Chinese implements Person{
 /*默认无参构造方法不管为私的还是公有的，都可以访问，并且要保证bean中存在可以被外界访问的无参构造函数*/
 private Chinese(){};
 /*定义需要被使用的斧头接口，具体使用什么斧头这里不管*/
 private Axe axe;
 /*定义被注入斧头的set方法，该方法一定要符合JAVABEAN的标准。在运行时候，
  *Sping就会根据配置的<ref local=""/>，找到需要注入的实现类*/
 public void setAxe(Axe axe){
  this.axe=axe;
 }
 /*这个时候使用的axe，就不再是接口Axe本身，而是被注入的子类实例，所以这里的chop()动作就是具体子类的chop动作*/
 public void useAxe() {
  axe.chop();
 }
}
//石斧
package test2;
public class StoneAxe implements Axe {
 public void chop() {
  System.out.println("石斧慢慢砍");
 }
}
```
###3. 建立映射配置文件bean.xml：
```
<?xml version="1.0" encoding="gb2312"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN//EN"
 "http://www.sprintframework.org/dtd/spring-beans.dtd">
<beans>
<bean id="chinese" class="test2.Chinese">
 <!-- 声明实现类test2.Chinese中的属性 -->
 <property name="axe">
  <!-- 指定其中声明的属性，需要用本地的那个id对应的class
    这里local的值为"stoneAxe"，表示axe的属性值在注入的时候，
    将会用test2.StoneAxe实例注入，到时在实例类Chinese中使用
    axe的时候，实际上调用的时候StoneAxe的实例
   -->
  <ref local="stoneAxe"/>
 </property>
</bean>
<bean id="stoneAxe" class="test2.StoneAxe"/>
</beans>
```
###4. 测试用例：
```
package test2;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.FileSystemXmlApplicationContext;
public class Caller {
 public static void main(String[] args) {
  Caller caller=new Caller();
  caller.doIt();  
 }
 public  void doIt(){
  String bean=getClass().getResource("bean.xml").toString();
  ApplicationContext ctx=new FileSystemXmlApplicationContext(bean);
  Person person=(Person)ctx.getBean("chinese");
  person.useAxe();
 }
}
```
###5. 对以上示例调用的说明：
程序及XML文件中都有详细说明，这里就不说明了。

##三、构造注入
---
构造注入即是通过构造函数进行注入，到目前为止，SPRINT支持**设值注入**与**构造注入**两种方式，它们可以同时存在。
现在采用的实例为了和设值注入进行对比，因此使用的程序都是设值注入的程序，其中的接口和测试实例都不需要修改，修改的只是需要被注入的实现类Chinese，以及配置文件，不同之处用加粗体表示。

###1. 修改实现类Chinese
```
package test2;
public class Chinese implements Person{
 /*默认无参构造方法不管为私的还是公有的，都可以访问，并且要保证bean中存在可以被外界访问的无参构造函数*/
 private Chinese(){}; 
 /*定义需要被使用的斧头接口，具体使用什么斧头这里不管*/
 private Axe axe;
 public Chinese(Axe axe){
  this.axe=axe;
 }
 /*这个时候使用的axe，就不再是接口Axe本身，而是被注入的子类实例，所以这里的chop()动作就是具体子类的chop动作*/
 public void useAxe() {
  axe.chop();
 }
}
```
###2. 修改配置文件bean.xml
```
<?xml version="1.0" encoding="gb2312"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN//EN"
 "http://www.sprintframework.org/dtd/spring-beans.dtd">
<beans>
<bean id="chinese" class="test2.Chinese">
<!-- 定义需要被构造注入的实现类，同设值注入的结果一样，都是注入接口的实现类 -->
 <constructor-arg><ref bean="stoneAxe"/></constructor-arg>
 </bean>
<bean id="stoneAxe" class="test2.StoneAxe"/>
</beans>
```
###3. 设值注入与构造注入比较
它们实现的结果是一样的，**都在将需要被调用的实现类注入到调用的实现类。**
*设值注入与传统JAVABEAN的写法一样，比较容易接受；而构造注入在应用程序加载的时候就已经完成了注入，可以控制加载顺序。*各有优缺点，根据不同的情况选用了。
 
##四、后记
编程中理解与使用SPRING就像是人从学走路到跑步的过程，如果没有理解什么是**单态模式**、**工厂模式**、**反射**、**JAVABEAN**、**XML基础**、**接口及其实现**等等，理解SPRING就会比较困难，就算是可以使用通，也不一定知道为什么，看看网上的一些信息，动不动要求说会不会SPRING、HIBERNATE等等，很少提到基础知识是否扎实。我在面试别人的过程中，往往更注意面试人员的程序基础、编程风格、动手能力、文档编写能力等，当然对当前一些流行的框架也会有提问，毕竟好的东西也是需要借签的。

---
本文转载自[冯立彬的博客][1]


  [1]: http://blog.csdn.net/fenglibing/article/details/4300517