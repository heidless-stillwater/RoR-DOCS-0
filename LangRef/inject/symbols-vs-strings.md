

## [Ruby Symbols vs. Strings](https://medium.com/@lcriswell/ruby-symbols-vs-strings-248842529fd9)

Ruby symbols are defined as **“scalar value objects used as identifiers, mapping immutable strings to fixed internal values.”** Essentially what this means is that symbols are **immutable strings**.

### Why use symbols over strings?

The immutable nature of symbols makes them very valuable in programming because mutable objects can cause bugs that are difficult to detect.

**Memory Efficient**: Since symbols are immutable, they always refer to the same object and the same place in memory.

A program that uses symbols over strings (when possible) will run more efficiently.

### When to use symbols?

*“If the textual content of the object is important, use a **String**. If the identity of the object is important, use a **Symbol**.”*

Symbols come in handy when you need a unique identifier to hold a value, **such as a key in a hash**

NOTE: **:symbol** & **symbol:** are syntactically identical

When in doubt, if a value does not need to change, use a symbol instead of a string.
