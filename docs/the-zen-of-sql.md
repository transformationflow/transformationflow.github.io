---
hide:
  - navigation
---
# The Zen of SQL
## Introduction
Inspired by the Python equivalent ([PEP20](https://www.python.org/dev/peps/pep-0020/) - The Zen of Python), and also using it as a starting point, the TF-SQL10[^1] are the core guiding principles which are central to writing clean, clear, well-structured and easy to maintain data transformations in SQL:

## TF-SQL10: SQL 10 Commandments
> - Readability counts.
> - Explicit is better than implicit.
> - Simple is better than complex.
> - Complex is better than complicated.
> - Flat is better than nested.
> - Errors should never pass silently.
> - Unless explicitly silenced.
> - Predictable is better than flexible.
> - Modular is better than monolithic.
> - Common Table Expressions are a seriously great idea - let's do more of those!

## Discussion

!!! example "01: Readability counts"
    Your code will be (hopefully) written once and potentially read many times, sometimes by you and probably also by others too.  Do your future self (and anybody who takes over your code base) a favour and make your code clear and easy to understand.  This means naming columns, variables and common table expressions in a clear, consistent and easy-to-understand manner, as well as structuring the code in a simple, sequential style to facilitate humans reading from top to bottom.

    An extension of this is also making sure that your code is readable by machines, so structuring e.g. comments and/or docstrings so that they can be reliably parsed and used.

!!! example "02: Explicit is better than implicit."
    Discussion



[^1]: Whilst the Python PEP20 actually only contains 19 aphorisms, the Transformation Flow SQL 10 (TF-SQL10) contains exactly 10 for the purposes of data integrity and predictability. 


