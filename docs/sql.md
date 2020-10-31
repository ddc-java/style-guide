---
title: SQL
menu: SQL
order: 20
---

*[GJSG]: Google Java Style Guide
*[JSON]: JavaScript Object Notation
*[XML]: Extensible Markup Language
*[ORM]: object-relation mapping
*[SSG]: SQL Style Guide

## Overview

In SQL programming, there really isn't an equivalent to GJSG, or even to the earlier, more basic [Code Conventions for the Java&trade; Programming Language](https://www.oracle.com/java/technologies/javase/codeconventions-contents.html) (last updated by Sun in 1999, and currently made available for archive purposes by Oracle). Most of the rules included in both of those publications (which differ only in a few details, and in elements added to the Java language after the final revisions to the first) are followed by the overwhelming majority of Java developers; however, there isn't a SQL style guide with anything close to the same rate of formal adoption or informal conformance.

In this style guide, the focus is primarily on naming (including casing), and on a few general formatting rules. Most of these are long-established, and widely followed---though with plenty of variation from organization to organization. One benefit of this flexibility is that many SQL formatters can be configured to apply several of these rules; this includes the **Code/Reformat code** feature of IntelliJ, which---when using the default settings---will apply all of the formatting rules (but not the structural or naming &amp; letter-casing rules) given here.

This style guide is deliberately oriented toward using SQL via an _object-relation mapping_ (ORM) library. That is, it's generally assumed that an ORM such as Hibernate or Room will be be managing persistence, and---in most cases---generating the SQL DDL code to implement the data model in a database schema. Thus, there is not much attention placed on formatting details for SQL statements; instead, naming and letter casing of keywords and identifiers are the main focus, with a secondary focus on formatting (primarily for the purpose of producing technical documentation).

Some of the rules here are based on Simon Holywell's [SQL Style Guide](https://www.sqlstyle.guide/) (SSG). However, where the DDC Java style rules incorporate those in GJSG with only a few amendments, the DDC SQL style guide deviates significantly from SSG. Thus, the rules below should be treated as normative, without reference to similar or contradicting rules in the SSG. 

On the other hand, beyond newline and indentation rules, this style guide doesn't address any other specifics of vertical alignment---a topic of great disagreement in SQL code formatting. SSG or other SQL style guides may be consulted for this purpose.

## Files

1. All SQL source code files **must be** named in `spinal-case`, with a `.sql` extension, _unless_ another naming convention is a requirement of a tool or library being used.

2. If multiple SQL statements are included in a single `.sql` file, or in a string literal in a source code file of another type, the code in the file (or literal) **must be** executable as written, in order. Minimally, this implies:

    1. Every statement **must be** terminated by a semicolon (`;`).

    2. The order of statements **must be** such that if all are executed in order, no constraints are violated, and all dependencies are satisfied. For example, an `ALTER TABLE` statement must either follow (immediately or otherwise) the corresponding `CREATE TABLE` statement, or it must be conditioned (using `IF EXISTS`) on the existence of the table in question.

## Casing

1. SQL keywords (including the names of built-in functions) **must be** written in `UPPERCASE`. (In some SQL dialects, some multi-word keywords have underscores; these would be written in `UPPER_SNAKE_CASE`.)

2. All explicitly named persistent or transient SQL objects (tables, views, columns, aliases, indices, sequences, stored procedures, user-defined functions) **must be** named using `lower_snake_case`. 

## Naming

### General

1. **Do not** use abbreviations (including acronyms and other initialisms), _unless_ the abbreviation is widely understood and commonly used in place of the unabbreviated form, _and only if_ the unabbreviated form is significantly longer than the abbreviation.

### Tables &amp; views

1. If the name of table is generated automatically (e.g. by an ORM from the name of an entity class), the automatically generated name **must be** used, _even if it does not conform_ to the casing and naming rules stated here, _unless_ one of the following is true:

    1. The automatically generated name is unacceptable---e.g. it conflicts with a SQL keyword---and must be specified explicitly.

    2. The table is a _join table_, automatically defined by an ORM to effect a many-to-many relationship. The name of such a table _may be_ left as generated, or a name _may be_ specified explicitly.

