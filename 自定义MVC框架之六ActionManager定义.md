本类主要用于根据类名反射得到类实例。类的名字根据解析得到的Action配置得到，得到Action配置中的`class`属性值后就可以使用反射获取类实例了。

```java
package com.snails.framework.action;

/**
 * 根据action配置的class属性值反射得到对应的类
 */
public class ActionManager {
	
	@SuppressWarnings("rawtypes")
	public static Action createAction(String className) {
		
		try {
			Class clazz = Thread.currentThread().getContextClassLoader().loadClass(className);
			//  判断当前线程是否运行了改action
			if (null == clazz) {
				clazz = Class.forName(className);
			}
			
			return (Action) clazz.newInstance();
			
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (InstantiationException e) {
			e.printStackTrace();
		} catch (IllegalAccessException e) {
			e.printStackTrace();
		}
		
		return null;
	}
}
```

项目完整代码请看[MyMVC](https://github.com/ubuntuvim/myMVC)，欢迎fork学习，如果你觉得对你有帮助给我点个赞吧，当然也欢迎给我提意见（email:1527254027@qq.com，chendequanroob@gmail.com）。
