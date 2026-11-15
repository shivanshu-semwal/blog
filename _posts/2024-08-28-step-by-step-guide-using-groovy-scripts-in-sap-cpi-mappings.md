---
title: "Step-by-Step Guide: Using Groovy Scripts in SAP CPI Mappings"
layout: post
tags:
  - groovy
  - sap
  - sap-cpi
description: An overview of Groovy scripting in SAP CPI mappings and how to structure the scripts.
---

In SAP CPI, when you use mappings to transform XML from one structure to another, you can perform operations on the elements like splitting, concatenation, and transformation.

When the standard functions are insufficient, Groovy scripts can be used.

## Types of SAP CPI message mapping Groovy scripts

There are two major ways to write Groovy scripts in SAP CPI:

### Process single value at a time

```java
import com.sap.it.api.mapping.*

def String exampleSingleValue(String field) {
    // Your code here
    return result;
}
```

And with multiple fields:

```java
import com.sap.it.api.mapping.*

def String exampleSingleValue(String field1, String field2) {
    // Your code here
    return result;
}
```

### Process all values in context at a time

```java
import com.sap.it.api.mapping.*

def void exampleAllValuesInContext(
    String[] fieldArray,
    Output output,
    MappingContext context
){
    output.addValue(result1);
    output.addSuppress();
    output.addValue(result2);
}
```

`MappingContext` can be used to get or set headers for the mapping execution.

## Confusions and misconceptions

* There is no “all values in queue” mode in SAP CPI.

## Other links

* SAP help for standard functions
* Groovy style guide
