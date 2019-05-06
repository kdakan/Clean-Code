# Clean Code

- [ 1. Clean code principles](#1-clean-code-principles)
- [ 2. Reasons for clean code](#2-reasons-for-clean-code)
- [ 3. Attributes of clean code](#3-attributes-of-clean-code)
- [ 4. Code duplication and DRY](#4-code-duplication-and-dry)
- [ 5. Exceptions to DRY](#5-exceptions-to-dry)
- [ 6. Names](#6-names)
- [ 7. Methods](#7-methods)
- [ 8. Comments](#8-comments)
- [ 9. Error handling](#9-error-handling)
- [10. Code metrics](#10-code-metrics)

## 1. Clean code principles:
* You should use the right tool for the job (tech boundaries matter, do not mix code between HTML, Javascript, CSS, C# backend, SQL database, they have their own strong use cases and there are clear ways of communication between them, use SQL and DB parameters to talk to DB, run security, validation and business logic in the backend and use HTTP to talk to it, use JSON and cookies to respond to javascript frontend, use CSS to style HTML, etc.)
* Code should have a high signal to noise ratio (TED=terse, expressive, do one thing, DRY=don't repeat yourself, similar to DB normalization)
* Code should be self-documenting (clear intent, use proper layers of abstraction)

## 2. Reasons for clean code:
* Most of the time and resources are spent during the maintenance phase in the lifecycle of a software project
* Each line of code is read by 10 different developers over time
* Code is maintained by 10 different developers over time because of the developer turnover rate
* Code that is not clean, becomes unmaintainable over time after it grows, and finally has to be discarded

## 3. Attributes of clean code:
* Readable, expressive, reveal intent and discoverable
* Maintainable, easy to add, change and extend
* Modular, having small units of single responsibility
* Simple (avoid accidental complexity)
* DRY (little or no code duplication)
* Easy to use and unit test, have minimal dependencies

## 4. Code duplication and DRY:
DRY = Don't Repeat Yourself (single source of truth)

Code duplication can be eliminated by:
* Using code analyzer tools
* Code reviews
* Team culture
* Good documentation
* Planned architecture

## 5. Exceptions to DRY:
* Performance/security critical code (like similar functionality coded in a more performant or secure way)
* Loosely coupling of layers, modules, or subsystems (like similar classes for UI view models, DTOs, and domain models, similar code for each module or microservice, etc.)
* To increase readability and reducing coupling between automated test methods (ideally automated test methods should be isolated from each other)
* Auto-generated code because of platform limitations, using RAD techniques, or building proof of concept code or prototypes

## 6. Names:
Names should:
* Reveal intent
* Be pronounceable
* Be clear and unambiguous
* Not have member prefix or postfix, like Hungarian notation to reveal type, _ or m_ for private members, or I for interface names, or Impl for concrete class names
* Searchable (not too short or too generic)
* Be nouns for a class and objects
* Be verbs for methods and delegates (lambda functions)
* Be consistent (use either manager, controller or driver but not all for the same purpose)
* Be picked from the business problem domain (Client, user, account, payment, etc.) and the solution domain (computer science terms, algorithm names, design pattern names)
* Larger scoped classes and methods (like public APIs, public or protected member methods) should have a shorter name
* Smaller scoped classes and methods (like private classes or private methods) should have a longer name
* Larger scoped variable members (like global variables, constants, public or protected properties) should have a longer name
* Smaller scoped variable members (like private member variables or local variables) should have a shorter name
* Method arguments, exception instances, and counter variables should have shorter names
* Boolean variables and members should start with is or be a past or simple present tense verb (like isDone, isLoggedIn, done, loggedIn, doesSomething, Validates(), etc.)

## 7. Methods:
Methods should:
* Be small
* Have single responsibility
* Be testable
* Be reusable and DRY
* Not have more than 3 levels of nesting in blocks
* Have single line in its conditional blocks (extract the block into a separate method if it is more than one line)
* Should not have side-effects other than its name implies (complicates the code)
* Apply command-query separation, should either return a value or change some state but not both (except for Try...() methods or when a command and a query has to be atomic like a Login() method, or for performance reasons like a DB Insert() method which will also retrieves the Id and foreign key Ids on the object after inserting an object graph)
* Throw exception instead of return error result (to eliminate deeply nested blocks)
* Extract long conditions (having multiple and/or operators) into a separate method that returns the bool result of this long condition
* Not have more than 3 parameters
* (You can grab the outside code getting or calculating a parameter and move it inside the method being called, and remove this method "parameter object")
* (You can also extract some of the parameters into a "parameter object", then even move the method and turn it into a member method of this object, thus remove those parameters)
* Not have boolean flag parameters (extract to 2 different methods for each case of the flag)
* Not pass null as parameters when calling other methods (else every methods will have to check if their arguments are null each time they are called)
* Not return null, but instead return a Maybe<T> result, or an empty collection, or throw an exception when appropriate (else every method result will have to be checked if it is null each time it is called, somebody will forget this check and try to use the null result)
* Prefer not using output parameters, instead return it as a result
* Have a single return (but early returns for defensive coding is good, or multiple returns in very short methods is OK)
* Fail fast (return early)
* Not use goto
* Not have multiple returns/breaks inside loops
* Not have a large number of cases in switch statements (don't tell, rather ask, use polymorphism for case code blocks or hashtable/dictionary for case return values)
* Not have temporal coupling (like Open...() some code block... Close...(), instead either use a method that accepts the code block as a lambda Action<T>, or use the .Net using (...) block Disposable pattern, or use template method pattern)
* Not have very long lines that need horizontal scrolling
* Not have #region blocks, this is a sign of a long method or a large class
* Not use magic numbers or magic strings, instead use enums or constants (prefer static read-only fields in C# instead of const, because const is statically linked at compile time and may break xcopy deployment of library code)

## 8. Comments:
* Are usually unnecessary when the code is clean and understandable
* TODO comments are OK
* Legal copyright/license comments are OK
* Informative complementary comments (like explaining the required date format on an abstract method argument, or reason behind an implementation decision, etc.) which cannot be expressed in code is OK
* Code should be deleted instead of commented out
* Public classes/members can have documentation comments (java docs comments) but private classes/members should not

## 9. Error handling:
* Exceptions should be used instead of return values from methods (to prevent deeply nested code blocks)
* Errors should be handled where they occur
* Specific exceptions with informative description messages should be used instead of generic exceptions
* Should not catch, or at least throw when we are unable to handle the exception type, so that call stack trace is not lost
* When we need to catch multiple exceptions in the same try/catch block, we can extend these exception types from a base exception type and catch the base exception type instead
* Errors should be logged
* Do not pass null to a method and do not return null (see methods section)
* If method depends on parameters passed from the uncontrolled outside environment, use defensive coding and check arguments first

## 10. Code metrics:
Code metrics that signal bad code quality, and should constantly be tracked:
* Cyclomatic complexity (measures independent linear code paths/branches), >10 is bad
* Lack of cohesion (cohesion measures correlation between methods and field members of a class, if a lot of methods do not use a lot of field members in a class, it lacks cohesion and should be divided into more classes)
* Coupling between classes (if a class definition and its methods use too many other classes, it is highly coupled, hard to use and test, and should be broken down into more classes and should also use abstractions like interfaces instead)
* Code coverage measures how much of the code (ratio of lines of code or branches) are covered (executed by) unit tests, however high coverage does not automatically mean the tests are effective, test input data and assertions are effectively important in tests
* Unit test pass percentage, when low, can indicate either the code has bugs or the tests are not isolated, they are brittle or obsolete
* Duplicate code percentage measures DRYness of code
* Technical debt measures the development time needed to maintain and extend the codebase, is a sign of taking shortcuts instead of applying the best solution
