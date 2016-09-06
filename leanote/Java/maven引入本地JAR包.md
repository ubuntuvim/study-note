修改`pom.xml`文件，加入如下配置：
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>2.5.1</version>
	<configuration>
    	<source>1.6</source>
    	<target>1.6</target>
    	<encoding>UTF-8</encoding>
    	<compilerArguments>
            <extdirs>${project.basedir}/svnkit1.8</extdirs>
    	</compilerArguments>
	</configuration>
</plugin>
```
**注意**：代码`<extdirs>${project.basedir}/svnkit1.8</extdirs>`指定了外部jar包所在目录。
本例子中文件夹`svnkit1.8`放在项目的跟目录下。


项目
  |__svnkit1.8
  |__src