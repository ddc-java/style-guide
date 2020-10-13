---
title: Java
menu: Java
order: 10
---

## Overview

This portion of the style guide should be treated as a supplement to the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html), (GJSG).

The GJSG is not reproduced here; instead, only amendments---additional constraints and relaxations or other changes to stated constraints---are listed below. Thus, a Java source file conforms to this style guide if (and only if) it conforms to the GJSG **and** the amendments stated below---except where the amendments contradict the constraints declared in the GJSG, in which cases the amendments below dictate the rules to be followed.

Numbered links below reference (and link to) the corresponding sections of the GJSG.

## Source file basics

GJSG [2 Source file basics](https://google.github.io/styleguide/javaguide.html#s2-source-file-basics) is normative, without amendment.

## Source file structure

GJSG [3 Source file structure](https://google.github.io/styleguide/javaguide.html#s3-source-file-structure) is normative, with the amendments below.

#### Ordering of class contents

GJSG [3.4.2 Ordering of class contents](https://google.github.io/styleguide/javaguide.html#s3.4.2-ordering-class-contents):

> The order you choose for the members and initializers of your class can have a great effect on learnability. However, there's no single correct recipe for how to do it; different classes may order their contents in different ways.
>
> What is important is that each class uses  **_some_**  **logical order**, which its maintainer could explain if asked. For example, new methods are not just habitually added to the end of the class, as that would yield "chronological by date added" ordering, which is not a logical ordering.

In concept, we agree with the above. However, we require more predictable structure in the ordering of class members:

1. The order of class members **must** follow this general sequence:

    * `static final` fields
    * `static` (but non-`final`) fields 
    * `final` (but non-`static`) fields
    * other (non-`static`, non-`final`) fields
    * `static` initializers
    * non-`static` initializers
    * constructors
    * methods
    * nested classes

    This still does not fully dictate the order of members within each of the above groups. There's no single correct way of ordering these contents; this may even vary from class to class. However, there are strict rules that must be followed; one is stated in GJSG [**3.4.2.1 Overloads: never split**](https://google.github.io/styleguide/javaguide.html#s3.4.2.1-overloads-never-split); another follows below. 

2. **Never** split accessor/mutator pairs.

    When a class has a pair of methods that follow the JavaBeans naming convention for accessors &amp; mutators (aka _getters_ and _setters_) for some inferable property name, these methods **must** appear sequentially, with no other code between them (not even private members).

## Formatting

GJSG [4 Formatting](https://google.github.io/styleguide/javaguide.html#s4-formatting) is normative, with the amendments below.

#### Vertical whitespace

GJSG [4.6.1 Vertical whitespace](https://google.github.io/styleguide/javaguide.html#s4.6.1-vertical-whitespace):

> _Multiple_ consecutive blank lines are permitted, but never required (or encouraged).

We constrain the use of vertical whitespace further with these rules:

1. Consecutive blank lines are _discouraged but permitted between members of a class,_ but the number of blank lies used **must be** consistent or predictable.

3. Consecutive blank lines are **not permitted** _inside the body_ of an initializer, constructor, or method.

#### Grouping parentheses 

GJSG [4.7 Grouping parentheses: recommended](https://google.github.io/styleguide/javaguide.html#s4.7-grouping-parentheses):

> Optional grouping parentheses are omitted only when author and reviewer agree that there is no reasonable chance the code will be misinterpreted without them, nor would they have made the code easier to read. It is _not_ reasonable to assume that every reader has the entire Java operator precedence table memorized.

To the above, we add this strict rule:

1. Though the compiler treats parentheses around a _single lambda parameter_ as optional, they are **required** in this bootcamp.

#### `switch` statements

GJSG [4.8.4.3 The `default` case is present](https://google.github.io/styleguide/javaguide.html#s4.8.4.3-switch-default):

> Each switch statement includes a  `default`  statement group, even if it contains no code.
> 
> **Exception:**  A switch statement for an  `enum`  type  _may_  omit the  `default`  statement group,  _if_  it includes explicit cases covering  _all_  possible values of that type. This enables IDEs or other static analysis tools to issue a warning if any cases were missed.

The above is further constrained: when a `default` is present (which, as noted, **must be** the case, except when the `case` values are _all_ of the enumerated values of an `enum`), it **must be the last** statement group in the `switch`.

## Naming

GJSG [5 Naming](https://google.github.io/styleguide/javaguide.html#s5-naming) is normative, with the amendments below.

GJSC [5.2.5 Non-constant field names](https://google.github.io/styleguide/javaguide.html#s5.2.5-non-constant-field-names):

> Non-constant field names (static or otherwise) are written in  [`lowerCamelCase`](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case).
>
> These names are typically nouns or noun phrases. For example,  `computedValues`  or  `index`.

GJSC [5.2.6 Parameter names](https://google.github.io/styleguide/javaguide.html#s5.2.6-parameter-names):

> Parameter names are written in  [`lowerCamelCase`](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case).
> 
> One-character parameter names in public methods should be avoided.

GJSC [5.2.7 Local variable names](https://google.github.io/styleguide/javaguide.html#s5.2.7-local-variable-names):
 
> Local variable names are written in  [`lowerCamelCase`](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case).
>
> Even when final and immutable, local variables are not considered to be constants, and should not be styled as constants.

For this bootcamp, the above are further constrained:

1. Parameter names **must** consist of 2 or more characters each, _except for lambda parameters_, which _may have_ single-character names. (If single-character names are used for lambda parameter names, the following naming rules don't apply).

2. Field, parameter, and local variable names of _all_ scalar types (primitives, wrappers, `String`) _except_ `boolean` and `Boolean`, **must be** singular nouns or noun phrases.

3. `boolean` or `Boolean` fields, parameters, and local variables **must be** named using adjectives or adjective phrases.

4. Fields, parameters, and local variables that are references to arrays and collections **must be** named with plural or collective nouns or noun phrases. For example, in a card game application, we may have a field (or several) of type `List<Card>`. Both `cards` and `deck` would be acceptable names for such a field: the former is a plural noun, and the latter is a collective noun.

5. **Do not** name `boolean` or `Boolean` fields with prefixes such as `is`, `are`, or `has`. (The `is` prefix is part of the JavaBeans specification for _accessor_ methods, not fields. Also, all of these prefixes make the field name a verb phrase, rather than an adjectival phrase.)

6. **Do not** use [Hungarian notation](https://en.wikipedia.org/wiki/Hungarian_notation) for field, parameter, or local variable names. That is, do not prefix the name with type or role identifiers.

7. _Avoid_ unnecessary suffixes on field, parameter, and local variable names. For example, _prefer_

   ```java
   private Button update;
   ```

    to
    ```java
   private Button updateButton;
   ```

    Similarly, while the `m` and `s` prefixes on non-`public`, non-`static` and `static` fields (respectively) are often seen in Java source code (including the Android library source code), these are also unnecessary, and _should be avoided._

8. If a non-`static` field is intended to act as a primary key value of a persistent instance (e.g a field annotated with `@PrimaryKey` in an entity class), _prefer_ the simpler `id` to `{entity name}Id`.

## Programming practices

GJSG [6 Programming Practices](https://google.github.io/styleguide/javaguide.html#s6-programming-practices) is normative, with the amendments below.

#### Types

1. If a non-`static` field is intended to act as a non-compound primary key value of a persistent instance (e.g a field annotated with `@PrimaryKey` in an entity class), the type of the field **must be** one of:

    * `UUID`
    * `long` or `LONG`
    * `int` or `Integer`

    Though the type selected _should_ be independent of the RDBMS being used, this is not always possible. For example, when using SQLite, the most performant choice (by far) for  the type of a field intended for use as a non-compound primary key value is `long` or `Long`.
     
#### Access level modifiers

1. Class members are declared at the _lowest practical access level_.

2. The **only fields allowed** to have the `public` access level are those marked `final`. (Note that `interface` fields are implicitly `public static final`, as are the enumerated values of an `enum` class.)

3. Non-`static` fields, even if `final`, **must not** be `public`, _except_ in nested `private` classes.

4. _Prefer_ `protected` accessors and mutators to `protected` fields.

#### `if`-`else if` ladders

1. An `if`-`else if` statement ladder _should_ include a final `else`, _especially_ when one or more of the following holds:

    * `else if` occurs multiple times in the ladder.

    * The purpose of the ladder is to assign one of a number of alternative values to a field.

    * The purpose of the ladder is to assign one of a number of alternative values to a local variable, and that variable is not declared immediately before the ladder.

## Javadoc 

GJSG [7 Javadoc](https://google.github.io/styleguide/javaguide.html#s7-javadoc) is normative, with the amendments below.

#### Where Javadoc is used

GJSC [7.3.1 Exception: self-explanatory methods](https://google.github.io/styleguide/javaguide.html#s7.3.1-javadoc-exception-self-explanatory):

> Javadoc is optional for "simple, obvious" methods like `getFoo`, in cases where there _really and truly_ is nothing else worthwhile to say but "Returns the foo".
> 
> **Important:** it is not appropriate to cite this exception to justify omitting relevant information that a typical reader might need to know. For example, for a method named `getCanonicalName`, don't omit its documentation (with the rationale that it would say only `/** Returns the canonical name. */`) if a typical reader may have no idea what the term "canonical name" means!

While we do not contradicting this exception entirely, we are qualifying it: 

1. Omitting the Javadoc comment entirely, even for a "self-explanatory" method, _should be treated as a truly exceptional condition._ In most cases, there _should_ be some comment, even if just a summary fragment. 

    Remember: Your documentation is likely to be read in an HTML page, without the reader having access to the implementation details of your class. Thus, a comment like

    ```java
    /** Returns age. */`
    ```

    may well be meaningless or of little value, especially if `age` is a `private` field. 

    However, the comment 

    ```java
    /** Returns the age (in generations) of this cell. */
    ``` 

    is potentially much more meaningful, doesn't depend on the reader knowing anything about the existence of an `age` field, and _shouldn't be_ omitted.

2. On the other hand, a Javadoc comment _may be_ omitted for a `@param` or `@return` tag, when such a comment would simply repeat what is already stated in the method-level Javadoc comment. This is most often the case for an accessor or mutator (getter or setter) method, where a comment fragment for the `@return` or `@param` tag (respectively) would tend to repeat the summary fragment of the method itself.

    Note that the reverse of the above exception is not permitted: The method-level Javadoc comment **must not** be omitted if a comment is provided for a `@param`, `@return`, or `@throws` tag of the same method. In other words, if a method includes any Javadoc comments at all, it **must** include at least the summary fragment comment for the method itself.
