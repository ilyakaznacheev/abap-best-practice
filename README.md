# ABAP Best Practices

A list of common principles of clean ABAP development.

### Contributing

Missing something? Something is wrong? Find a typo?

Feel free to create an issue or make a pull request. More information in [contributors guide](https://github.com/ilyakaznacheev/abap-best-practice/blob/master/CONTRIBUTING.md).

#### Contents

- [Style and Guidelines](#style-and-guidelines) 
    - [Use Pretty-Printer](#use-pretty-printer)
    - [Use Naming Conventions](#use-naming-conventions)
- [Coding](#coding)
- [Language and Translation](#language-and-translation)
    - [Do not hardcode texts](#Do-not-hardcode-texts)
    - [Do not use text constants](#Do-not-use-text-constants)
    - [Use text tables for text storage in DB](#Use-text-tables-for-text-storage-in-DB)
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

## Coding

todo

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

## Object-Oriented Programming

Here are some OOP best practices, not only SAP-specific but also common practices

### Use classes instead of functional modules or performs whereas possible

*SAP postulates that usage of non-object-oriented code modules is obsolete.*
Use FMs only where are no possibility to use classes (e.g. RFC, update modules, etc.)

ABAP is an enterprise programming language, and OOP can better then others describe complicated
business processes.

Furthermore, all new SAP technologies are class-based.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenabap_obj_progr_model_guidl.htm)

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
