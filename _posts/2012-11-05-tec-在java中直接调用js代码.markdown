---
layout: post
title: "Java中调用js代码"
tag: "tec"
comment: true
published: true
date: 2012-11-05
---


JDK1.6版添加了新的ScriptEngine类，允许用户直接执行js代码。

####在Java中直接调用js代码
不能调用浏览器中定义的js函数，会抛出异常提示ReferenceError: “alert” is not defined。


    package com.sinaapp.manjushri;
 
    import javax.script.ScriptEngine;
    import javax.script.ScriptEngineManager;
    import javax.script.ScriptException;
 
    /**
     * 直接调用js代码
     */
    public class ScriptEngineTest {
 
	public static void main(String[] args) {
		ScriptEngineManager manager = new ScriptEngineManager();
		ScriptEngine engine = manager.getEngineByName("javascript");
		try {
			engine.eval("var a=3; var b=4;print (a+b);");
			// 不能调用浏览器中定义的js函数
			// engine.eval("alert(\"js alert\");");   // 错误，会抛出alert引用不存在的异常
		} catch (ScriptException e) {
			e.printStackTrace();
		}
	} 
    }


输出结果：7

####在Java中绑定js变量
在调用engine.get(key);时，如果key没有定义，则返回null


    package com.sinaapp.manjushri;
 
    import javax.script.Bindings;
    import javax.script.ScriptContext;
    import javax.script.ScriptEngine;
    import javax.script.ScriptEngineManager;    
    import javax.script.ScriptException;
 
    public class ScriptEngineTest2 {
	public static void main(String[] args) {
		ScriptEngineManager manager = new ScriptEngineManager();
		ScriptEngine engine = manager.getEngineByName("javascript");
		engine.put("a", 4);
		engine.put("b", 3);
		Bindings bindings = engine.getBindings(ScriptContext.ENGINE_SCOPE);
		try {
						// 只能为Double，使用Float和Integer会抛出异常
			Double result = (Double) engine.eval("a+b");
			System.out.println("result = " + result);
			engine.eval("c=a+b");
			Double c = (Double)engine.get("c");
			System.out.println("c = " + c);
		} catch (ScriptException e) {
			e.printStackTrace();
		}
	}
    }

输出：
result = 7.0
c = 7.0

####在Java中调用js文件中的function，传入调用参数，并获取返回值
js文件中的merge函数将两个参数a，b相加，并返回c。


    // expression.js
    function merge(a, b) {
	c = a * b;
	return c;
    }


在Java代码中读取js文件，并参数两个参数，然后回去返回值。


    package com.sinaapp.manjushri;
 
    import java.io.FileReader;
 
    import javax.script.Invocable;
    import javax.script.ScriptEngine;
    import javax.script.ScriptEngineManager;
 
    /**
     * Java调用并执行js文件，传递参数，并活动返回值
     *
     * @author manjushri
     */
    public class ScriptEngineTest {
 
	public static void main(String[] args) throws Exception {
		ScriptEngineManager manager = new ScriptEngineManager();
		ScriptEngine engine = manager.getEngineByName("javascript");
 
		String jsFileName = "expression.js";
		// 读取js文件
		FileReader reader = new FileReader(jsFileName);
		// 执行指定脚本
		engine.eval(reader);
		if(engine instanceof Invocable) {
			Invocable invoke = (Invocable)engine;
			// 调用merge方法，并传入两个参数
			// c = merge(2, 3);
			Double c = (Double)invoke.invokeFunction("merge", 2, 3);
			System.out.println("c = " + c);
		}
		reader.close();
	}
    }


摘自：[Manjushri的知识库](http://manjushri.sinaapp.com/?p=50#more-50)