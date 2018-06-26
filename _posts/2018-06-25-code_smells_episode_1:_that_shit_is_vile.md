---
layout: post
title:  code smells episode 1: that shit is vile
date:   2018-06-25
tags: [ruby]
---
Mapping code smells and how to get rid of those dank odors.

| Smell | Conditions | Spells  |
| ----- | ---------- | ------- |
| **Duplicated code** | Same expression in two or more methods of the
same class | `Extract Method` |
|                | Same expression in two sibling subclasses | `Extract
Method`, `Pull Up Method` |
|                | Similar expressions but not the same | `Extract
Method`, `Form Template Method` |
|                | Methods that do same thing with different approaches
| `Substitute Algorithm` |
|                | Same expression in the middle of two or more methods
| `Extract Surrounding Method` |
|                | Same expressions in two or more unrelated classes |
`Extract Class`, `Extract Module` |
| **Long method**  | Too many temporary variables | `Replace Temp with
Query`, `Replace Temp with Chain` |
|                | Too many method parameters | `Introduce Parameter
Object`, `Preserve Whole Object` |
|                | Still too many method parameters | `Replace Method
with Method Object` |
|                | Conditional expressions | `Decompose Conditional` |
|                | Loops | `Collection Closure Methods` |
| **Large class** | Too many responsibilities | `Extract Class`,
`Extract Subclass`, `Extract Module` |
| **Long parameter list** | Parameter data can be obtained by asking an
object | `Replace Parameter with Method` |
|                | Multiple parameters are sourced from an object |
`Preserve Whole Object` |
|                | Multiple parameters are sourced from no logical
object | `Introduce Parameter Object`, `Introduce Named Parameter` |