2. Explicitly named tables and views **must be** named with singular nouns or noun phrases. (This corresponds to the convention for Java class names.) 

3. In table and view names, **do not** use unnecessary prefixes, such as `tbl_`, `table_`. Similarly, **do not** use unnecessary suffixes (`_tbl`, `_table`, etc.).

### Table &amp; view aliases

Table and view aliases (aka _correlations_) are commonly defined in join expressions, and referenced in column selection lists, `ON` clauses, `WHERE` clauses, `ORDER BY` clauses, etc.

The rules below _do not apply_ to SQL statements generated by an ORM. However, in explicitly specified SQL statements using table/view aliases, follow these rule:

1. Aliases _should be_ constructed from initialisms for the correlated table or view name. For example, an alias for a table named `user_profile` would be named `up`, while an alias for `building_location_detail` would be `bld`.

2. If a given table is included 2 or more times in a join expression, **all** of its aliases **must be** numbered---e.g. `bld1`, `bld2`. (Underscores _may be_ used between the initialism and the number: `bld_1`, `bld_2`; as usual, consistency is _strongly_ advised.)

### Columns

1. If using Hibernate (or a similar ORM), which automatically maps `lowerCamelCase` field names in an entity class to `lower_snake_case` column names (e.g. `someData` to `some_data`), **do not** specify a column name explicitly, _unless both of the following conditions hold_:

    * The ORM is being used to map a new Java data model to an existing database schema. 
    
    * The ORM's column name mapping scheme does not correctly translate a given field name to the name of the existing column.

2. Explicitly named columns of all types other than `BOOLEAN` **must be** named with singular nouns or noun phrases.

3.  `BOOLEAN` columns **must be** named with adjectives or adjectival phrases.

4. With a few exceptions (e.g. columns that are used as primary keys, foreign keys, or unique keys), **do not** use the table name as a column name prefix. For example, in a table named `account`, use the column name `balance` instead of `account_balance`.

5. For `BOOLEAN` columns, **do not** use prefixes like `is_`, `has_`, `contains_`, etc. (All of these make the name a verb phrase, rather than an adjectival phrase.) For example, use `enabled` instead of `is_enabled`. 

6. All columns used as non-compound (single-column) primary keys or foreign keys **must be** named with the `_id` suffix. **No other** columns are permitted to use that suffix.

7. All columns constituting non-compound (single-column) primary keys **must be** named with the `{table}_id` pattern (or `{entity}_id`, for a table corresponding to an entity class). For example, the primary key column for an `appointment` table would be named `appointment_id`.

8. Foreign keys _may be_---but need not be---named to match the corresponding primary keys. An acceptable alternative approach is to name the foreign key according to the role played by the referenced table.

    For example, assume we have the table `contributor`, with the primary key `contributor_id`. We also have another table, `post`, with a foreign key that references `contributor.contributor_id`. This foreign key column might also be named `contributor_id`---or it could be named something like `author_id`, since the referenced contributor record represents the author of any given post.

9. Columns constituting non-compound (single-column) unique keys (other than primary keys, which are implicitly unique) _should be_ given names that indicate that values in the column uniquely identify rows. _Suggested_ name endings that are commonly (but not exclusively) used for this purpose include `_name`, `_code`, and `_key`. 

### Column aliases

1. Column aliases **must** follow the same rules as column names.

### Subprograms &amp; triggers

1. Stored procedures and triggers **must be** named using verbs or verb phrases.

2. **Do not** use unnecessary prefixes in stored procedure or trigger names, such as `sp_`, `tr_`, or `proc_`. These are similarly unnecessary in suffix form; **don't use them**.

3. Functions may be defined to return scalar values, composite values (this may be thought of as a single row), or rowsets; the naming rules reflect these possibilities:

    1. Scalar-valued functions **must be** named according to the same rules as columns.

    2. Composite- and rowset-valued functions **must be** named according to the same rules as tables and view.

4. **Do not** use unnecessary prefixes, such as `func_`, or `fn_`. These are equally lacking in meaningful information in suffix form; **don't use them**.

### Indices

