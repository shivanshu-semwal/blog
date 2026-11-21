---
title: "Practical XML Manipulation with Groovy: Common Examples"
layout: post
tags:
  - xml
  - groovy
description: A practical guide to parsing and transforming XML using Groovy.
---

## Introduction

To parse xml in groovy there are two options available

* `XmlParser` which returns a `Node` objects when parsing XML
    
* `XmlSlurper` which returns `GPathResult` instances when parsing XML. It evaluates the structure lazily. So if you update the xml you’ll have to evaluate the whole tree again.
    

When to use which one:

* **If you want to transform an existing document to another** then `XmlSlurper` will be the choice
    
* **If you want to update and read at the same time** then `XmlParser` is the choice.
    

Read more here - [https://groovy-lang.org/processing-xml.html](https://groovy-lang.org/processing-xml.html)

## Examples

### Copying a complete XML inside another XML node

* You have a xml and you want a **part** (it may consist nested nodes) **of that xml** inside some other xml structure you created.
    

You can get that part of xml using `XmlParser` as a `GPath` and then use the code given below. The main concept is to use `mkp.yieldUnescaped` where `mkp` is `MarkupBuilderHelper` instance for the `xmlBuilder` object.

Here is link for more details about `MarkupBuilderHelper` - [https://groovy-lang.org/processing-xml.html#\_markupbuilderhelper](https://groovy-lang.org/processing-xml.html#_markupbuilderhelper)

```java
import groovy.xml.MarkupBuilder
import groovy.xml.XmlUtil

xml = '''
<records>
  <car name='HSV Maloo' make='Holden' year='2006'>
    <country>Australia</country>
    <record type='speed'>Production Pickup Truck with speed of 271kph</record>
  </car>
  <car name='P50' make='Peel' year='1962'>
    <country>Isle of Man</country>
    <record type='size'>Smallest Street-Legal Car at 99cm wide and 59 kg in weight</record>
  </car>
  <car name='Royale' make='Bugatti' year='1931'>
    <country>France</country>
    <record type='price'>Most Valuable Car at $15 million</record>
  </car>
</records>
'''

// records is a groovy.util.NodeList
def records = new XmlParser(false, false).parseText(xml).car
def writer = new StringWriter()
def xmlBuilder = new MarkupBuilder(writer)
xmlBuilder.mkp.xmlDeclaration(version: '1.0', encoding: 'UTF-8') // add xml declaration

xmlBuilder.'car-records'('id': '1') {
    // since we are using NodeList we have to iterate
    // in case it is Node you can directly use, mkp.yieldUnescaped()
    records.forEach { child ->
        mkp.yieldUnescaped(XmlUtil.serialize(child).replaceFirst(/<\?xml.*\?>/, ''))
    }
}

println(writer.toString())
```

In case there are you don't want to insert one but child elements of a particular element you can utilize this code snippet:

```java
import groovy.xml.MarkupBuilder
import groovy.xml.XmlUtil

xml = '''
<records>
  <car name='HSV Maloo' make='Holden' year='2006'>
    <country>Australia</country>
    <record type='speed'>Production Pickup Truck with speed of 271kph</record>
  </car>
  <car name='P50' make='Peel' year='1962'>
    <country>Isle of Man</country>
    <record type='size'>Smallest Street-Legal Car at 99cm wide and 59 kg in weight</record>
  </car>
  <car name='Royale' make='Bugatti' year='1931'>
    <country>France</country>
    <record type='price'>Most Valuable Car at $15 million</record>
  </car>
</records>
'''

def records = new XmlSlurper(false, false).parseText(xml).car
def writer = new StringWriter()
def xmlBuilder = new MarkupBuilder(writer)
xmlBuilder.mkp.xmlDeclaration(version: '1.0', encoding: 'UTF-8') // add xml declaration

xmlBuilder.'car-records'('id': '1') {
    records.forEach { child ->
        mkp.yieldUnescaped(XmlUtil.serialize(child).replaceFirst(/<\?xml.*\?>/, ''))
    }
}

println(writer.toString())
```

### Perform some operation (sort, unique, custom, ...) on some items in XML based on some criteria

Suppose you want perform some operation on some items inside XML, like sorting or keeping only unique items, here is a common outline on how you can do that.

* First query for all the items, use `GPath` to access them.
    
* After extracting all the items remove them from the original XML.
    
* Perform manipulation on them, you can use criteria in form of closures. Also if you want to multiple manipulations use chaining of functions.
    
* Add the manipulated items back to the XML.
    
* Use `XmlUtil` to serialize the output XML.
    

```java
import groovy.xml.XmlUtil

xml = """<vehicles>
    <cars>
        <car>
            <id>3</id>
            <name>Toyota Camry</name>
            <color>Silver</color>
        </car>
        <car>
            <id>1</id>
            <name>BMW 3 Series</name>
            <color>Navy Blue</color>
        </car>
        <car>
            <id>2</id>
            <name>Audi A4</name>
            <color>Black</color>
        </car>
        <car>
            <id>5</id>
            <name>Honda Accord</name>
            <color>White</color>
        </car>
        <car>
            <id>4</id>
            <name>Mercedes-Benz C-Class</name>
            <color>Dark Grey</color>
        </car>
    </cars>
</vehicles>
"""

def parser = new XmlParser(false, false)
def vehicles = parser.parseText(xml)

// Extract the items we need segments
def cars = vehicles.cars.car

// remove those items from xml
cars.each { item ->
    def parent = item.parent()
    parent.remove(item)
}

// Perform the operation you want on those items, like sorting in this case
// the criteria will be passed as a closure
def sortedCars = cars.sort { it.id.text() }

// or you can get all unique items
def uniqueCars = cars.unique { it.id.text() }
// or both unique soorted cars
def uniqueSortedCars = cars.unique { it.id.text() }.sort { it.id.text() }

// Add those modified items to the xml
// note that I'm getting the first root element at 0, if there were 
// multiple roots then you have to use multiple each
sortedCars.each { vehicles.cars[0].append(it) }

// Convert the modified XML back to string
def outputXml = XmlUtil.serialize(vehicles)

println(outputXml)
```

### Perform some operation on items with text values

Suppose you want to perform some operation like capitalization, removing leading zeros, changing date format, etc. on items which have text values.

Here is outline on how to do that:

* first query for all the elements for which you want to do the transformation
    
* use some closure or function to perform the transformation
    

```java
def xml = "" // use same as the example above
def parser = new XmlParser()
def vehicles = parser.parseText(xml)

// Find all the elements which you want for transformation
vehicles.'**'.findAll { it.name() == 'name' }.each { element ->
    element.value = element.text().toUpperCase()
}

// Convert the modified XML back to a string
def outputXml = XmlUtil.serialize(vehicles)
println outputXml
```

...more examples to come.