数据（Action配置文件）有了，对应的数据存储类（ActionMapping）也有了。毫无疑问，接下来就是数据的解析了。使用`dom4j`解析`xml`文件。

```java
package com.snails.framework.action;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

import org.apache.commons.lang3.StringUtils;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;


/**
 * 读取action的配置文件，并转换成ActionMapping对象
 */
public class ActionMappingManager {
	
	// 参数一保存每个action标签的name属性值，参数二保存转换后的action标签对象
	Map<String, ActionMapping> actionMappings = new HashMap<String, ActionMapping>();
	
	public ActionMappingManager() {	}
	// 读取配置文件并解析
	public ActionMappingManager(String[] configFiles) throws Exception {
		//  校验入参
		if (null == configFiles || configFiles.length <= 0) {
			throw new Exception("请指定action的配置文件！");
		}
		// 遍历action配置文件，并解析初始化到actionMappings中
		for (String cfgFile : configFiles) {
			readCfg(cfgFile);
		}
	}
	
	/**
	 * 根据action类名获取action配置
	 * @param actionName 名称
	 * @return ActionMapping
	 * @throws Exception 
	 */
	public ActionMapping getActionMapping(String actionName) throws Exception {
		if (StringUtils.isBlank(actionName)) {
			throw new Exception("入参actionName不允许为空！");
		}
		ActionMapping am = this.actionMappings.get(actionName);
		if (null == am) {
			throw new Exception("没有Action["+actionName+"]的配置！");
		}
		
		return am;
	}
	
	/**
	 * 读取action配置，并设置到ActionMapping map中
	 * @param cfgFile action配置
	 */
	@SuppressWarnings("rawtypes")
	private void readCfg(String cfgFile) {

		try {
			//  解析action配置
//			InputStream is = this.getClass().getResourceAsStream(cfgFile);
			InputStream is = Thread.currentThread().getContextClassLoader().getResourceAsStream(cfgFile);
			 // 创建saxReader对象  
	        SAXReader reader = new SAXReader();  
	        // 通过read方法读取一个文件 转换成Document对象  
	        Document doc = reader.read(is);   //new File("src/dom4j/sida.xml")
	        //获取根节点元素对象  
			Element actions = doc.getRootElement();
			//  遍历每个action标签
			Iterator it = actions.elementIterator();
			while (it.hasNext()) {
				//  action转换为ActionMapping
				ActionMapping am = new ActionMapping();
				Element action = (Element) it.next();
				// 获取action标签的属性name和class
				String actionName = action.attributeValue("name");
				String actionClassName = action.attributeValue("class");
				
				am.setActionName(actionName);
				am.setActionClassName(actionClassName);
				
				// 遍历action下的result标签
				Iterator results = action.elementIterator();
				while (results.hasNext()) {
					Element result = (Element) results.next();
					// 获取result标签的属性name和内容
					String resultName = result.attributeValue("name");
					String redirect = result.attributeValue("redirect");
					String resultContent = result.getTextTrim();
					if (StringUtils.isBlank(resultContent))
						resultContent = "input";
					
					//  保存result标签值
					Map<String, String> rs = new HashMap<String, String>();
					rs.put("RESULT_CONTENT", resultContent);
					rs.put("REDIRECT", redirect);
					am.setResultMap(resultName, rs);
				}
				
				actionMappings.put(am.getActionName(), am);
			}

		} catch (DocumentException e) {
			e.printStackTrace();
		}
		
	}
	
	public static void main(String[] args) {
		    
		String cfgFile = "E:\\html2pdf_codes\\snails\\src\\com\\snails\\framework\\config\\snails-actions.xml";
		new ActionMappingManager().readCfg(cfgFile);
	}
}
```

方法中的参数异常直接抛到上一层处理。代码也没什么难度，注意解析过程获取节点数据的关键字与xml文件对应即可。
如果你对xml文件的解析不是很明白请参考[xml解析教程](https://www.baidu.com/s?wd=dom4j%E8%A7%A3%E6%9E%90xml&rsv_spt=1&rsv_iqid=0xd4cdb0e5000a615d&issp=1&f=3&rsv_bp=1&rsv_idx=2&ie=utf-8&tn=baiduhome_pg&rsv_enter=0&oq=dom4j%E8%A7%A3%E6%9E%90xml&rsv_t=44e2UcI2nb1JG2MNxwVoCiJAnp7EY8mCUw8hbK5d9n4q6EYLtB5t%2FooTycDqlaIQW4pq&rsv_sug3=8&rsv_sug1=6&rsv_sug7=100&rsv_pq=8bd2fa580008e6b8&rsv_sug4=458)。其他不多说，请继续看后面的文章。

项目完整代码请看[MyMVC](https://github.com/ubuntuvim/myMVC)，欢迎fork学习，如果你觉得对你有帮助给我点个赞吧，当然也欢迎给我提意见（email:1527254027@qq.com，chendequanroob@gmail.com）。
