# C# Coding Guidelines (2025 Edition)

## Overview
The "C# Coding Guidelines" aims to promote readability, consistency, and teamwork in C# code development. This guideline emphasizes the use of the latest C# language features and provides specific guidance on naming conventions for variables and methods, code layout and formatting, recommended coding practices including exception handling and collection handling. It also covers performance considerations and tool utilization such as .editorconfig files and code analysis, concluding that it is important to apply these guidelines flexibly after reaching team consensus.

## Main Topics

- Coding Guidelines
- Naming Conventions
- Layout Formatting
- Code Writing
- Tool Utilization

## C# Coding Guidelines

### 1\. Introduction

Coding conventions are essential for maintaining code readability, consistency, and collaboration within development teams. Following established guidelines makes code easier to understand, maintain, and extend. Even code you write yourself becomes like someone else's code over time, so it's important to follow consistent rules.

This guideline is based on conventions used by Microsoft's documentation team for sample and documentation development, the C# Coding Guidelines 2024 published on Qiita, and Google's C# Style Guide. Rules can be modified to suit team needs.

### 2\. General Policy

Code represents implementation, comments represent intent, commits represent reasons for change, and tickets describe phenomena and problems. Strive for meaningful, understandable, and correct naming to refine your code. Correct naming requires proper class structure.

This guideline emphasizes the following points:

- Use the latest language features and C# versions whenever possible. This allows for more concise and readable code and promotes the introduction of new features.
- Avoid old language constructs.
- Aim to improve code readability and maintainability.

### 3\. Naming Conventions

Naming conventions follow Microsoft's C# naming guidelines.

- Use Pascal Case:
    - Classes, methods, enumerations
    - Public fields and properties
    - Constants (Qiita recommendation, subject to change). Google states that const is not affected by camelCase, but Qiita recommends Pascal Case for everything except private fields.
    - Structures
    - Namespaces and assembly names
    - File names and directory names
    - Primary constructor parameters of record types
    - Control names (Qiita recommendation, subject to change)
- Use camel Case:
    - Parameter variables
    - Local variables
- Add underscores to private fields:
    - private string _userId; (Qiita, Google recommendation)
    - Add s_ to private static fields:
        - private static string s_userId; (Qiita, Google recommendation)
    - Microsoft's documentation team style sometimes doesn't add underscores to private fields. Choose a consistent style for your team. Can be configured in .editorconfig.
- Prefixes and suffixes:
    - Add I- to interfaces.
    - Add -able to interfaces representing functionality.
    - Add -Base to abstract classes.
    - Add -Async to asynchronous methods.
    - Start type parameters with T or T-.
- Method names should be verb phrases.
- Add is-, has-, can- to names representing boolean values. This applies to variable names, property names, and method names.
    - Variable names: bool isItemExist, isChanged, canSave
    - Property names: bool IsRefreshing { get; }
    - Method names: bool IsNullOrEmpty(...)
- Avoid double negatives. Names returning boolean values should be based on positive forms.
- Use plural forms for collection and list variable names.
- Avoid abbreviations. However, they can be considered for local variables with short scope.
- Call static members using the class name ClassName.StaticMember. This makes static access clear and code more understandable.
- Use predefined types (int, string, etc.). It's recommended to use language keywords for data types instead of runtime types (System.Int32, System.String, etc.). Qiita recommends CLR types for member access, but we follow Microsoft's official recommendation.
- Avoid duplication between namespace and class names.
- Use plural forms for enumeration bit fields. Add the [Flags] attribute.

### 4\. Layout and Formatting

Proper layout emphasizes code structure and improves readability.

- Use 4 spaces for indentation. Do not use tabs. Can be configured in .editorconfig.
- Write only one statement per line.
- Write only one declaration per line.
- Break long lines (aim for 65 characters for document readability improvement). Set indentation (4 spaces) for continuation lines.
- Use "Allman" style for braces. Opening and closing braces are on their own new lines. Braces align with the current indentation level.
- Always use braces when necessary. Do not omit them even for single statements in if statements.
- Add at least one blank line between method definitions and property definitions.
- It's recommended to add a blank line at the end of files.
- Use parentheses when creating expressions with clauses.
- Align the direction of comparison operators representing ranges.
- When writing method chains, place periods at the beginning.
- Write auto-implemented properties and empty constructors on one line.
- It's recommended to add commas after the last element when enumerating arrays.
- Specify character encoding and line endings to maintain consistency. Can be configured in .editorconfig.

