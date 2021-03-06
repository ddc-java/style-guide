---
title: JSON
menu: JSON
order: 40
---

{% include ddc-abbreviations.md %}
      
## Overview

In Java development, the applications of _JavaScript Object Notation_ (JSON) are similar to---though slightly more narrow in scope than---those of XML. It is most often used for data interchange (e.g. sending and receiving data to and from web services), but it can also be used for configuration files (e.g. as an alternative to YAML for an OpenAPI/Swagger service definition) and other purposes.

Structurally, it can be useful to think of JSON as being analogous to XML---but without standard related facilities such as DTDs, schemas, XSL transforms, and XPath; with an anonymous root element; and combining the concepts of attributes and child elements into a single _property_ concept. (This may sound very limiting; it actually isn't as limiting as it might appear, and the benefit---a reduction in the verbosity that is a common pain point for XML users---often justifies these constraints.)

As is the case with XML, we have no control over the naming and letter casing of properties in JSON values we receive from an external service or data source. Also, the actual JSON values we send and receive are often generated and parsed by serialization libraries such as Gson or Jackson; thus, we typically have (or need) little control over the formatting of these values.

## Files

1. _To the extent that file naming and casing are within our control_, JSON files **must be** named in `spinal-case`, with an appropriate lowercase extension (usually `.json`).

2. If a JSON file is part of the configuration or source code of a Java development project, this file **must not** physically reside within the same classpath element as Java source files; instead, it should be located (directly or indirectly) in a resource folder.

    In an IDE such as IntelliJ IDEA, this will typically be implemented by defining one or more _resource folders_ in the project, separate from the _source folders_. The former contains non-Java assets, while the latter contains the Java source files.

3. The encoding for a JSON file **must be** UTF-8, without a byte order mark (BOM).

## Properties

In this bootcamp, the naming and casing of JSON properties is rarely in our control; we're usually consuming or authoring JSON objects that have a set of properties dictated by the services or tools we're using.

When we do have control over naming and casing of properties---e.g. when developing a web service capable of returning JSON responses to the client---the rules below apply.

1. Property names **must be** nouns or noun phrases.

2. Array-valued properties **must be** given plural or collective names.

3. Property names **must be** in `lowerCamelCase`.

## Formatting

Most of the time, JSON formatting is not that important to us, since we're not authoring JSON files and using them as configuration or other source files in our projects; instead, we're using a library to send and receive them, and they're not seen by human eyes. 

There are exceptions to the above, of course. For example, we might incorporate a data dump from an external data source, packaged as JSON, into the startup/post-install step for our web service or Android app. In that case, the file may indeed need to be human-readable. In cases such as those, the following rules apply. 

1. _Use a formatting tool!_ When IntelliJ is configured to use the Google Style Guide scheme, the **Code/Format Code** command can be used in JSON files, enforcing (at least) the rules below. The tools in the JSON section of [**Formatting tools**](resources.md#formatting-tools) can also be configured to produce formatted JSON that conforms to the rules here.

2. Indentation of object properties and array elements **must** use space characters instead of tabs.

3. Every property of an object **must** start a new line.

4. All properties of an object **must** be indented by the same amount (at least 2 spaces) to the right of the indentation of the line where the object's opening brace appears.

5. Every element of an array **must** start a new line.

6. All elements of an array **must** be indented by the same amount (at least 2 spaces) to the right of the indentation of the line where the array's opening bracket appears.

7. If a property is scalar-valued (i.e. the value is string, number, Boolean, or `null`), the value **must** follow the property name on the same line.

8. If a property is array- or object-valued, the opening token (bracket or brace, respectively) of the array or object **must** follow the property name on the same line.

9. The closing brace or bracket of a JSON object or array (respectively) **must** start a new line.

10. The closing brace or bracket of a JSON object or array (respectively) **must** be indented to the same level as the line containing the opening brace or bracket.  

11. The colon that separates a property name from its value **must have** a trailing space, but _should not_ have a leading space. That is, the colon _should_ follow immediately after the quoted property name, but it **must** itself be followed by a _single_ space, and then the starting token of the property value.

12. The comma following any object property or array element that is _not_ the last property listed in the object, or the last element in the array, must appear immediately after the preceding property or element, with no intervening spaces. (Note that if the previous property or element is object- or array-valued, the comma will follow immediately after the closing brace or bracket of the previous property or element.)
