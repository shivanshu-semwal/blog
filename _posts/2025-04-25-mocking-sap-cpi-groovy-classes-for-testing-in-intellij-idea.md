---
title: "Mocking SAP CPI Groovy Classes for Testing in IntelliJ Idea"
layout: post
tags:
  - groovy
  - sap-cpi
description: Create mock classes in IntelliJ to test Groovy code that depends on SAP CPI classes.
image:
  path: /assets/img/covers/mocking-sap-cpi-groovy-classes-for-testing-in-intellij-idea-cover.jpeg
  alt: "Mocking SAP CPI Groovy Classes for Testing in IntelliJ Idea"
---


SAP CPI uses groovy scripts to help with developing custom logic or some complex data manipulation task. For this it provide a GUI to write groovy script and some normal static code error. That may be enough to develop small code it does not provide sufficient help for developing complex groovy scripts.

For this purpose we can use IDE for groovy scripting like `IntelliJ Idea` or `Ecllipse`. These IDE provide with the capabilities to debug code, syntax highlighting, showing static errors, auto completion and many more.

To develop SAP CPI groovy script in IDE we need the libraries used there like `Message` class ([`com.sap.gateway.ip.core.customdev.util.Message`](https://help.sap.com/doc/a56f52e1a58e4e2bac7f7adbf45b2e26/Cloud/en-US/com/sap/gateway/ip/core/customdev/util/Message.html)), or `ValueMappingApi` class ([`com.sap.it.api.mapping.ValueMappingApi`](https://help.sap.com/doc/d47441d304c14a0ab9d3986c1b553a1e/Cloud/en-US/com/sap/it/api/mapping/ValueMappingApi.html)) and many more. But most commonly used one is `Message`, which is the one I'm going to implement in this blog.

Also for this you need to check the Java and Groovy version present in SAP CPI and match it with you IDE Java and Groovy version. For this you can use this code

```java
import com.sap.gateway.ip.core.customdev.util.Message
import java.util.HashMap

def Message processData(Message message) {
    def javaVersion = System.getProperty("java.version")
    def javaVendor = System.getProperty("java.vendor")
    def javaRuntime = System.getProperty("java.runtime.name")
    def groovyVersion = GroovySystem.version

    def body = """
    Java Version   : ${javaVersion}
    Java Vendor    : ${javaVendor}
    Java Runtime   : ${javaRuntime}
    Groovy Version : ${groovyVersion}
    """.stripIndent()

    message.setBody(body)
    return message
}
```

After this if you create a file `com.sap.gateway.ip.core.customdev.util.Message.groovy` inside `src` folder of your groovy script. 

And place this code inside that file, this will help us to mock the `Message` class.

```java
package com.sap.gateway.ip.core.customdev.util

class Message{
	Object msgBody
	Map<String, Object> msgHeaders
	Map<String, Object> msgProp
	
	Message(String body) { setBody(body) }
	
	/* methods related to body of message*/
	public <Klass> Klass getBody(Class<Klass> klass) {
		def body = klass.cast(this.getBody())
		return body ?: null
	}
	Object getBody() { return this.msgBody ?: null }
	void setBody(msgBody) { this.msgBody = (Object) msgBody }
	
	/* methods related to headers of the message*/
	Map<String, Object> getHeaders() { return this.msgHeaders }
	Object getHeader(String name){ return this.msgHeaders ? this.msgHeaders.get(name) : null }
	public <Klass> Klass getHeader(String name, Class<Klass> klass){
		if(!this.getHeader(name)) return null
		else return this.getHeader(name, klass) ?: null
	}
	public setHeaders(Map<String, Object> msgHeaders) { this.msgHeaders = msgHeaders }
	void setHeader(String name, Object value){
		if(!this.msgHeaders) this.msgHeaders = [:]
		this.msgHeaders.put(name, value)
	}
	
	/*methods related to properties of message*/
	Map<String, Object> getProperties(){ return this.msgProps }
	Object getProperty(String name){ return this.msgProps ? this.msgPops.get(name) : null }
	public <Klass> Klass getProperty(String name, Class<Klass> klass){
		return (!this.getPoperty(name)) ? null : (this.getProperty(name, klass) ?: null)
	}
	void setProperties(Map<String, Object> msgProps){ this.msgProps = msgProps }
	void setProperty(String name, Object value){
		it(!this.msgPops) this.msgProps = [:]
		this.msgProps.put(name, value)
	}

}
```

After implementing this you can use message class in your groovy script and start debugging.
Similarly, if needed we can implement other classes too.