1. In most SQL dialects, index names can be generated automatically, and need not be specified explicitly. If that isn't the case for the underlying RDBMS, but an ORM is used, the ORM virtually always generates index names automatically. _Prefer the automatically generated name_ to an explicitly specified name.

2. If an index must be named explicitly, the name **must** follow the applicable pattern from the list below.

    1. For an index backing a primary key comprised of a single column:
    
        ```text
        pk_{table}
        ```

        **Placeholders**

        `{table}`
        : Name of the given table.

    2. For an index backing a non-primary key (compound or otherwise) or a compound primary key:
    
        ```text
        {prefix}_{table}_{column1}[_{column2}[…]]
        ```

        **Placeholders**

        `{prefix}`
        : Type prefix: `pk` for primary key index, `uq` for unique key index, `ix` for non-unique index.

        `{table}`
        : Name of the table on which the given index is defined.
        
        `{column1}` 
        : Name of the first column used in the index.
        
        `{column2}`
        : Name of the second column used in the index (if applicable).

        ...
        : Additional column names (if any) used in the index, separated by underscores (`_`) .

        The square brackets enclose optional parts of the name, and must be replaced accordingly; the brackets themselves are not allowed in the actual index name.

### Sequences

Many RDBMSs use sequences to implement auto-numbered (aka auto-incremented) fields.

1. When using an ORM (and in some cases when an ORM is not used), sequences are defined implicitly, with an automatically generated name. When this is the case, the generated name **must be** used.

2. If a sequence name must be specified explicitly, the name **must** follow the applicable pattern from the list below.

    1. For a sequence providing values for a non-compound primary key column:

        ```text
        {column}_seq
        ```

        **Placeholders**

        `{column}`
        : Name of the column that will be assigned values from the given sequence.
        
    2. For a sequence providing values for a column that is not a non-compound primary key: 

        ```text
        {table}_{column}_seq
        ```

        **Placeholders**

        `{table}`
        : Name of the table containing the column that will be assigned values from the given sequence.
        
        `{column}`
        : Name of the column that will be assigned values from the given sequence.
        
## Types

1. If an ORM is being used, and the ORM is capable of mapping a Java type to a SQL type automatically, the SQL type _should be_ left as mapped, unless the resulting type violates one of the strict rules that follow.

2. Columns of character types (e.g. `CHAR`, `VARCHAR`, `TEXT`) _may be_ used (individual or compounded with other columns) in unique keys, but **must not** be used in primary keys.

3. Any table that is _not_ a join table (used to effect a many-to-many relationship) **must have** a non-compound (single-column) primary key, with automatically generated values of one of these types:

    * `UUID`
        
        For a primary key, a value of this type **must be** stored and indexed as a 16-byte (128-bit) binary value; in some RDBMSs, this requires that we declare the actual type as `BINARY(16)`, `CHAR(16) FOR BIT DATA`, etc.
        
    * `BIGINT`
    
    * `INT`

        For a primary key of either of the last 2 types above, the `UNSIGNED` modifier should be used if possible (not all RDBMS platforms support this).

## Formatting

### Overview

Our style guide does not fully specify the allowed or required formatting of SQL code across multiple lines. In particular, as noted below, single-line statements are permitted in some contexts; in such cases, the conventions for new lines and indentation are moot. Nonetheless, the rules specified do apply, along with the naming and casing rules above, with any conditions as stated. Beyond that, _consistency is critical_: Apply the same formatting rules throughout a project.

### Code formatting tools

