# SSH框架简介

### 一、什么是SSH框架？

SSH框架是Struts+Spring+Hibernate集成的web应用程序开源框架。

**Struts：**用来控制的，核心控制器是Controller。
**Spring：**对Struts和Hibernate进行管理整合的。
**Hibernate：**操控数据库。

### 二、SSH是怎样一个流程？

SSH从职责上分为四层：表示层（Struts）、业务逻辑层（Spring）、数据持久层（Hibernate）。

整体的调用关系：前台页面->Action->Service->Dao->数据库

在表示层，首先通过前台页面实现交互，负责接收请求（request）和传送请求（response），Struts根据配置文件（Struts.xml）将ActionServlet（Struts的内置核心控制器组件）接收到的Request请求委派给Action处理。

在业务层中，管理服务器组建的Spring IOC容器负责向Action提供业务模型
（Model）组件并和该组件的协作对象处理（Dao）组件完成业务逻辑，并提供事务处理、缓冲池等容器组件以提升和保持数据的完整性。

在持久层，依赖于Hibernate的对象化映射和数据库交互，处理Dao组件请求的数据，并返回处理结果。

### 三、SSH框架有什么优点？

1.Spring管理对象的实例化，把对象的创建和获取放到外部,更加的灵活方便。
2.Hibernate避免了JDBC连接连接数据库的冗余繁杂。
3.各层分工明确，实现了各层之间的解耦额，代码更加灵活。


【参考】

http://blog.csdn.net/wangmei4968/article/details/49364101
