---
layout: post
title:  code smells episode 1 - that shit is vile
date:   2018-06-25
tags: [ruby]
---
Mapping code smells and how to get rid of those dank odors.

| Smell | Conditions | Spells  |
| ----- | ---------- | ------- |
| **Duplicated code** | Same expression in two or more methods of the same class | `Extract Method` |
|                | Same expression in two sibling subclasses | `Extract Method`, `Pull Up Method` |
|                | Similar expressions but not the same | `Extract Method`, `Form Template Method` |
|                | Methods that do same thing with different approaches | `Substitute Algorithm` |
|                | Same expression in the middle of two or more methods | `Extract Surrounding Method` |
|                | Same expressions in two or more unrelated classes | `Extract Class`, `Extract Module` |
| **Long method**  | Too many temporary variables | `Replace Temp with Query`, `Replace Temp with Chain` |
|                | Too many method parameters | `Introduce Parameter Object`, `Preserve Whole Object` |
|                | Still too many method parameters | `Replace Method with Method Object` |
|                | Conditional expressions | `Decompose Conditional` |
|                | Loops | `Collection Closure Methods` |
| **Large class** | Too many responsibilities | `Extract Class`, `Extract Subclass`, `Extract Module` |
| **Long parameter list** | Parameter data can be obtained by asking an object | `Replace Parameter with Method` |
|                | Multiple parameters are sourced from an object | `Preserve Whole Object` |
|                | Multiple parameters are sourced from no logical object | `Introduce Parameter Object`, `Introduce Named Parameter` |
| **Divergent Change** | Class is often changed in different ways for different reasons | `Extract Class` |
| **Shotgun Surgery** | One change requires a slew of changes in a lot of different areas | `Move Method`, `Move Field`, `Inline Class` |
| **Feature Envy** | A method that seems more interested in another class than its own | `Move Method`, `Extract Method` |
| **Data Clumps** | Multiple data items that appear as instance variables | `Extract Class` |
|                 | Multiple data items that appear as method parameters | `Introduce Parameter Object`, `Preserve Whole Object` |
| **Primitive Obsession** | Multiple individual data values as primitives | `Replace Data Value with Object` |
|                 | Have conditionals that depend on typing | `Replace Type Code with Polymorphism`, `Replace Type Code with Module Extension`, `Replace Type Code with State/Strategy` |
|                 | Have group of instance variables that should go together | `Extract Class` |
|                 | Excessive primitives in parameter list | `Introduce Parameter Object` |
|                 | Dissecting array | `Replace Array with Object` |
| **Case Statements** | Multiple case statements in different places that all need to be updated when new requirements are introduced | `Extract Method`, `Move Method`, `Replace Type Code with Polymorphism`, `Replace Type Code with Module Extension`, `Replace Type Code with State/Strategy` |
|                 | Limited number of case statements | `Replace Parameter with Explicit Methods` |
|                 | Have a nil case conditional | `Introduce Null Object` |
| **Parallel Inheritance Hierarchies** | Special case of Shotgun Surgery where everytime a subclass is created from one class, another subclass must also be created | `Move Method`, `Move Field` |
| **Lazy Class** | Classes or modules that aren't doing enough | `Collapse Hierarchy` |
|                | Classes or modules that are borderline useless | `Inline Class`, `Inline Module` |
| **Speculative Generality** | Classes or modules that aren't doing enough | `Collapse Hierarchy` |
|                | Unnecessary delegation | `Inline Class` |
|                | Methods with unused parameters | `Remove Parameter` |
|                | Methods with odd names | `Rename Method` |
| **Temporary Field** | An object whose instance variables are set only in certain conditions | `Extract Class`, `Introduce Null Object` |
| **Message Chains** | Long method chains that enforce tightly coupled code | `Hide Delegate`, `Extract Method`, `Move Method` |
| **Middle Man** | Encapsulation gone too far, e.g., when a class's interface has too many methods only delegating to other classes | `Remove Middle Man`, `Inline Method`, `Replace Delgation with Hierarchy` |
| **Inapproppriate Intimacy** | Classes that are too tightly coupled, often interacting with private parts | `Move Method`, `Move Field`, `Change Bidirectional Association to Unidirectional`, `Extract Class`, `Hide Delegate`, `Replace Inheritance with Delegation` |
| **Alternative Classes with Different Interfaces** | Methods with different interfaces that perform the same function | `Rename Method`, `Move Method`, `Extract Module`, `Introduce Inheritance` |
| **Incomplete Library Class** | Existing library class missing functionality | `Move Method` |
| **Data Class** | Classes that have attributes and no other behavior or responsibility | `Remove Setting Method`, `Encapsulate Collection`, `Move Method`, `Extract Method`, `Hide Method` |
