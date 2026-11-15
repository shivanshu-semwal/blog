---
title: "Practical XML Manipulation with Groovy: Common Examples"
layout: post
tags:
  - xml
  - groovy
description: A practical guide to parsing and transforming XML using Groovy.
---

## Introduction

To parse XML in Groovy there are two options available:

* `XmlParser`, which returns `Node` objects
* `XmlSlurper`, which returns `GPathResult` and is lazy

When to use which one:

* If you want to transform an existing document to another, `XmlSlurper` is a good choice.
* If you want to update and read at the same time, `XmlParser` is the choice.

Read more here: https://groovy-lang.org/processing-xml.html

## Examples

### Copying a complete XML inside another XML node

You can use `XmlParser` and `MarkupBuilder` to copy a section of XML into a new XML structure:

```groovy
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
</records>
'''

def records = new XmlParser(false, false).parseText(xml).car
def writer = new StringWriter()
def xmlBuilder = new MarkupBuilder(writer)
xmlBuilder.mkp.xmlDeclaration(version: '1.0', encoding: 'UTF-8')

xmlBuilder.'car-records'('id': '1') {
    records.forEach { child ->
        mkp.yieldUnescaped(XmlUtil.serialize(child).replaceFirst(/<\?xml.*\?>/, ''))
    }
}

println(writer.toString())
```

### Sorting and filtering XML content

```groovy
import groovy.xml.XmlUtil

xml = '''<vehicles>
    <cars>
        <car><id>3</id><name>Toyota Camry</name></car>
        <car><id>1</id><name>BMW 3 Series</name></car>
        <car><id>2</id><name>Audi A4</name></car>
    </cars>
</vehicles>
'''

def parser = new XmlParser(false, false)
def vehicles = parser.parseText(xml)
def cars = vehicles.cars.car

cars.each { item ->
    def parent = item.parent()
    parent.remove(item)
}

def sortedCars = cars.sort { it.id.text() }
sortedCars.each { vehicles.cars[0].append(it) }

println(XmlUtil.serialize(vehicles))
```

### Transform text values inside XML

```groovy
def xml = ''
def parser = new XmlParser()
def vehicles = parser.parseText(xml)

vehicles.'**'.findAll { it.name() == 'name' }.each { element ->
    element.value = element.text().toUpperCase()
}

println(XmlUtil.serialize(vehicles))
```

More examples can be added similarly when needed.
