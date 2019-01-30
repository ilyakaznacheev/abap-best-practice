# ABAP Best Practices

A list of common principles of clean ABAP development.

### Contributing

Missing something? Something is wrong? Find a typo?

Feel free to create an issue or make a pull request. More information in [contributors guide](https://github.com/ilyakaznacheev/abap-best-practice/blob/master/CONTRIBUTING.md).

#### Contents

- [Style and Guidelines](#style-and-guidelines) 
    - [Use Pretty-Printer](#Use-Pretty-Printer)
    - [Use Naming Conventions](#Use-Naming-Conventions)
    - [Use snake_case](#Use-snake_case)
    - [Use consistent spelling](#Use-consistent-spelling)
    - [Avoid obsolete statements](#Avoid-obsolete-statements)
- [Coding](#coding)
    - [Follow the Separation of Concerns principle](#Follow-the-Separation-of-Concerns-principle)
    - [Write self-describing code](#Write-self-describing-code)
    - [Comment what you do, not how you do](#Comment-what-you-do-not-how-you-do)
    - [Be as local as possible](#Be-as-local-as-possible)
    - [Avoid deep nesting](#Avoid-deep-nesting)
    - [Use automated code checks](#Use-automated-code-checks)
    - [Delete dead code](#Delete-dead-code)
- [Language and Translation](#language-and-translation)
    - [Do not hardcode texts](#Do-not-hardcode-texts)
    - [Do not use text constants](#Do-not-use-text-constants)
    - [Use text tables for text storage in DB](#Use-text-tables-for-text-storage-in-DB)
    - [Use same original language for all objects in a project](#Use-same-original-language-for-all-objects-in-a-project)
    - [Keep translation in mind](#Keep-translation-in-mind)
    - [Use only English naming for development objects](#Use-only-English-naming-for-development-objects)
- [Object-Oriented Programming](#object-oriented-programming)
    - [Use classes instead of functional modules or performs whereas possible](#Use-classes-instead-of-functional-modules-or-performs-whereas-possible)
- [Database Usage](#database-usage)
    - [Use OpenSQL whereas possible](#Use-OpenSQL-whereas-possible)
- [Performance](#perfromance)
    - [Do not perform SELECT in loops](#Do-not-perform-SELECT-in-loops)
- [Testing](#testing)
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
LOOP AS lt_data ASSIGNING <ls_data>.
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

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenabap_obj_progr_model_guidl.htm)

### Use automated code checks

Use syntax check, extended program check, and code inspector to validate your code syntax, architecture, guidelines, vulnerabilities, and other quality aspects.

### Delete dead code

Remove old and unused code. Syntax check and extended program check will help you to find it.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abendead_code_guidl.htm)

## Language and Translation

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

## Object-Oriented Programming

Here are some OOP best practices, not only SAP-specific but also common practices

### Use classes instead of functional modules or performs whereas possible

*SAP postulates that usage of non-object-oriented code modules is obsolete.*
Use FMs only where are no possibility to use classes (e.g. RFC, update modules, etc.)

ABAP is an enterprise programming language, and OOP can better then others describe complicated
business processes.

Furthermore, all new SAP technologies are class-based.

## Database Usage

### Use OpenSQL whereas possible

Use OpenSQL (ABAP SQL since 7.53) instead of Native SQL.
It has several advantages:

- syntax check;
- validation check;
- cross-server interpretation;
- common syntax;
- integration with ABAP.

Main causes of NativeSQL usage: performance problems, specific DB functions. 

## Performance

### Do not perform SELECT in loops

Try to avoid DB operations in loops like `DO-ENDDO`, `WHILE-ENDWHILE`, `LOOP-ENDLOOP`, `PROVIDE-ENDPROVIDE` and `SELECT-ENDSELECT`.

Instead, extract operation criteria from the loop and execute DB operation **once**.

It may be difficult from an architecture perspective to decouple DB operations from loops. To solve this problem you can use Data Access Class (DAC) pattern to encapsulate all DB operations into some class (or set of classes) and perform DB operations as little as possible (e.g. before loop or after loop using lazy load). Then perform a regular read in a loop, but from internal table encapsulated in DAC, not from DB.


## Testing

todo

## S/4 Programming Model

todo

## BOPF

todo

## Core Data Services

todo
