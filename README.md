# ABAP Best Practices

A list of common principles of clean ABAP development.

### Contributing

Missing something? Something is wrong? Find a typo?

Feel free to create an issue or make a pull request. More information in [contributors guide](https://github.com/ilyakaznacheev/abap-best-practice/blob/master/CONTRIBUTING.md).

#### Contents

- [Style and Guidelines](#style-and-guidelines) 
    - [Use Pretty-Printer](#use-pretty-printer)
    - [Use Naming Conventions](#use-naming-conventions)
    - [Use snake_case](#use-snake_case)
    - [Use consistent spelling](#use-consistent-spelling)
    - [Avoid obsolete statements](#avoid-obsolete-statements)
- [Coding](#coding)
    - [Follow the Separation of Concerns principle](#follow-the-separation-of-concerns-principle)
    - [Write self-describing code](#write-self-describing-code)
    - [Comment what you do, not how you do](#comment-what-you-do-not-how-you-do)
    - [Be as local as possible](#be-as-local-as-possible)
    - [Do not use magic numbers](#do-not-use-magic-numbers)
    - [Avoid deep nesting](#avoid-deep-nesting)
    - [Use automated code checks](#use-automated-code-checks)
    - [Delete dead code](#delete-dead-code)
    - [Do not ignore errors](#do-not-ignore-errors)
    - [Use class-based exceptions](#use-class-based-exceptions)
    - [Handle exceptions as soon as possible](#handle-exceptions-as-soon-as-possible)
    - [One exception class per one problem, several texts for different problem reasons](#one-exception-class-per-one-problem-several-texts-for-different-problem-reasons)
    - [Check program accessibility](#check-program-accessibility)
    - [Wrap any shared data access into data access classes](#wrap-any-shared-data-access-into-data-access-classes)
    - [Avoid implicit data declarations](#avoid-implicit-data-declarations)
    - [Use built-in Boolean types and constants](#use-built-in-boolean-types-and-constants)
    - [Do not use system fields in UI](#do-not-use-system-fields-in-ui)
    - [Use a suitable category of internal table](#use-a-suitable-category-of-internal-table)
    - [Choose an appropriate way to access a table row](#choose-an-appropriate-way-to-access-a-table-row)
    - [Do not modify an entire table in a loop](#do-not-modify-an-entire-table-in-a-loop)
    - [Exit processing with RETURN](#exit-processing-with-return)
    - [Do not implement logic in dialog modules and event blocks](#do-not-implement-logic-in-dialog-modules-and-event-blocks)
    - [Only use macros in exceptional cases](#only-use-macros-in-exceptional-cases)
- [Language and Translation](#language-and-translation)
    - [Do not hardcode texts](#do-not-hardcode-texts)
    - [Do not use text constants](#do-not-use-text-constants)
    - [Use text tables for text storage in DB](#use-text-tables-for-text-storage-in-db)
    - [Use same original language for all objects in a project](#use-same-original-language-for-all-objects-in-a-project)
    - [Keep translation in mind](#keep-translation-in-mind)
    - [Use only English naming for development objects](#use-only-english-naming-for-development-objects)
    - [Use only translatable texts in UI](#use-only-translatable-texts-in-ui)
    - [Use numbered placeholders in messages](#use-numbered-placeholders-in-messages)
- [Object-Oriented Programming](#object-oriented-programming)
    - [Use classes instead of functional modules or performs whereas possible](#use-classes-instead-of-functional-modules-or-performs-whereas-possible)
    - [Stay SOLID](#stay-solid)
    - [Use GRASP](#use-grasp)
    - [Learn OOP design patterns](#learn-oop-design-patterns)
    - [Respect The Law of Demeter](#respect-the-law-of-demeter)
    - [Avoid classes for helpers, utilities, etc.](#avoid-classes-for-helpers-utilities-etc)
- [Database Usage](#database-usage)
    - [Use OpenSQL whereas possible](#use-opensql-whereas-possible)
    - [Check sy-subrc after DB operations](#check-sy-subrc-after-db-operations)
- [Performance](#perfromance)
    - [Do not perform SELECT in loops](#do-not-perform-select-in-loops)
- [Testing](#testing)
  - [Test only public interface](#test-only-public-interface)
  - [Isolate your tests](#isolate-your-tests)
  - [Keep tests repeatable](#keep-tests-repeatable)
  - [Use unit tests as behavior example and documentation](#use-unit-tests-as-behavior-example-and-documentation)
  - [Keep tests in mind while designing architecture](#keep-tests-in-mind-while-designing-architecture)
- [S/4 Programming Model](#s4-programming-model)
- [BOPF](#bopf)
- [Core Data Services](#core-data-services)
    

## Style and Guidelines

Basic approaches to write clean and pleasant code

### Use Pretty-Printer

Setup Pretty-Printer in settings and run it every time you save your code.

Set same Pretty-Printer settings in your project guidelines to avoid different formatting in the same systems.

### Use Naming Conventions

Choose naming rules for every code or dictionary object you create. Use it to avoid confusion.

In addition, you can setup Code Inspector to check naming convention. 

### Use snake_case

Name your variables, methods, classes, etc. with underscores between words like `lo_table_container->get_sorted_table_data()`. It is the standard convention for ABAP.

[Wikipedia](https://en.wikipedia.org/wiki/Snake_case#Examples_of_languages_that_use_snake_case_as_convention)

### Use consistent spelling

There are many alternative language constructs in ABAP like set of (`=`, `<>`, `>=`, etc.) vs. (`EQ`, `NE`, `GE`, etc.), data declarations, operations, etc.

Chose one of alternatives and use it consistently during you development.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenalternative_langu_guidl.htm)

### Avoid obsolete statements

Some statements in ABAP are outdated. Some of them are deprecated, some are just replaces with new operands. Try to avoid obsolete statements if there some newer alternative exists.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenmodern_abap_guidl.htm)

## Coding

Common rules of writing better code in ABAP

### Follow the Separation of Concerns principle

Separate program into units with minimal overlapping between the functions of the individual units.

### Write self-describing code

Good code should explain itself. Self-describing code doesn't require so much comments or huge documentation.

Make you statements self-explanatory, e.g.:

- choose meaningful names for variables, functions, classes, etc. (`lv_i` → `lv_index`, `lt_data` → `lt_sale_orders`, `cl_util` → `cl_file_io_helper`, `start()` → `run_accounting_document_processing()`)
- group logical steps into methods (e.g. split method `process_document()` into sequence of methods `prepare_document_data()`, `is_doc_creation_possible()`, `lock_tables()`, `create_document()`, `unlock_table()`, etc.)
- decrease amount of lines in a programming block
- decrease nesting level
- avoid implicitness

### Comment what you do, not how you do

Do not comment aspects of implementation, a self-describing code will provide this information for a reader. Comment logic from the business process point of view, the information that the reader can't extract from code.

In the best case, short description o9f business logic unit in method header or before method call will be enough.

### Be as local as possible

Create variables, methods and attributes with as lowest scope as possible. The greater the scope you variable/method has, the more coupled your program is.

### Do not use magic numbers

Avoid hard-coded constants or unnamed variables.

Instead, move them into meaningful variables or constants. Note, that just move text literal with the same name is not enough (`ABC123` ↛ `lc_abc123`), give it a proper description (`ABC123` → `lc_storage_class`)

Bad:
```abap
lo_doc_processor->change_document(
  iv_blart = 'AB'
  iv_bukrs = 'C123'
  iv_popup = lv_x
).
```

Good:
```abap
CONSTANTS:
  lc_clearing_document TYPE blart VALUE 'AB',
  lc_main_company      TYPE bukrs VALUE 'C123'.
DATA:
  lv_show_popup TYPE abap_bool.
*...
lo_doc_processor->change_document(
  iv_blart = lc_clearing_document
  iv_bukrs = lc_main_company
  iv_popup = lv_show_popup
).
```

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenliterals_guidl.htm)

### Avoid deep nesting

Don't write deeply nested loops, cases, and other control structures. Instead of nesting exit from control structure with ifs, checks, and returns.

Bad:
```abap
LOOP AT lt_data ASSIGNING <ls_data>.
  IF a = b.
    IF c = d.
      IF ls_data IS NOT INITIAL.
        ls_data-field = 'aaa'.
      ENDIF.
    ELSE.
      ls_data-field = 'bbb'.
    ENDIF.
  ELSE.
    ls_data-field = 'ccc'.
  ENDIF.
ENDLOOP.
```

Good:
```abap
LOOP AT lt_data ASSIGNING <ls_data>.
  IF a <> b.
    ls_data-field = 'ccc'.
    CONTINUE.
  ENDIF.

  IF c <> d.
    ls_data-field = 'bbb'.
    CONTINUE.
  ENDIF.

  CHECK ls_data IS NOT INITIAL.
  ls_data-field = 'aaa'.
ENDLOOP.
```

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abennesting_depth_guidl.htm)


### Use automated code checks

Use syntax check, extended program check, and code inspector to validate your code syntax, architecture, guidelines, vulnerabilities, and other quality aspects.

[Open Source list of checks for SCI/ATC](https://github.com/larshp/abapOpenChecks)

### Delete dead code

Remove old and unused code. Syntax check and extended program check will help you to find it.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abendead_code_guidl.htm)

### Do not ignore errors

React to errors. It can be either a proper message, or blog entry, or an exception raising. But don't ignore them, otherwise, you will no way to find a cause of any problem.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenreaction_error_guidl.htm)

### Use class-based exceptions

There are several historical types of error in ABAP - system exceptions, classic exceptions, and class-based exceptions. System exceptions aren't allowed to use, classic exceptions are explicit and outdated. There is no reason to use other than class-based exceptions.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenclass_exception_guidl.htm)

### Handle exceptions as soon as possible

Better to handle exception closer on call stack to the raising clause. Handle it when the context of current stack level has enough information for proper handling.

### One exception class per one problem, several texts for different problem reasons

Don't create many different classes for each raising clause. Instead create one class for one kind of problem, i.e. for one type of problem handling.

Create meaningful messages that describe each reason for this kind of problem. Error handling can be the same, but reasons, logging, and user notification will differ.

Example: you are reading a file from PC. There can be a different problem - file is corrupted, the file is empty, access denied, etc. But if you just want to know, if the file was uploaded successful, one exception class `zcl_io_error` will be enough. But create proper messages for each error reason or error type to let the user know, *why* exactly the file was not uploaded.

### Check program accessibility

Ensure that your application is can be used by people with impairments. It means that any information on the user interface should be given in an accessible form:

- input and output fields must have meaningful labels;
- icons must have a tooltip;
- table columns must have a header;
- information must not be expressed by color alone;
- input and output fields on the screen should be grouped as appropriate in frames, each with a meaningful title.

That ensures, that people impairments like color blindness or screen-reader users will have full access to application functionality.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenaccessibility_guidl.htm)

### Wrap any shared data access into data access classes
When you use some shared data like shared memory, shared objects, buffers, etc., don't access them directly. Instead, wrap them into setter and getter methods of static data access class.

It will help you to control access to shared data and easily find any shared data changes via the where-used list. It will also allow you to mock shared data access in unit tests.

### Avoid implicit data declarations

When possible try to not use data declarations like `TABLES`, `NODES`. They create data objects with implicit access.

Use `DATA` instead. Only use `NODES` with LDB. Both create global work areas that will be shared and used through all the program.

Never use `TABLE ... WITH HEADER LINE`. Use either structure, field-symbol or reference with the type of table line or inline declarations and table expressions.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abentable_work_area_guidl.htm)

### Use built-in Boolean types and constants

When you want to use some logical information, use built-in Boolean type `abap_bool` instead of `char1` or other types.

Use constants `abap_true` and `abap_false` for Boolean true and false values. Do not use literals like `'X'`, `' '` - it is a hardcode.

Usage of `space`, `IS INITIAL` or `IS NOT INITIAL` is also not advisable, because they check state of technical implementation of `abap_bool`, but not the sense of real Boolean data object.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abendataobjects_true_value_guidl.htm)

### Do not use system fields in UI

System fields (sy) are technical. They should not be shown to the user.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenuse_ui_guidl.htm)

### Use a suitable category of internal table

Select a suitable table category. For small tables indexes or hashed keys may be redundant, but for large tables always use the following rule:

- index accesses: standard table
- index accesses and key accesses: sorted table
- only key accesses: hashed tables

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenselect_table_type_guidl.htm)

### Choose an appropriate way to access a table row

There are three ways to store an accessed row of an internal table while reading - `INTO` copies row into structure, `ASSIGNING` assigns the row to field symbol and `REFERENCE INTO` creates a reference to the row. The same is for table expressions, but a row storage type being chosen by the category of the result.

The rule is:

- use a work area (structure) if the row type is narrow and the read row is not to be modified.
- use field symbol if the row type is wide or deep and the read row is to be modified.
- use reference if the row type is wide or deep and a reference to the read row is to be passed.

In performance reason better to avoid row copying in loops.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abentable_output_guidl.htm)

### Do not modify an entire table in a loop

While looping through an internal table, don't execute statements that will modify the entire table body. Only modify table row-by-row.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenloop_guidl.htm)

### Exit processing with RETURN

Use only `RETURN` to exit method, function, form, etc. Do not use `CHECK` or `EXIT`.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenexit_procedure_guidl.htm)

### Do not implement logic in dialog modules and event blocks

Instead, call the relevant class method, that encapsulates logic implementation.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abendial_mod_event_block_guidl.htm)

### Only use macros in exceptional cases

Avoid macros usage when possible. Macro has several disadvantages:

- it's unable to debug;
- no syntax check;
- implicit call interface
- no interface parameters type check.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenmacros_guidl.htm)

## Language and Translation

How to make application ready for internationalization and localization

### Do not hardcode texts

Never write texts as inline text literals - they are difficult to find and not possible to translate. Use message class or text symbol instead. 

### Do not use text constants

Do not use constants for storing text (unless it is not text but a char constant). Text constants cannot be translated. Use message class or text symbol instead.

### Use text tables for text storage in DB

Do not store texts in same dictionary tables as other data. Create text tables and assign them to the main table via a foreign key. There are some advantages of text tables:

- They support translation;
- Several texts can be handled for the same object (e.g. short, medium, long text, etc.);
- No search help needed. Texts from the text table will be automatically added into a value list;
- No extra maintaining needed. Text column will be automatically added to a maintenance view;
- Translations can be done with transaction `SE63`.

### Use same original language for all objects in a project

Pick one language and use it as an origin when you create new objects. It will be easier to maintain and translate in future.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenoriginal_langu_guidl.htm)

### Keep translation in mind

Remember that all texts, messages, names, etc. can be translated. Phrases will have a different length (or even different justification) in different languages. Leave some free space for it.

*English phrases are much shorter, than other most-used languages.*

### Use only English naming for development objects

When you name some programming objects like variables, method or class names, or dictionary objects like types, structures, tables, etc. use only English names. 

Do not use other languages, do not combine them. English is understandable in most countries, it is useful and polite to make your code international. Maybe it will be supported by another team from another country.

And it is just a standard and a best practice in a programming world. Don't be a barbarian.

### Use only translatable texts in UI

Send to user only translatable texts, like messages, OTR, text symbols, etc.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abensystem_text_guidl.htm)

### Use numbered placeholders in messages

Use numbered placeholders `&1` - `&4` instead of anonymous placeholders `&`. Order of inserted words may differ in different languages. A translator may need to change the order of the replacement texts when translating message texts. With anonymous placeholders, it isn't possible.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abentrans_relevant_text_guidl.htm)

## Object-Oriented Programming

Here are some OOP best practices, not only SAP-specific but also common practices

### Use classes instead of functional modules or performs whereas possible

*SAP postulates that usage of non-object-oriented code modules is obsolete.*
Use FMs only where are no possibility to use classes (e.g. RFC, update modules, etc.)

ABAP is an enterprise programming language, and OOP can better then others describe complicated
business processes.

Furthermore, all new SAP technologies are class-based.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenabap_obj_progr_model_guidl.htm)

### Stay SOLID

Use SOLID principles in OOP development. Here is five core principles of a flexible and extendable software development:

- [**S**ingle responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle)
- [**O**pen/closed principle](https://en.wikipedia.org/wiki/Open/closed_principle)
- [**L**iskov substitution principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle)
- [**I**nterface segregation principle](https://en.wikipedia.org/wiki/Interface_segregation_principle)
- [**D**ependency inversion principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle)

### Use GRASP

This is a set of patterns and principles for assigning responsibility to classes and objects in the object-oriented design.

There are not only useful behavioral patterns but also very important principles like "low coupling" and "high cohesion".

[Wikipedia](https://en.wikipedia.org/wiki/GRASP_(object-oriented_design))

### Learn OOP design patterns

There is a set of well-known classic OOP design patterns, which are very handy in enterprise development. If you know them, you can easily share design ideas with the team, faster solve architecture challenges and find better problem solutions.

### Respect The Law of Demeter

- each unit should have only limited knowledge about other units: only units "closely" related to the current unit;
- each unit should only talk to its friends; don't talk to strangers;
- only talk to your immediate friends.

That means if class A has access to class B, and B, in turn, has access to class C, class A shouldn't be able to call a method of C directly like `A->B->C->method_of_C()`. B has to have a special method for it.

For example if we want a dog to bark, we will not call `lc_dog->get_head()->get_voice_functions()->run_bark_sound()`, but `lc_dog->bark()`.

[Wikipedia](https://en.wikipedia.org/wiki/Law_of_Demeter)

### Avoid classes for helpers, utilities, etc.

There are many cases in ABAP development when some utility method may be needed. But instead of static methods turn them into powerful objects. Think about function as a class, that is responsible to do this kind of operations. E.g. instead of utility to upload Excel table into string table, create a class that uploads an Excel table in the constructor and able to give it to you in any shape you want (even as a string table). 

You can go ahead and create an interface for table data formatting and implement it for various range of tables - Excel, CSV, XML, etc. Or set a table reader interface as a constructor parameter of table formatter, and implement it for different kinds of data uploads.

In any case, it will be more flexible and more useful as just a static method.

For example:

- `lt_str = cl_file_util=>upload_file_into_str_tab( lv_path )` → `lt_str = NEW cl_file( path )->get_str_tab( )`
- `lv_date = cl_format_util=>format_date_to_gmt( sy-datum )` → `lv_date = NEW cl_date_formatter( sy-datum )->get_gmt( )`

## Database Usage

How to create efficient DB requests

### Use OpenSQL whereas possible

Use OpenSQL (ABAP SQL since 7.53) instead of Native SQL.
It has several advantages:

- syntax check;
- validation check;
- cross-server interpretation;
- common syntax;
- integration with ABAP.

Main causes of NativeSQL usage: performance problems, specific DB functions. 

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abendatabase_access_guidl.htm)

### Check sy-subrc after DB operations

Check DB operation status explicitly by checking `sy-subrc`.

Even if you don't have any error handling, place `sy-subrc` check to make it explicit and let anyone know, that error handling doesn't require.

```abap
IF sy-subrc <> 0. 
  * nothing to do
ENDIF 
```

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenreturn_value_guidl.htm)

## Performance

Use this rules to avoid performance bottlenecks

### Do not perform SELECT in loops

Try to avoid DB operations in loops like `DO-ENDDO`, `WHILE-ENDWHILE`, `LOOP-ENDLOOP`, `PROVIDE-ENDPROVIDE` and `SELECT-ENDSELECT`.

Instead, extract operation criteria from the loop and execute DB operation **once**.

It may be difficult from an architecture perspective to decouple DB operations from loops. To solve this problem you can use Data Access Class (DAC) pattern to encapsulate all DB operations into some class (or set of classes) and perform DB operations as little as possible (e.g. before loop or after loop using lazy load). Then perform a regular read in a loop, but from internal table encapsulated in DAC, not from DB.

## Testing

How to check your code quality

### Test only public interface

In the unit test, you should not test any internal implementation of the class, e.g. private and protected methods.

Test only public methods, it will simulate the way program use the class in "real life". Every internal method will be called from public methods anyway. If not - the method doesn't really used in the class and should be removed.

The test should check the behavior of the class, not it's implementation. You can refactor or change the implementation, but the test will be the same. If the behavior of the class will not change, no change required in the test too.

Leave any internal logic encapsulated by testing only public methods.

### Isolate your tests

Tests should not affect each other. Each test should run separately in isolated environments - e.g. you should clean up the environment before/after a test run. 

### Keep tests repeatable

Same test input should give the same output, actual values should meet expectations, test behavior should be repeatable.

### Use unit tests as behavior example and documentation

A unit test shows how a program module should work. If you will test it the same way, as it is used in a real program, it will be the best documentation (with examples!) for a developer.

### Keep tests in mind while designing architecture

Only well-designed programs can be easily tested. To make it easier for unit testing make your modules able to be tested independently. Make them low coupled and highly cohesive, remove unnecessary relations, use dependency injection, hide dependent classes behind interfaces to mock them in tests. SoC, SOLID, GRASP are your friends.

## S/4 Programming Model

todo

## BOPF

todo

## Core Data Services

todo
