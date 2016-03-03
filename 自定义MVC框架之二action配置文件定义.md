为了减少Java代码中的校验我们先定义`Action配置`的文件的校验文件（`snails-actions-validate.xsd`），文件路径请看[框架总说明](https://github.com/ubuntuvim/study-note/blob/master/%E8%87%AA%E5%AE%9A%E4%B9%89MVC%E6%A1%86%E6%9E%B6%E4%B9%8B%E4%B8%80%E6%A1%86%E6%9E%B6%E6%80%BB%E8%AF%B4%E6%98%8E.md)中的图片所示。

#### snails-actions-validate.xsd

下面的`schema`文件既是格式校验文件。

```xml
<!--?xml version="1.0" encoding="UTF-8"?-->
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
	targetNamespace="http://www.w3schools.com"
	xmlns="http://www.w3schools.com"
	elementFormDefault="qualified">
	<!-- elementFormDefault="qualified" 指出任何 XML 实例文档所使用的且在此 schema
	中声明过的元素必须被命名空间限定。
	-->

	<xs:element name="actions">
	    <xs:complexType>
  			<!-- maxOccurs="unbounded"定义元素可以出现一次或者多次 -->
	        <xs:sequence maxOccurs="unbounded">
			    <xs:element name="action">
			  		<xs:complexType>
	       			   <xs:sequence maxOccurs="unbounded">
	       			   		<!-- action的子元素result -->
  			  				<xs:element name="result">
  			  					<xs:complexType>
  			  						<!-- result节点，节点包括字符串形式的内容 -->
  			  						<xs:simpleContent>
  								        <xs:extension base="xs:string">
  								      		<xs:attribute name="name" type="xs:string" use="required" />
    								      		<!-- 定义属性redirect只能有2个值， -->
    								      		<xs:attribute name="redirect" use="optional" default="false">
      								      			<xs:simpleType>
      					                  <xs:restriction base="xs:string">
      													    <xs:enumeration value="true" />
      													    <xs:enumeration value="false" />
      													</xs:restriction>
      												</xs:simpleType>
  								      		</xs:attribute>
  								        </xs:extension>
  								    </xs:simpleContent>
  			  					</xs:complexType>
  			  				</xs:element>
  			  			</xs:sequence>

			  			<!-- action 节点的属性 -->
			  			<xs:attribute name="name" type="xs:string" use="required" />
			  			<xs:attribute name="class" type="xs:string" use="required" />
			  		</xs:complexType>
			    </xs:element>
	        </xs:sequence>
	    </xs:complexType>
	</xs:element>

</xs:schema>
```

标签的解释请看注释信息或者看下面的参考网址：

1. [http://www.w3chtml.com/schema/xml-schema-root.html](http://www.w3chtml.com/schema/xml-schema-root.html)
2. [http://www.cnblogs.com/caoxch/archive/2006/11/17/563856.html](http://www.cnblogs.com/caoxch/archive/2006/11/17/563856.html)
3. [https://www.ibm.com/developerworks/cn/xml/x-cert/part6/](https://www.ibm.com/developerworks/cn/xml/x-cert/part6/)

#### snails-actions.xml

得到校验文件后，开始编写`Action配置文件`，我们可以先写一个用户登录Action的配置。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<actions xmlns="http://www.w3schools.com"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.w3schools.com snails-actions-validate.xsd">

		<action name="login" class="com.snails.action.LoginAction">
			<result name="success">success.jsp</result>
			<result name="fail" redirect="true">fail.jsp</result>
		</action>
</actions>
```

注意头部的`xsi:schemaLocation="http://www.w3schools.com snails-actions-validate.xsd`，这里引入了校验文件，此时两个xml都是在同一个目录下。

到此两个主要的配置文件已经定义完成，下面就是解析xml文件了。

项目完整代码请看[MyMVC](https://github.com/ubuntuvim/myMVC)，欢迎fork学习，如果你觉得对你有帮助给我点个赞吧，当然也欢迎给我提意见（email:1527254027@qq.com，chendequanroob@gmail.com）。
