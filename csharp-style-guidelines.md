## Comments
* Comments must be written in English
* Avoid commenting the obvious such as 
```
var c = a + b; // Add a to b
```
* Use XML comments on classes that are meant to be consumed by other codebases. Examples: custom frameworks, common libraries, etc... Classes that are never meant to be used outside of a given solution do not need XML comments unless they add needed clarity.
* Attempt to make code self descriptive so that comments are not needed.
* Comments should be written with a professional tone. Avoid joking, cursing, or finger pointing. Assume the code will be audited by a 3rd party or shown to an executive.

## Naming Conventions
The following naming conventions should be adhered to on all new codebases. Try to adhere to the current style of an existing codebase if converting the entire codebase is not practical.

* Use camelCasing when naming private fields, and local variables.
* Use PascalCasing when naming classes, structs, enums, methods, events, delegates, property names, public fields, and constants.  
* Prefix private fields with a "\_"
* Use unabbreviated meaningful names. Exception: single letter variable names for loop counters and lambda parameters are OK.
* Err or the side of names being too long rather than too short.
* Abbreviations of two letters in length should have both letters capitalized. example: ```var userID = 1;```
* Abbreviations of three or more letters should only have the the first letter capitalized. example: ```var expectedDob = DateTime.Now().AddMonths(9); ```
* Interfaces should always be named starting with a capital "I" followed by a capital letter. Example ```public interface IEntity { }```

## General Coding Conventions
* Avoid files for "global" consts and enums. Try to hang constants and enums off of a relevant class. If this is not possible, create separate files per logical grouping. Example: ConsumerConstants, ProductConstants, etc...
* Always use system aliases for "primitive" types. Example: use "string" instead of "String".
* Prefer collections over arrays, unless there is a specific performance problem.
* Curly braces should always be on the next line, except within razor view templates, auto properties, empty constructors, or one-liner lambdas.
* Always use braces with if statements.
* Use the var keyword whenever possible to reduce "type noise" and to reduce the impact of refactoring. It is OK to specify the type if you think the code needs it to avoid a misunderstanding.
* Do not use deprecated methods and classes and/or deprecated libraries.
* Declare variables within the scope they are used and directly prior to their use.
* "public const" should only be used for things that will never change (such as the temperature water boils at). Use "public static readonly" when this is not the case. const may be used freely with private and internal values (as long as internals aren't made visible to another assembly that isn't a test assembly).
* Do not use "Yoda Conditions".  



Example: 

	// Good
	if (foo == null) { }
	// Bad Yoda
	if (null == foo) { }

* Abstract I/O and third party calls unless it has been deemed ok otherwise.
* Use Entity Framework for DB access unless otherwise approved.
* Always dispose of objects marked as IDisposable. Scoping an IDisposable within a using block is a good idea unless the IDisposable's lifetime is managed externally, such as via an IoC container.
	* Caveat: [Do not use using blocks with WCF proxies](http://msdn.microsoft.com/en-us/library/aa355056.aspx). 
	* Caveat: You do not need to manually dispose of objects that have their lifetimes managed by an IoC container.
* Classes, except data models, should have a logical layout given by the following. When it improves readability or understanding, alternate layouts can be used but the deviation should be documented and justified.
	1. Consts and fields ordered by access level*.
	2. Nested classes/enums ordered by access level.
	3. Properties ordered by access level.
	4. Constructors ordered by access level, then by parameter count.
	5. Methods ordered by access level, then by name, then by parameter count.
* Data model classes, DB entities, viewmodels, etc.,  typically only define a single default constructor, public properties, and, potentially, public and private validation methods. These use an alternate layout:
	1. Default constructor - if your class includes collections of related data entities, you consider initializing these to empty lists in the constructor to avoid complicated access logic.
	2. Entity properties, including any foreign keys if exposed, ordered by access level.  Typically value types and strings.
	3. Relations - typically references to other data model classes
	4. Methods ordered by access level, then by name, then by parameter count.
* Be mindful about access modifiers at the class and member level. Do not expose classes or members to the outside world unless you intend to.
* Do not use regions (except in unit test classes where you want to group tests).
* There should only be one class per file except for when a class is nested.

\* Access level ordering should always be: private, protected, internal, then public.

Examples:

    public class SomeClass
    {
        const int MonthsInYear = 12;
        int _somePrivateField;
        int SomePublicField;
        public const string SquigglyLine = "~"; 
        
        public enum Status
        {
            Started,
            Completed
        }
        
        public string SomeProperty { get; set; }
        
        public SomeClass() 
        {
            
        }
        
        public string SomeMethod()
        {
            var someLocalVariable = "foo";
            return foo;			
        }
    }
		
* Try to keep classes under 1000 lines
* Try to keep methods under 100 lines
* Try to keep the width of your code narrow enough to avoid the need to scroll horizontally on a typical setup (16x10 with explorer panes open on both sides). 

## Error Handling
* Always use a top-level handler to catch and log unhandled exceptions at the application seam (in the global.asax.cs, in the main method, etc...)
* Do not catch exceptions in application code unless you plan to do something about them. Defer catastrophic error handling and logging to the top level handler.
* Log exceptions to log files or the event log. When in Azure log to Azure diagnostics + blob storage instead if possible.

## Builds and Configuration
* Set builds to treat warnings as errors.
* Do not hard code values that would benefit from being in configuration.
* Do not store sensitive information in clear text in config files. Prefer retrieving such values out of a config database or service. If the values must be in a config file, consider encryption.
* Never store production keys in source control. Use a build-time or deploy-time transform, config db, or service that limited people have access to.
* Do not check in code that breaks the build. Use stash, shelve, or an unstable branch when you need to check in non-building code.
