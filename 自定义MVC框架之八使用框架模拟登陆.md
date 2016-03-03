到此框架已经开发完成，此时你可以启动项目，验证项目是否有错误，特别是配置的文件的路径问题。
如果项目成功启动并且没有报任何错误，说明框架开发是没问题的，下面就是验证框架是否达到要求。我们使用登录这个非常常见的例子验证。

1. 新建三个JSP文件，这三个文件的作用请看[自定义MVC框架之一框架总说明](https://github.com/ubuntuvim/study-note/blob/master/%E8%87%AA%E5%AE%9A%E4%B9%89MVC%E6%A1%86%E6%9E%B6%E4%B9%8B%E4%B8%80%E6%A1%86%E6%9E%B6%E6%80%BB%E8%AF%B4%E6%98%8E.md)中的说明
2. 在配置文件`snails-actions.xml`增加`LoginAction`类配置
3. 编写`LoginAction`，简单判断用户名和密码都是等于`admin`时跳转到成功页面`success.jsp`，否则跳转到失败页面`fail.jsp`。

```java
package com.snails.action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.snails.framework.action.Action;

public class LoginAction implements Action {

	@Override
	public String execute(HttpServletRequest request,
			HttpServletResponse response) throws Exception {

		String username = request.getParameter("username");
		String password = request.getParameter("password");
		//  简单判断，如果用户名和密码都是admin则登录成
		if (username.equals(password)) {
			request.setAttribute("username", username);
			return SUCCESS;
		} else {
			return "fail";
		}
	}

}
```

**注意：** 要使用框架就必须实现`Action`类，并且实现方法`execute`，在此方法中调用M层处理，最后返回与配置对应的视图名。


#### JSP页面

这三个JSP页面就不再贴出来了，请直接到[github项目myMVC](https://github.com/ubuntuvim/myMVC/tree/master/WebContent)直接获取吧。

#### 验证

启动项目，输入[http://localhost:8080/snails/](http://localhost:8080/snails/)进入到登录页面。
用户名和密码都是输入admin，点击提交，可以看到了转到`success.jsp`。

**登录成功**

![用户名和密码输入admin](http://7xnrhh.com1.z0.glb.clouddn.com/QQ%E6%88%AA%E5%9B%BE20160303175614.png)

![登录成功](http://7xnrhh.com1.z0.glb.clouddn.com/QQ%E6%88%AA%E5%9B%BE20160303175655.png)

**登录失败**

![用户名和密码输入的不是admin](http://7xnrhh.com1.z0.glb.clouddn.com/QQ%E6%88%AA%E5%9B%BE20160303175715.png)

可以看到地址栏为：`http://localhost:8080/snails/fail.jsp`因为此跳转设置为重定向（`<result name="fail" redirect="true">fail.jsp</result>`）。