### 5\. Code Writing (Practices & Idioms)

#### Utilizing New Language Features

Actively use the latest language features.

- Use var actively when the type is obvious from the right side of the assignment. Use when viewers can infer the type from the expression. This can make writing shorter and improve readability. Don't assume the type is obvious from method names. Can be configured in .editorconfig.
- Use collection expressions ([]) to initialize all collection types.
    - string[] vowels = [ "a", "e", "i", "o", "u" ];
    - You can also create empty collections.
- Use Func<> and Action<> to define delegate types. When creating instances of delegate types, use concise syntax.
- Use using statements to make code concise. When the finally block code in try-finally statements is only a call to the Dispose method, use using statements instead. Use the new using syntax that doesn't require braces.
    - using Font normalStyle = new Font("Arial", 10.0f);
- Raw string literals (""" """) are recommended. They are preferred over escape sequences or verbatim strings. Using them for multi-line text or strings containing many escape characters (paths, regular expressions, etc.) significantly improves readability. Can also be used as here documents.
    - var message = """This is a long message...""";
    - var sql = """SELECT u.user_name ...""";
- Use expression-based string interpolation ($"{}"). Recommended for concatenating short strings. Improves readability.
- Use required properties to force property value initialization when creating objects. Can express initialization obligations instead of constructors.
- Actively utilize record types. Declaring with primary constructors automatically implements read-only properties, Constructor, Deconstruct, Clone, ToString, Equals, GetHashCode, etc., reducing boilerplate code. Suitable for immutable data containers. Non-destructive changes are possible with with expressions.
    - public record Person(string FirstName, string LastName);
- Use file-scoped namespaces. Reduces the indentation level of the entire code and improves readability.
    - namespace MySampleCode;
- Consider enabling nullable reference types. Null checks are performed at compile time, which can reduce the possibility of runtime errors. Null check guard clauses may become unnecessary.
    - #nullable enable

#### Exception Handling

- Only catch exceptions that can be properly handled. Avoid catching general exceptions (such as System.Exception) without exception filters.
- Use specific exception types to provide meaningful error messages.
- Use try-catch statements for most exception handling.
- Don't use exceptions for flow control. Use exceptions to indicate unexpected error conditions, not for normal program branching. However, asynchronous cancellation exceptions are an exception to this rule.
- When rethrowing exceptions in catch blocks, use throw; to preserve the original stack trace. Use when passing the cause to the caller as-is. Consider using throw ex; when details are unnecessary or to prevent information leakage, but note that the stack trace will be overwritten.
- Don't catch exceptions caused by coding mistakes like NullReferenceException or IndexOutOfRangeException; strive to fix the code.
- Use catch-all carefully and log whenever possible.

#### Collection Handling

- Use collection expressions ([]) or collection initializers to initialize collections.
- When receiving collections as method arguments, use IEnumerable. This allows accepting various collection types such as arrays and lists.
- When removing items from containers during loops, use the RemoveAll() method or create a new collection with items that won't be removed.
- It's recommended to prioritize List<> for public variables, properties, and return value types. Using IReadOnlyCollection / IReadOnlyList / IEnumerable for inputs can indicate that collections are immutable.
- When declaring Dictionary<TKey, TValue>, it's recommended to leave comments about what the keys and values represent.

#### Control Flow

- Actively use early returns. Guard clauses and such can reduce code nesting and improve readability.
- Using ternary operators for simple conditions improves readability. Don't use for complex conditions.
- Using switch expressions for complex branching improves readability. For more complex cases, use if-else and prioritize readability when deciding syntax. Pattern matching can be utilized.

#### Other

