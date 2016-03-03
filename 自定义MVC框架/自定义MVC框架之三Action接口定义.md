前一篇[自定义MVC框架之一action配置文件定义](https://github.com/ubuntuvim/study-note/blob/master/%E8%87%AA%E5%AE%9A%E4%B9%89MVC%E6%A1%86%E6%9E%B6%E4%B9%8B%E4%B8%80action%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E5%AE%9A%E4%B9%89.md)定义了Action的配置格式。本篇介绍`Action`接口的定义，由于是用于学习只是简单定义了几个公共变量和一个业务处理的方法。

```java
package com.snails.framework.action;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public interface Action {

	public String INPUT = "input";
	public String ERROR = "error";
	public String SUCCESS = "success";

	/**
	 * 处理业务逻辑方法，每个Action类都需要重写此方法，并返回与配置文件对应的页面配置字符串
	 * 比如返回 'input'：
	 * <result name="input">index.jsp</result>
	 *
	 * @param request
	 * @param response
	 * @return
	 */
	public String execute(HttpServletRequest request, HttpServletResponse response) throws Exception;

}
```

代码很简单，不多说。其他使用框架的所有`Action`类都要实现此类。

项目完整代码请看[MyMVC](https://github.com/ubuntuvim/myMVC)，欢迎fork学习，如果你觉得对你有帮助给我点个赞吧，当然也欢迎给我提意见（email:1527254027@qq.com，chendequanroob@gmail.com）。
