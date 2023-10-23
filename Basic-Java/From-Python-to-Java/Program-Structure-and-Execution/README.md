# Programs and Their Execution

| **Python** | **Java** |
| --- |  --- |
| Programs consist of modules.  Modules in turn can contain statements, function definitions, and/or class definitions.  Each module is associated with a source file (**.py** extension) and possibly a byte code file (**.pyc** extension).An interpreter called a Python virtual machine (PVM) translates Python source files to byte code during execution.  The PVM may save the corresponding byte code in files for subsequent executions. | Programs consist of interfaces and classes.  Interfaces and classes are contained in source files (**.java** extension).  A source file compiles into one or more executable byte code files (**.class** extension).Classes and interfaces can be part of a package.  A package is a bit like a module, from which any resources can be imported.  The byte code files for a package are usually contained in a directory whose name is the package name. Programs must first be compiled before they can be executed.  Programs are either **applets**（小程序） or applications.  Applets are programs that run in a Web browser.  Applications are programs that run on a standalone computer.  **In either case**（无论哪种情况）, Java programs execute in an interpreter called a Java virtual machine (JVM). |

# Running a Program

| **Python** | **Java** |
| --- |  --- |
| A Python program can be run as a script from a command prompt, as follows:| A Java application must first be compiled, as follows:|
**\> python helloworld.py**|**\> javac HelloWorld.java**|
||The byte code file can then be run as follows:
||**\> java HelloWorld**|
|Syntax and type errors are caught at run time.|Syntax and type errors are caught at compile time.  All other errors are caught at run time.

# A First Program: Hello World

| **Python** | **Java** |
| --- |  --- |
| The simplest version of this program consists of a single statement:<br>**print "Hello world!"**<br>The PVM simply executes this statement.Another version embeds this statement in a **main** function which is called at the end of the module:<br><br>**def main():**<br>    **print "Hello world!"**<br>**main()**<br><br>The PVM executes the function definition and then calls the function. | The simplest version of this program defines a **main** method within a class:<br><br>**public class HelloWorld{**<br>    **static public void main(String\[\] args){**<br>        **System.out.println("Hello world!");**<br>    **}**<br>**}**<br>The source code defines but does not call this method.  At program startup, the compiled class **HelloWorld** is loaded into the JVM.  The JVM then calls **main**, which is a class method in **HelloWorld**.A Java application must include at least one class that defines a **main** method.  The byte code file for this class is the entry point for program execution. |

# Lexical（词法的） Structure

| **Python** | **Java** |
| --- |  --- |
| Literals include numbers, strings, tuples, lists, and dictionaries.Identifiers include the names of variables, classes, functions, and methods.<br>Reserved words include those of the major control statements (**if**, **while**, **for**, **import**, etc.), operators (**in**, **is**, etc.), definitions (**def**, **class**, etc.) and special values (**True**, **False**, **None**, etc.). | Literals include numbers, characters, and strings.<br><br>Identifiers include the names of variables, classes, interfaces, and methods.<br><br>Reserved words include those of the major control statements (**if**, **while**, **for**, **import**, etc.), operators (**instanceof**, **throw**, etc.), definitions (**public**, **class**, etc.), special values (**true**, **false**, **null**, **this**, **super** etc.), and standard type names (**int**, **double**, **String**, etc.). |

# Syntactical Structure

| **Python** | **Java** |
| --- |  --- |
| Lexical items（词汇项） on a single line are separated by zero or more spaces.
Indentation is significant and is used to mark syntactical structures, such as statement blocks and code within function, class, and method definitions.
A phrase can be broken and continued on the next line after a comma, or by using the **'\\'** symbol.
The headers of control statements and function, class, and method definitions end with a colon (**:**). |Indentation is not significant, so all lexical items are separated by zero or more spaces.Blocks of code in statements and definitions are enclosed in curly braces (**{}**).Simple statements end with a semicolon (**;**).Boolean expressions in loops and **if** statements are enclosed in parentheses.   |
