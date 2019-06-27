# MyBatisGeneratorOwn
Mybatis逆向工程工具,使用了[mybatis/generator](https://github.com/mybatis/generator),
包括批量添加,分页,序列化和基础curd.

---

#### 使用maven构建,导入即可使用.  
(由于ojdbc依赖不存在于maven仓库,所以需要手动下载)

#### 使用方式

* 找到`/resources/generatorConfig.xml`

* 修改数据库配置

```xml
<!--MySql数据库连接的信息：驱动类、连接地址、用户名、密码 -->
 <jdbcConnection driverClass="com.mysql.jdbc.Driver"
 	connectionURL="jdbc:mysql://ip:3306/db?useUnicode=true&amp;characterEncoding=UTF-8"
 	userId="root"
 	password="123456">
 	<!-- 针对mysql数据库 -->
 	<property name="useInformationSchema" value="true"></property>
</jdbcConnection>
```

* 设置mapper,xml,pojo属性

```xml
<!-- 默认false，把jdbc decimal 和 numeric 类型解析为 Integer，为 true时把jdbc decimal 和 numeric 类型解析为java.math.BigDecimal -->
<javaTypeResolver>
	<property name="forceBigDecimals" value="true" />
</javaTypeResolver>

<!-- targetProject:生成PO实体类的位置 -->
<javaModelGenerator targetPackage="com.ft.user.entity" targetProject="./src">
	<!-- enableSubPackages:是否让schema作为包的后缀 -->
	<property name="enableSubPackages" value="true" />
	<!-- 从数据库返回的值被清理前后的空格 -->
	<property name="trimStrings" value="true" />
</javaModelGenerator>

<!-- targetProject:mapper.xml映射文件生成的位置 -->
<sqlMapGenerator targetPackage="com.ft.user.mapper" targetProject="./src">
	<!-- enableSubPackages:是否让schema作为包的后缀 -->
	<property name="enableSubPackages" value="false" />
</sqlMapGenerator>

<!-- targetPackage：mapper接口生成的位置 -->
<javaClientGenerator type="XMLMAPPER" targetPackage="com.ft.user.mapper" targetProject="./src">
	<!-- enableSubPackages:是否让schema作为包的后缀 -->
	<property name="enableSubPackages" value="true" />
</javaClientGenerator>
```

* 指定数据库表及需要生成的方法
```xml
<!-- 指定数据库表 -->
<table tableName="表名"
enableDeleteByExample="false"
enableUpdateByPrimaryKey="true"
selectByExampleQueryId="false"
selectByPrimaryKeyQueryId="true"

enableUpdateByExample="false"
enableDeleteByPrimaryKey="false"
enableSelectByExample="false"
enableSelectByPrimaryKey="true" 

enableInsert="true"
enableCountByExample="false"
>
</table>
```
* 配置插件(非必须)
```xml
<!-- 生成limit分页属性	【扩展插件】-->
<plugin type="mybitisPlugin.PaginationPlugin" /> 
<!--生成的实体类实现序列化接口	【扩展插件】-->
<plugin type="mybitisPlugin.SerializablePlugin" />
<!-- 批量添加插件 -->
<plugin type="mybitisPlugin.BatchInsertPlugin" />
```
* 执行generatorSqlmap\GeneratorSqlmap.java中的main方法即可

#### 插件方式

---
* 使用maven插件[itfsw/mybatis-generator-plugin](https://github.com/itfsw/mybatis-generator-plugin)生成代码.   
`idea`  
用户边栏找到maven插件的选项`mybatis-generator`,展开后运行`mybatis-generator:generate`即可.  
`eclipse`  
运行`org.mybatis.generator:mybatis-generator-maven-plugin:1.3.7:generate`命令即可.