1. In this bootcamp, the most important guideline to follow for SQL code formatting---apart from following the naming and casing rules, of course---is simple: _Use a formatting tool!_ This might be the IntelliJ **Code/Format Code** command, or any of a number of other SQL code formatting tools, including those listed in the SQL section of [**Formatting tools**](resources.md#formatting-tools) reference section of this document.

    Some of these tools (including the one provided by IntelliJ) are dialect-specific: the appropriate dialect of SQL must be configured in order to format the code properly. In most cases, formatting works much better if the SQL code is syntactically correct for the chosen dialect; for the IntelliJ SQL formatter, this is required.
    
### Single-line statements

In some contexts, even a reasonably complex SQL statement _may be_ written in a single line. In the single-line form of a statement, the rules for newlines and indentation (below) don't apply.

1. A SQL statement used as the `value` argument of a Room or Spring Data `@Query` annotation _may be_ written in a single line, directly in the annotation. However, if the statement has a multi-table/view `FROM` clause, consider writing it in multiple lines (concatenated as needed) as a `static final String` field, and then referencing that constant in the `value` argument of the `@Query` annotation.

2. A subquery (correlated or not) within another statement _may be_ written in a single line, in parentheses. However, you are _encouraged_ to split the subquery into multiple lines, indented as appropriate---_especially_ if the subquery involves multiple tables, views, or subqueries. 

### New lines

1. The following keywords (and keyword combinations) **must** start a new line (with the exceptions noted for single-line statements, above):

    * `ADD`
    * `ALTER`
    * `CREATE`
    * `DECLARE`
    * `DELETE`
    * `DROP`
    * `ELSE`
    * `END`, when used to close a block that starts with `BEGIN`.
    * `FROM`
    * `GROUP BY`
    * `HAVING`
    * `IF`
    * `INSERT`
    * *`[JOIN_TYPE]`*`JOIN` , where *`JOIN_TYPE`* (if present) is one of `INNER`, `LEFT`, `RIGHT`, `FULL OUTER`, `CROSS`, `NATURAL`, etc.
    * `MODIFY`
    * `ORDER BY`
    * `SELECT`
    * `SET`
    * `TRUNCATE`
    * `UPDATE`
    * `VALUES`
    * `WHERE`

2. The second, and every subsequent, column expression in a `SELECT` column list **must** start a new line. The first column expression _should_ start a new line.

3. When multiple tables are includes in the `FROM` clause of a `SELECT` statement, and the `JOIN` keyword is not used to declare join conditions, each table name after the first **must** start a new line. (Before the adoption of the `JOIN` keyword, this form was commonly used with a join condition expressed in the `WHERE` clause; _avoid_ this practice.)

4. Every column, table constraint, or primary key definition in a `CREATE TABLE` statement **must** start a new line. Column-level constraints, and the `PRIMARY KEY` modifier on a column definition, _should_ be on the same line as the column definition.

5. The second, and every subsequent, column assignment following `SET` in an `UPDATE` statement **must** start a new line. The first column assignment _should_ start a new line.

6. The closing brace of a `CREATE TABLE` statement **must** start a new line.

7. The opening brace of a `CREATE TABLE` statement _should not_ (but is permitted to) start a new line.

8. In the Boolean expression of the `WHERE` or `HAVING` clause, the `AND`, `OR`, and `NOT` conjunction operators _should_ start a new line. 

9. Between `CASE` and the matching `END`, each `WHEN` and `ELSE` _should_ start a new line. If this is done, the `END` **must** start a new line.
 
### Indentation

In most contexts, _indentation_ refers to the spacing between the absolute start of a line and the first non-white-space character, with the implicit understanding that items indented to the same level are aligned along the left edge of the text. However, this is not always the case in SQL: Some style guides and formatting tools use indentation to produce a consist alignment of the _right_ edge of keywords (or the first words in keyword phrases) Either of these indentation approaches is acceptable; _favor_ consistency over convenience.

Regardless of your left vs. right alignment preference, follow these rules:

1. Indentation **must** use space characters instead of tabs.

2. The keywords starting the clauses of a SQL statement (e.g. `FROM`, `WHERE`, etc. in a `SELECT` statement) **must be** indented to the same level as the statement itself.
    
    As stated above, this does not _necessarily_ mean that the clause and statement keywords are vertically aligned at the start (left-most character) of the keyword. Many SQL style guides and formatting tools dictate a right alignment of keywords, creating a _river_---a vertical column of whitespace after the keywords. So the following 2 forms are both acceptable.

    **Left-aligned**:
    ```sql
    SELECT
        ...
    FROM
        ...
    WHERE
        ...
    GROUP BY
        ...
    HAVING
        ...
    ORDER BY
        ...
    ```

    **Right-aligned**

    ```sql
    SELECT
        ...
      FROM
        ...
     WHERE
        ...
     GROUP BY
        ...
    HAVING
        ...
     ORDER BY
        ...
    ```

    Note that in the second form, the _end_ of each clause keyword (or the first word in a keyword phrase) is aligned. This is not required, but it is a fairly common practice in SQL formatting, and is allowed.

3. When a new line is added to a clause of a SQL statement---e.g. for the second and subsequent columns in the column list of a `SELECT` statement, or a `JOIN` in the `FROM` clause of a `SELECT` statement---the new line **must be** indented further to the right of the indent level of the clause itself.

    Here's an example `SELECT` statement with a `JOIN`, demonstrating this rule:

    ```sql
    SELECT
        a.article_id,
        a.title,
        up.name
    FROM
        article AS a
        JOIN user_profile AS up
            ON up.user_id = a.author_id;
    ```

4. The column, table constraint, and primary key definitions of a `CREATE TABLE` statement are contained within **must be** indented within the curly braces of the statement.

5. For `BEGIN`...`END` blocks, the `END` keyword **must be** indented to the same level as the matching `BEGIN`.

6. For `CASE`...`END` blocks that extend over multiple lines, the `END` keyword **must be** indented to the same level as the matching `CASE`.

7. All of the `WHEN` and `ELSE` keywords in a multiline `CASE` statement **must be** indented to the same level, and to the right of the enclosing `CASE` and `END`. 

8. All statements contained within `BEGIN` and `END` **must be** indented to the same level, and to the right of  the `BEGIN` and `END`.

### Vertical whitespace

1. When a SQL file contains multiple statements, every semicolon (`;`) statement terminator **must be** followed by _at least_ (and preferably _exactly_) 1 blank line.

## Programming practices

The rules below _do not apply_ when a SQL statement expression is generated automatically by an ORM; however, in explicitly specified SQL statements, they do apply.

### Joins

1. **Do not** use the old-style form, where the `JOIN` keyword is not used, and the join condition is specified only in the `WHERE` clause. 

    **Bad**

    ```sql
    SELECT
        a.article_id,
        a.title,
        up.name
    FROM
        article AS a,
        user_profile AS up
    WHERE
        up.user_id = a.author_id;
    ```

    **Good**
    
    ```sql
    SELECT
        a.article_id,
        a.title,
        up.name
    FROM
        article AS a
        JOIN user_profile AS up
            ON up.user_id = a.author_id;
    ```

2. **Do not** use correlated subqueries in place of `JOIN`. For example, the following is equivalent to the example statements in point 1, but is not acceptable, since there is a `JOIN` equivalent (as seen above):

    **Bad**
        
    ```sql
    SELECT
        a.article_id,
        a.title,
        (
            SELECT 
                up.name 
            FROM 
                user_profile up 
            WHERE 
                up.user_id = a.author_id
        ) as name
    FROM
        article AS a;
    ```

    Note: In some cases, a correlated subquery can't be avoided without complicating the code significantly. However, _almost never_ does this happen outside of a `JOIN`. In other words, we're using the correlated subquery to define a table source used in the join. This use of a correlated subquery---as a participant in a `JOIN`, rather than instead of a `JOIN`---is permitted, but _should be avoided when possible._
    
### Table &amp; view aliases

1. Table/view aliases **must be** used in multi-table `FROM` clauses---that is, in all join expressions---unless the table name is 5 characters or less in length.

2. The `AS` keyword **must be** used when declaring an alias.

3. Table/view aliases _should be_ constructed from initialisms for the correlated table or view name. For example, an alias for a table named `user_profile` would be named `up`, while an alias for `building_location_detail` would be `bld`.

4. If a given table is included 2 or more times in a join expression, **all** of its aliases **must be** numbered---e.g. `bld1`, `bld2`. (Underscores _may be_ used between the initialism and the number: `bld_1`, `bld_2`; as usual, consistency is _strongly_ advised.)

### Column aliases

1. A column alias **must be** used whenever an item in a query select list is a computed expression, rather than a simple column name.

2. The `AS` keyword **must be** used when declaring a column alias.
