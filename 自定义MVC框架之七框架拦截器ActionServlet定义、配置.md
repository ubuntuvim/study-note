到本篇为止，全部的准备工作都已经做好了，万事俱备只欠东风。本类的作用就类似于`struts`框架的入口文件，我们需要在`web.xml`配置，指定那些请求会进入到框架中，框架会根据请求的内容截取到`Action`名字，然后根据`Action`名字获取到`Action`的配置信息，然后根据`class`属性值反射得到类实例，然后执行`Action`的`execute`方法得到返回的试图名。最后根据视图名对应的视图跳转。

#### 拦截器定义

```java
package com.snails.framework.servlet;

import java.io.IOException;
import java.util.Map;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.commons.lang3.StringUtils;

import com.snails.framework.action.Action;
import com.snails.framework.action.ActionManager;
import com.snails.framework.action.ActionMapping;
import com.snails.framework.action.ActionMappingManager;

/**
 * action的总控制器，所有以".action"结尾的请求都会被这个servlet拦截，并进入到框架中处理
 */
public class ActionServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	private ActionMappingManager amm = null;

    public ActionServlet() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		this.doPost(request, response);
	}

	/**
	 * 截取请求，如果请求是以".action"结尾则进入到框架中处理
	 * 否则不处理
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		try {
			// amm 在init方法中初始化
			ActionMapping am = amm.getActionMapping(getActionName(request));
			//  获取action实例
			Action action = ActionManager.createAction(am.getActionClassName());
			//  执行实现类的业务逻辑
			String result = action.execute(request, response);
			if (StringUtils.isBlank(result)) {
				response.sendError(404, "未配置Action对应的input元素。");
				return;
			}

			Map<String, String> resultMap = am.getResult(result);
      // map的key暂时写死了
			if (Boolean.parseBoolean(resultMap.get("REDIRECT"))) {
				response.sendRedirect(resultMap.get("RESULT_CONTENT"));
			} else {
				request.getRequestDispatcher(resultMap.get("RESULT_CONTENT")).forward(request, response);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	/**
	 * 解析URL获取action的名称
	 * @param request 请求
	 * @return
	 */
	private String getActionName(HttpServletRequest request) {
		String contextPath = request.getContextPath();
		String actionPath = request.getRequestURI().substring(contextPath.length());

		return actionPath.substring(1, actionPath.lastIndexOf(".")).trim();
	}


	@Override
	public void init(ServletConfig config) throws ServletException {
		//  读取框架拦截器的配置信息
		String configFiles = config.getInitParameter("configFiles");
		if (StringUtils.isBlank(configFiles)) {
			try {
				throw new Exception("没有在拦截器中配置任何action配置文件！");
			} catch (Exception e) {
				e.printStackTrace();
			}
		}

		try {
			//  先去除分割配置文件间的空格（如果有）
      //  如果多个配置文件使用逗号','分割，用于学习的就不做的那么灵活了。
			this.amm = new ActionMappingManager(configFiles.replaceAll("\\s*", "").split(","));
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

简单讲这就是一个拦截器，然后根据请求的`URL`处理。在方法`init`中读取初始化Action的配置信息，由于在`web.xml`中设定了此拦截器在项目的启动的时候就初始化了，所以在`doGet`、`doPost`中就随便使用了！


#### 拦截器配置

定义好拦截器之后还没起作用，需要在`web.xml`中配置。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	id="WebApp_ID" version="2.5">

	<display-name>snails</display-name>

	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.htm</welcome-file>
		<welcome-file>default.jsp</welcome-file>
	</welcome-file-list>

	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>com.snails.framework.filter.CharactorFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
			<param-name>ignore</param-name>
			<!-- true过滤，false不过滤 -->
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

  <!--  框架拦截器配置 -->
	<servlet>
		<description>框架拦截器配置</description>
		<display-name>ActionServlet</display-name>
		<servlet-name>ActionServlet</servlet-name>
		<servlet-class>com.snails.framework.servlet.ActionServlet</servlet-class>
		<init-param>
			<param-name>configFiles</param-name>
			<param-value>config/snails-actions.xml</param-value>
		</init-param>
		<load-on-startup>0</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>ActionServlet</servlet-name>
		<url-pattern>*.action</url-pattern>
	</servlet-mapping>
</web-app>
```

上述是项目的全部`web.xml`配置，但是只要注意最后一段即可，其他的配置暂时不管。

* `<param-value>config/snails-actions.xml</param-value>`指定了Action配置文件
* `<load-on-startup>0</load-on-startup>`设定当项目启动时就实例化这个拦截器
* `<url-pattern>*.action</url-pattern>`定义了框架拦截器的请求后缀，只有以".action"为后缀的请求才会进入到自定义的框架中。


项目完整代码请看[MyMVC](https://github.com/ubuntuvim/myMVC)，欢迎fork学习，如果你觉得对你有帮助给我点个赞吧，当然也欢迎给我提意见（email:1527254027@qq.com，chendequanroob@gmail.com）。