- Don't use magic numbers; prepare explanatory variables or constants. Leaving intent in comments helps with code understanding.
- It's recommended to write increment/decrement operators (++/--) on separate lines rather than using them within expressions. This reduces considerations and avoids misreading.
- Don't reuse local variables unnecessarily and give them accurate names. Also, keep scope small and declare just before use. Avoid unnecessary object creation (new) within loops.
- Clearly distinguish between methods without side effects (pure functions) and methods with side effects. Don't add side effects to names like Get(), Set() that expect lightweight and simple operations.
- Ideally, use properties when they are lightweight and have no side effects. Consider using methods when processing takes time or results change with each call.
- Prioritize declarative descriptions over procedural ones. Using declarative syntax like LINQ, object initializers, switch expressions can make code easier to understand and reduce bugs.
- Consider struct for small, short-lived cases that can be treated as value types (Vector3, Quaternion, etc.). Use class in most cases. Consider struct when performance is critical. readonly struct, in modifier, and record struct can also be utilized.
- When calling delegates, the Delegate?.Invoke() format is recommended. This makes it clear at the call site that it's a delegate call and enables thread-safe null checking.
- Field initializers are recommended.
- Consider using extension methods only when the original class source is unavailable or difficult to change. The functionality to add should be appropriate as a core function of the original class. Use extension methods carefully as they can make code unclear.
- For string concatenation, use string interpolation ($"{}") for short cases, and System.Text.StringBuilder for multiple concatenations. When performance isn't an issue, prioritize the highest readability.
- Object initializers (new SomeClass { ... }) are suitable for 'plain old data' types. It's recommended to avoid them for classes or structs with constructors.

### 6\. Performance Considerations

Premature optimization should be avoided. First write clean and readable code, then identify bottlenecks through measurement before tackling optimization.

- Always use the latest .NET runtime. Runtime evolution improves performance without changing code.
- Don't block the UI thread by using async/await to avoid blocking user operations. Use asynchronous programming for I/O-bound operations (network, database, file access, etc.). Consider using Task.Run() for time-consuming CPU-bound processing.
- Use asynchronous programming with async and await for I/O-bound operations.
- Be careful of deadlocks and use Task.ConfigureAwait when necessary. Especially for ASP.NET Core APIs where you want to avoid context switching overhead, consider setting .ConfigureAwait(false).
- For very performance-critical short asynchronous methods, consider using ValueTask instead of Task to reduce heap allocation.
- Write code to avoid unnecessary object creation (especially new within loops) and unnecessary heap allocation (boxing, excessive string concatenation, etc.). Consider using classes that enable efficient memory management such as System.Text.StringBuilder, ArrayPool<T>, System.IO.Pipelines.
- LINQ has high readability but requires attention to lazy evaluation characteristics and performance degradation from improper use. Use immediate evaluation with ToList(), ToArray() appropriately to prevent unnecessary re-evaluation.
- Exception handling and stack trace acquisition are costly, so prioritize using TryParse() and is/as operators that can avoid exceptions whenever possible.

### 7\. Tool Utilization

Tools help teams apply rules.

- Enable code analysis and apply necessary rules. When rule violations are detected, warnings and diagnostics are generated, and developers can be notified in CI builds.
- Create .editorconfig files to automatically apply style guidelines. Visual Studio can automate code formatting and application of some rules. You can copy existing .editorconfig as a starting point.
- Utilize Visual Studio's code cleanup features to automatically fix code style.
- Introduce analyzers and extensions like ReSharper or StyleCop.Analyzers as needed.

### 8\. Comment Rules

Comments are used to convey intent - not what the code is trying to do, but why it's trying to do it, and how it meets specific design or requirements.

- Use single-line comments (//) for simple explanations.
- Avoid multi-line comments (/* */) and use line comments (//) instead. Block comments can make commit log diffs harder to read.
- Use XML comments to describe methods, classes, fields, and all public members. This allows documentation to be displayed in IntelliSense. It's recommended to add comments to at least public parts.
- Write comments on separate lines, not at the end of code lines.
- Start comment text with a capital letter and end with a period.
- Insert one space between comment delimiters (//) and comment text.
- Don't write comments that just repeat the code content as they become noise.
- Don't write change history comments as they become noise. Check change history with git blame, etc.
- Use tagged comments like TODO, HACK, NOTE to clearly indicate incomplete work or temporary solutions.

### 9\. References

This guideline references the following materials:

- [.NET Coding Conventions - C# | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [C# CODING GUIDELINES 2024 #Programming - Qiita](https://qiita.com/Ted-HM/items/1d4ecdc2a252fe745871)
- [C# at Google Style Guide | styleguide](https://google.github.io/styleguide/csharp-style.html)

These materials provide more detailed information about C# coding styles and practices.

This guideline can be used as a starting point. It's most important to flexibly adjust it according to team circumstances and project characteristics, and apply it after everyone agrees.

### 10\. Audio Commentary

- [C# Coding Guidelines 2025 Audio Commentary](Contents/CSharpCodingGuildline.Explanation.mp3)
- [C# Coding Guidelines 2025 Audio Commentary Transcript](Contents/CSharpCodingGuildline.Explanation.English.txt)