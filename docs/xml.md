---
title: XML
menu: XML
order: 30
---

## Overview

Due to its essential simplicity and extensibility, XML plays a variety of roles in software development. Common uses of XML documents include---but are not limited to---these:

* Data interchange (this is the case for many web services---some using SOAP or other standard schemas, others using provider-specified schemas). 

* Data transformation rules (XSLT).

* Schemas that can be used to validate the structure and content of other XML documents.

* Domain-specific language code (see the Flowgorithm file format for an example of this).

* Text markup content (e.g. XHTML).

* Project definition files (e.g. Maven `pom.xml` files).

* Build scripts (e.g. Ant files).

* Configuration data (e.g. Java XML properties files, Android package manifests).

* Localizable UI resources (Java XML resource bundles, Android string, color, and drawable resources).

* Style rules (Android style resources).

* UI layouts (Android menu and layout resources, JavaFX layout files).

Given the wide variety of applications, it is difficult to have a comprehensive style guide for XML---especially since at least the element and attribute names in most of the above listed items are not under our control. Thus, the rules and practices below should be interpreted with that understanding.

## Files

1. _To the extent that file naming and casing are within our control_, files containing XML documents **must be** named in `spinal-case`, with an appropriate lowercase extension (`.xml`, `.fxml`, etc.).

    (An example of a use where letter casing is not in our control is Android resource files, which must be named in `lower_snake_case`.)

2. In the source code structure of a Java development project (including simple Java libraries &amp; console-mode applications, Android apps &amp; services, Maven projects, etc.), XML files **must not** physically reside within the same classpath element as Java source files. For example, in an Android app project, Java files are located in the `app/src/main/java` subdirectory, while resources (XML and others) are in `app/src/main/res`. 

    In an IDE such as IntelliJ IDEA, this will typically be implemented by defining one or more _resource folders_ in the project, separate from the _source folders_. The former contains XML files and other non-Java assets, while the latter contains the Java source files.

3. The encoding for an XML file **must be** UTF-8, without a byte order mark (BOM).

## Elements

In most cases in this bootcamp, the naming and casing of elements is not in our control; we're usually consuming or authoring XML documents that have a set of supported elements dictated by the tools we're using, or by the schemas recognized by those tools.

When we do have control over naming and casing of elements---e.g. when developing a web service capable of returning XML responses to the client---the rules below apply.

1. Element names **must be** nouns or noun phrases.

2. Elements which act as containers for repeated child elements **must be** given plural or collective names.

3. Element names _should be_ in `UpperCamelCase`. Among the permissible exceptions are elements which are specify property values of a parent element (e.g. where a structure is needed, rather than a scalar that could be specified as an attribute value): these _may be_ named in `lowerCamelCase`. 

## Attributes

As with elements, we are usually not in control of the attributes available in a given application. When we are in control, follow these rules;

1. Attribute names _should be_ nouns or noun phrases, except for names of Boolean-valued attributes, which _should be_ adjectives or adjective phrases.

2. If an attribute is multi-valued (e.g. a comma-separated list), it **must be** given a plural or collective name.

3. Attributes **must be** named in `lowerCamelCase`.

## Namespaces

1. Namespace prefixes **must be** in `spinal-case`.

2. While the prefix of a namespace is only significant within its scope (the element where the namespace is declared as an attribute with the `xmlns` prefix, and all child nodes of that element), apply the _principle of least astonishment_ (PoLA): if a given namespace has a very commonly used prefix, then _favor_ that prefix in your XML as well---unless that conflicts with the above rule.

    In particular, the well-established namespaces below **must** use the prefixes shown:
	
    * `xmlns:xsd="http://www.w3.org/2001/XMLSchema"` or `xmlns:xs="http://www.w3.org/2001/XMLSchema"`

    * `xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"`

    * `xmlns:xsl="http://www.w3.org/1999/XSL/Transform"`

    * `xmlns:android="http://schemas.android.com/apk/res/android"`

    * `xmlns:app="http://schemas.android.com/apk/res-auto"`  

    * `xmlns:tools="http://schemas.android.com/tools"`

    * `xmlns:fx="http://javafx.com/fxml"`

## Formatting

To the extent that XML is used for project definition, configuration, or source code---POM files, XML properties files, Android resources, JavaFX layout files, etc.---and to the extent that we have control over the file formatting, the following rules apply.

1. _Use a formatting tool!_ For example, when IntelliJ is configured to use the Google Style Guide schema, the **Code/Format Code** command can be used in XML files, enforcing (at least) the rules below.

2. Indentation of elements and attributes **must** use space characters instead of tabs.

3. Every XML element **must** begin on its own line.

4. All child elements of a given parent **must be** indented by the same amount (at least 2 spaces) to the right of the opening tag of the parent element.

5. If an element contains more than one attribute, then every attribute starting with the second **must** start a new line. The first attribute of such an element _should_ start a new line. 

6. All attributes starting a new line must be indented the same amount (at least 2 spaces) to the right of the indent position of the element itself.

7. If an element has zero or one attributes, no child elements, and a text node that does not extend beyond a single line, the start and end tags of the element _should be_ written on a single line. 

8. _Favor_ _empty-element_ (self-closing) tags for elements that have no body content (i.e. no child elements or text nodes).

9. When an element has a closing tag, and that closing tag is not on the same line as the opening tag, the closing tag **must be** indented to the same level as the opening tag.

