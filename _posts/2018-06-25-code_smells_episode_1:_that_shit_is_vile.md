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
|                     | Same expression in two sibling subclasses |
`Extract Method`, `Pull Up Method` |
|                | Similar expressions but not the same | `Extract
Method`, `Form Template Method` |
|                | Methods that do same thing with different approaches
| `Substitute Algorithm` |
|                | Same expression in the middle of two or more methods
| `Extract Surrounding Method` |
|                | Same expressions in two or more unrelated classes |
`Extract Class`, `Extract Module` |
