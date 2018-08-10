
spring   学习

# Spring  概念

1.spring是开源的轻量级的框架

2.spring核心主要两部分  

- aop：面向切面编程，扩展功能不是修改源代码实现。  

- ioc：控制反转。比如一个类，在类里面有个方法（不是静态方法），调用类里面的方法步骤是：创建类的对象，使用对象来调用这个方法，这里创建类对象需要使用new的方式来创建。
把对象的创建交给spring，采用配置文件的方式来创建对象，这个就是控制反转。


   

3.spring是一站式框架

- spring在java EE三层结构中，每一层都提供不同的解决技术
- web层： spring MVC
- service层： spring IOC
- dao层： spring jdbcTemplate

4.spring版本

- hibernate用的5.x版本
- spring用的4.x版本

# Spring的IOC操作

1.把对象的创建交给spring进行管理

2.ioc操作两种方式

- 配置文件的方式
- 注解的方式

3.IOC底层原理

ioc底层原理使用的技术

- xml配置文件
- dom4j解析xml
- 工厂设计模式
- 反射 



























