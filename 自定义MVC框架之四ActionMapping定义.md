有了Action配置之后，我们需要定义一个与配置匹配的javabean。主要是方便数据保存。

```java
package com.snails.framework.action;

import java.util.HashMap;
import java.util.Map;

/**
 * 每个ActionMapping对象对应着一个action配置。
 *
 * <action name="login" class="com.snails.action.LoginAction">
 * 		<result name="input">index.jsp</result>
 * 		<result name="success">success.jsp</result>
 * 	</action>
 */
public class ActionMapping {

	private String actionName;
	private String actionClassName;
	//  result数组，参数一保存result的name属性值，参数二保存result标签内的内容
//	private Map<String, String> resultMap = new HashMap<String, String>();

	//  result数组，参数一保存result的name属性值，参数二也是一个map，保存result标签内的内容与属性redirect的值
	private Map<String, Map<String, String>> resultMap = new HashMap<String, Map<String, String>>();

	/**
	 * 获取标签 <result name="resultName" redirect="true">xxx.jsp</result> 的内容 xxx.jsp
	 * @param result 标签name属性值
	 * @return result标签的内容
	 */
	public Map<String, String> getResult(String resultName) {
		return resultMap.get(resultName);
	}

	public void setResultMap(String resultNameAttr, Map<String, String> result) {
		this.resultMap.put(resultNameAttr, result);
	}

	public String getActionName() {
		return actionName;
	}
	public void setActionName(String actionName) {
		this.actionName = actionName;
	}
	public String getActionClassName() {
		return actionClassName;
	}
	public void setActionClassName(String actionClassName) {
		this.actionClassName = actionClassName;
	}

}
```
