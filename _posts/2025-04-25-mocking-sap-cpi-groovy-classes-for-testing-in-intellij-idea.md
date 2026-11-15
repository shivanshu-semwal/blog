---
title: "Mocking SAP CPI Groovy Classes for Testing in IntelliJ Idea"
layout: post
tags:
  - groovy
  - sap-cpi
description: Create mock classes in IntelliJ to test Groovy code that depends on SAP CPI classes.
---

SAP CPI uses Groovy scripts to implement custom logic. For easier testing, you can write the Groovy code in IntelliJ or Eclipse and mock the SAP classes.

The key requirement is to match the Java and Groovy versions used by SAP CPI.

To do this, you can inspect the runtime versions using a script:

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

Then create a local mock class `Message.groovy` in the source folder with the required methods and fields.

```java
package com.sap.gateway.ip.core.customdev.util

class Message {
    Object msgBody
    Map<String, Object> msgHeaders
    Map<String, Object> msgProps

    Message(String body) { setBody(body) }

    Object getBody() { return this.msgBody ?: null }
    void setBody(msgBody) { this.msgBody = (Object) msgBody }

    Map<String, Object> getHeaders() { return this.msgHeaders }
    Object getHeader(String name){ return this.msgHeaders ? this.msgHeaders.get(name) : null }
    void setHeader(String name, Object value){
        if(!this.msgHeaders) this.msgHeaders = [:]
        this.msgHeaders.put(name, value)
    }

    Map<String, Object> getProperties(){ return this.msgProps }
    Object getProperty(String name){ return this.msgProps ? this.msgProps.get(name) : null }
    void setProperty(String name, Object value){
        if(!this.msgProps) this.msgProps = [:]
        this.msgProps.put(name, value)
    }
}
```

This allows you to unit-test the Groovy logic without the full SAP runtime.
