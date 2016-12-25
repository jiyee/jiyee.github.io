---
title: Enemy of the State 阅读摘要
date: 2016-08-08
---
Justin Spahr-Summers
2014

state and mutation
- Simple:less concepts and concerns
- Complexity: mixing “complecting” 

All systems have essential complexity.

State is hard to test.

State is an implicit input that can change unexpecteble

## Minimize states.
- Values
- Purity
- Isolation

### Values
Structs, Enums, Copied (not shared)

Value types are immutable in swift.

Keys:
**Variables mutate**
**Values never change**

Values are automatically **thread-safe**
Values are automatically **predicatable**

### Pure functions
same inputs always yield the same result.
Must not have *observable* side effects.

### Isolation
Objects should have only one reason to change
isolate unrelatted piece of states
Before log in and After log in ViewModel 差异

stateless core, stateful shell
