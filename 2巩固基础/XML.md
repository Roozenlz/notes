# XML配置文件详解

## 概念

> Extensible Markup Language 可扩展标记语言,可扩展：标签都是自定义的。

![约束](http://tc.roozen.xyz/image/%E7%BA%A6%E6%9D%9F.bmp)

### 功能

存储数据：

1. 配置文件
2. 在网络中传输

### xml与html的区别

1. xml标签都是自定义的，html标签是预定义
2. xml的语法严格，html语法松散
3. xml是存储数据的，html是展示数据



## 语法

### 基本语法

1. xml文档的后缀名 .xml
2. xml第一行必须定义为文档声明(必须为实际意义上的第一行)
3. xml文档中有且仅有一个根标签
4. 属性值必须使用引号(单双都可)引起来
5. 标签必须正确关闭（自封闭或者闭标签都可）
6. xml标签名称区分大小写(html不区分)

实例：

```xml
<?xml version='1.0' ?>
<!--上方为文档说明，必须处在第一行-->
<users>
    <user id='1'>
        <name>zhangsan</name>
        <age>23</age>
        <gender>male</gender>
        <br/>
    </user>
    <user id='2'>
        <name>lisi</name>
        <age>24</age>
        <gender>female</gender>
    </user>
</users>
```

### 组成部分

- 文档声明

  - 格式：\<?xml 属性列表?\>

  - 属性列表：

    - version:版本号，有1.0和1.1，大多用1.0，必须要有的属性
    - encoding:编码方式。默认为ISO-8859-1
    - standalone:是否独立，值为yes(不依赖其他文件)或no

  - 指令(了解)：结合CSS的

    `<?xml-stylesheet type="text/css" href="a.css" ?>`

  - 标签：自定义的

    - 规则：
      1. 名称可以包含字母、数字以及其他的字符 
      2. 名称不能以数字或者标点符号开始 
      3. 名称不能以字母 xml（或者 XML、Xml 等等）开始 
      4. 名称不能包含空格 

  - 属性：id值唯一

  - 文本：

    - CDATA区:在该区域中的数据会原样展示

      格式：`<![CDATA[ 展示数据 ]]>`

      ```xml
      <?xml version="1.0" encoding="utf-8" standalone='no' ?>
      <!--<?xml-stylesheet type="text/css" href="a.css" ?>-->
      <root>
          <code>
              <!-- if(a &lt; b &amp;&amp; a &gt; c){}-->
              <![CDATA[
                  if(a < b && a > c) {}
              ]]]>
      	</code>
      </root>
      ```

      

### 约束

> 规定xml文档的书写规则

作为框架的使用者(程序员)：

1. 能够在xml中引入约束文档
2. 能够简单的读懂约束文档



#### 分类

##### DTD:一种简单的约束技术

一个简单的dtd约束文件

```dtd
<!ELEMENT students (student+) >
<!ELEMENT student (name,age,sex)>
<!ELEMENT name (#PCDATA)>
<!ELEMENT age (#PCDATA)>
<!ELEMENT sex (#PCDATA)>
<!ATTLIST student number ID #REQUIRED>
<!--PCDATA表示字符串-->
<!--REQUIRED必须有的属性-->
<!--ATTLIST属性(attribution)-->
```

引入dtd文档到xml文档中：

- 内部dtd：将约束规则定义在xml文档中
- 外部dtd：将约束的规则定义在外部的dtd文件中
  - 本地：`<!DOCTYPE 根标签名 SYSTEM "dtd文件的位置">`
  - 网络：`<!DOCTYPE 根标签名 PUBLIC "dtd文件名字" "dtd文件的位置URL">`

```xml-dtd
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE students SYSTEM "student.dtd">

<!--<!DOCTYPE students [
		<!ELEMENT students (student+) >
		<!ELEMENT student (name,age,sex)>
		<!ELEMENT name (#PCDATA)>
		<!ELEMENT age (#PCDATA)>
		<!ELEMENT sex (#PCDATA)>
		<!ATTLIST student number ID #REQUIRED>
]>-->
<students>
	<student number="s001">
		<name>zhangsan</name>
		<age>abc</age>
		<sex>hehe</sex>
	</student>
	<student number="s002">
		<name>lisi</name>
		<age>24</age>
		<sex>female</sex>
	</student>
</students>
```



##### Schema:一种复杂的约束技术

```xml
<?xml version="1.0"?>
<xsd:schema xmlns="http://www.itcast.cn/xml"
        xmlns:xsd="http://www.w3.org/2001/XMLSchema"
        targetNamespace="http://www.itcast.cn/xml" elementFormDefault="qualified">
    <xsd:element name="students" type="studentsType"/>
    <xsd:complexType name="studentsType">
        <xsd:sequence>
            <xsd:element name="student" type="studentType" minOccurs="0" maxOccurs="unbounded"/>
        </xsd:sequence>
    </xsd:complexType>
    <xsd:complexType name="studentType">
        <xsd:sequence>
            <xsd:element name="name" type="xsd:string"/>
            <xsd:element name="age" type="ageType" />
            <xsd:element name="sex" type="sexType" />
        </xsd:sequence>
        <xsd:attribute name="number" type="numberType" use="required"/>
    </xsd:complexType>
    <xsd:simpleType name="sexType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="male"/>
            <xsd:enumeration value="female"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="ageType">
        <xsd:restriction base="xsd:integer">
            <xsd:minInclusive value="0"/>
            <xsd:maxInclusive value="256"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="numberType">
        <xsd:restriction base="xsd:string">
            <xsd:pattern value="heima_\d{4}"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema> 
```

引入：

1. 填写xml文档的根元素
2. 引入xsi前缀`xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"`
3. 引入xsd文件命名空间`xsi:schemaLocation="http://www.itcast.cn/xml  student.xsd"`
4. 为每一个xsd约束声明一个前缀,作为标识`xmlns="http://www.itcast.cn/xml"`

示例：

```xml
<students   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://www.itcast.cn/xml"
    xsi:schemaLocation="http://www.itcast.cn/xml  student.xsd">
    <!--ns:namespace命名空间-->
    <!--可以有多个约束文件-->
</students>
```

spring框架的配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <context:annotation-config />
    
    <context:component-scan base-package="cn.cisol.mvcdemo">
        <context:include-filter type="annotation"
            expression="org.springframework.stereotype.Controller" />
    </context:component-scan>
    <mvc:annotation-driven />
    <mvc:resources mapping="/resources/**" location="/resources/" />

    <bean  class="org.springframework.web.servlet.view.ContentNegotiatingViewResolver">
        <property name="order" value="1" />
        <property name="mediaTypes">
            <map>
                <entry key="json" value="application/json" />
                <entry key="xml" value="application/xml" />
                <entry key="htm" value="text/html" />
            </map>
        </property>

        <property name="defaultViews">
            <list>
                
                <bean
                    class="org.springframework.web.servlet.view.json.MappingJackson2JsonView">
                </bean>
            </list>
        </property>
        <property name="ignoreAcceptHeader" value="true" />
    </bean>

    <bean  class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass"
            value="org.springframework.web.servlet.view.JstlView" />
        <property name="prefix" value="/WEB-INF/jsps/" />
        <property name="suffix" value=".jsp" />
    </bean>
  
    <bean id="multipartResolver"
        class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="209715200" />
        <property name="defaultEncoding" value="UTF-8" />
        <property name="resolveLazily" value="true" />
    </bean>

</beans>
```



解析

> 操作xml文档，将文档中的数据读取到内存中

操作xml文档
  		1. 解析(读取)：将文档中的数据读取到内存中
  		2. 写入：将内存中的数据保存到xml文档中。持久化的存储

解析xml的方式：

- DOM：将标记语言文档一次性加载进内存，在内存中形成一颗dom树
  - 优点：操作方便，可以对文档进行CRUD的所有操作
  - 缺点：占内存

- SAX：逐行读取，基于事件驱动的。
  - 优点：不占内存。
  - 缺点：只能读取，不能增删改

xml常见的解析器：

- JAXP：sun公司提供的解析器，支持dom和sax两种思想
- DOM4J：一款非常优秀的解析器，支持dom思想，服务端常用
- Jsoup：jsoup 是一款Java 的HTML解析器，可直接解析某个URL地址、HTML文本内容。它提供了一套非常省力的API，可通过DOM，CSS以及类似于jQuery的操作方法来取出和操作数据。
- PULL：Android操作系统内置的解析器，sax方式的。







