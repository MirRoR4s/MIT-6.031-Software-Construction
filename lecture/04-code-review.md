# [Reading 4: Code Review](https://web.mit.edu/6.031/www/sp21/classes/04-code-review/)

**Software in 6.031**

| Safe from bugs                                   | Easy to understand                                           | Ready for change                                  |
| :----------------------------------------------- | :----------------------------------------------------------- | :------------------------------------------------ |
| Correct today and correct in the unknown future. | Communicating clearly with future programmers, including future you. | Designed to accommodate change without rewriting. |

**Objectives**

In today’s class, we will practice:

- code review: reading and discussing code written by somebody else
- general principles of good coding: things you can look for in every code review, regardless of programming language or program purpose

---

## Code review（代码审查）

Code review is careful, systematic study of source code by people who are not the original author of the code. It’s analogous to proofreading a paper.

Code review really has two purposes:

- **Improving the code.** Finding bugs, anticipating possible bugs, checking the clarity of the code, and checking for consistency with the project’s style standards.
- **Improving the programmer.** Code review is an important way that programmers learn and teach each other, about new language features, changes in the design of the project or its coding standards, and new techniques. In open source projects, particularly, much conversation happens in the context of code reviews.

Code review is widely practiced in open source projects like Apache and [Mozilla](http://blog.humphd.org/vocamus-1569/?p=1569). Code review is also widely practiced in industry. In [Google’s code review process](https://google.github.io/eng-practices/review/), for example, you can’t push any code into the main repository until another engineer has signed off on it in a code review.

Why is code review widely used? There are many ideas out there for software development processes (some with buzzwords like Agile and Scrum). But code review is a practice with good evidence that it actually works. [Hillel Wayne](https://twitter.com/hillelogram/status/1120495752969641986) has collected some of the research highlights, among them that code review can find 70-90% of software defects, and that overwhelming numbers of software engineers at Google and Microsoft find it valuable and worth doing.

In 6.031, we’ll do code review on problem sets, as described in the [Code Reviewing document](https://web.mit.edu/6.031/www/sp21/general/code-review.html) on the course website.

### Style standards

Most companies and large projects have coding style standards. These can get pretty detailed, even to the point of specifying whitespace (how deep to indent) and where curly braces and parentheses should go. These kinds of questions often lead to [holy wars](http://www.outpost9.com/reference/jargon/jargon_23.html#TAG897) since they end up being a matter of taste and style.

In 6.031, we have no official style guide. If you are new to Java and want a recommendation, [Google Java Style](http://google.github.io/styleguide/javaguide.html) is widely used and appealing for its readability, particularly rules like these:

```java
if (isOdd(n)) {
    n = 3*n + 1;
}
```

- space after keyword (`if`), but not after function name (`isOdd`)
- `{` at the end of a line, `}` on a line by itself
- `{`…`}` around all blocks, even empty or one-statement blocks

Eclipse has an autoformatter (*Source* → *Format*) whose default rules are similar to Google style.

But we won’t dictate where to put your curly braces in this class. That’s a personal decision that each programmer should make. It’s important to be self-consistent, however, and it’s *very* important to follow the conventions of the project you’re working on. If you’re the programmer who reformats every module you touch to match your personal style, your teammates will hate you, and rightly so. Be a team player.

But there are some rules that are quite sensible and target our big three properties, in a stronger way than placing curly braces. The rest of this reading talks about some of these rules, at least the ones that are relevant at this point in the course, where we’re mostly talking about writing basic Java. These are some things you should start to look for when you’re code reviewing other students, and when you’re looking at your own code for improvement. Don’t consider it an exhaustive list of code style guidelines, however. Over the course of the semester, we’ll talk about a lot more things — specifications, abstract data types with representation invariants, concurrency and thread safety — which will then become fodder for code review.

### Smelly example #1

Programmers often describe bad code as having a “bad smell” that needs to be removed. “Code hygiene” is another word for this. Let’s start with some smelly code.

```java
public static int dayOfYear(int month, int dayOfMonth, int year) {
    if (month == 2) {
        dayOfMonth += 31;
    } else if (month == 3) {
        dayOfMonth += 59;
    } else if (month == 4) {
        dayOfMonth += 90;
    } else if (month == 5) {
        dayOfMonth += 31 + 28 + 31 + 30;
    } else if (month == 6) {
        dayOfMonth += 31 + 28 + 31 + 30 + 31;
    } else if (month == 7) {
        dayOfMonth += 31 + 28 + 31 + 30 + 31 + 30;
    } else if (month == 8) {
        dayOfMonth += 31 + 28 + 31 + 30 + 31 + 30 + 31;
    } else if (month == 9) {
        dayOfMonth += 31 + 28 + 31 + 30 + 31 + 30 + 31 + 31;
    } else if (month == 10) {
        dayOfMonth += 31 + 28 + 31 + 30 + 31 + 30 + 31 + 31 + 30;
    } else if (month == 11) {
        dayOfMonth += 31 + 28 + 31 + 30 + 31 + 30 + 31 + 31 + 30 + 31;
    } else if (month == 12) {
        dayOfMonth += 31 + 28 + 31 + 30 + 31 + 30 + 31 + 31 + 30 + 31 + 31;
    }
    return dayOfMonth;
}
```

The next few sections and exercises will pick out the particular smells in this code example.

### Don’t repeat yourself (DRY)

Duplicated code is a risk to safety. If you have identical or very similar code in two places, then the fundamental risk is that there’s a bug in both copies, and some maintainer fixes the bug in one place but not the other.

Avoid duplication like you’d avoid crossing the street without looking. Copy-and-paste is an enormously tempting programming tool, and you should feel a frisson of danger run down your spine every time you use it. The longer the block you’re copying, the riskier it is.

[Don’t Repeat Yourself](http://en.wikipedia.org/wiki/Don't_repeat_yourself), or DRY for short, has become a programmer’s mantra.

The `dayOfYear` example is full of identical code. How would you DRY it out?

---

### Comments where needed

Good software developers write comments in their code, and do it judiciously. Good comments should make the code easier to understand, safer from bugs (because important assumptions have been documented), and readier for change.

One kind of crucial comment is a specification, which appears above a method or above a class and documents the behavior of the method or class. In Java, this is conventionally written as a Javadoc comment, meaning that it starts with `/**` and includes `@`-syntax, like `@param` and `@return` for methods. Here’s an example of a spec:

```java
/**
 * Compute the hailstone sequence.
 * See http://en.wikipedia.org/wiki/Collatz_conjecture#Statement_of_the_problem
 * @param n starting number of sequence; requires n > 0.
 * @return the hailstone sequence starting at n and ending with 1.
 *         For example, hailstone(3)=[3,10,5,16,8,4,2,1].
 */
public static List<Integer> hailstoneSequence(int n) {
    ...
}
```

Specifications document assumptions. We’ve already mentioned specs a few times, and there will be much more to say about them in a future reading.

Another crucial comment is one that specifies the provenance or source of a piece of code that was copied or adapted from elsewhere. This is vitally important for practicing software developers, and is required by the [6.031 collaboration policy](https://web.mit.edu/6.031/www/sp21/general/collaboration.html) when you adapt code you found on the web. Here is an example:

```java
// read a web page into a string
// see http://stackoverflow.com/questions/4328711/read-url-to-string-in-few-lines-of-java-code
String mitHomepage = new Scanner(new URL("http://www.mit.edu").openStream(), "UTF-8").useDelimiter("\\A").next();
```

One reason for documenting sources is to avoid violations of copyright. Small snippets of code on Stack Overflow are typically in the public domain, but code copied from other sources may be proprietary or covered by other kinds of open source licenses, which are more restrictive. Another reason for documenting sources is that the code can fall out of date; the [Stack Overflow answer](http://stackoverflow.com/questions/4328711/read-url-to-string-in-few-lines-of-java-code) from which this code came has evolved significantly in the years since it was first answered.

Some comments are bad and unnecessary. Direct transliterations of code into English, for example, do nothing to improve understanding, because you should assume that your reader at least knows Java:

```java
while (n != 1) { // test whether n is 1   (don't write comments like this!)
   ++i; // increment i
   l.add(n); // add n to l
}
```

But obscure code should get a comment:

```java
int sum = n*(n+1)/2;  // Gauss's formula for the sum of 1...n

// here we're using the sin x ~= x approximation, which works for very small x
double moonDiameterInMeters = moonDistanceInMeters * apparentAngleInRadians;
```

[The `dayOfYear` code](https://web.mit.edu/6.031/www/sp21/classes/04-code-review/#dayOfYear) needs some comments — where would you put them? For example, where would you document whether `month` runs from 0 to 11 or from 1 to 12?

#### READING EXERCISES

---

### Fail fast

*Failing fast* means that code should reveal its bugs as early as possible. The earlier a problem is observed (the closer to its cause), the easier it is to find and fix. As we saw in the [first reading](https://web.mit.edu/6.031/www/sp21/classes/01-static-checking/#static_checking_dynamic_checking_no_checking), static checking fails faster than dynamic checking, and dynamic checking fails faster than producing a wrong answer that may corrupt subsequent computation.

The `dayOfYear` function doesn’t fail fast — if you pass it the arguments in the wrong order, it will quietly return the wrong answer. In fact, the way `dayOfYear` is designed, it’s highly likely that a non-American will pass the arguments in the wrong order! It needs more checking — either static checking or dynamic checking.

#### READING EXERCISES

****



### Avoid magic numbers

There’s a computer science joke that the only numbers that computer scientists understand are 0, 1, and sometimes 2.

All other constants are called [magic](https://en.wikipedia.org/wiki/Magic_number_(programming)#Unnamed_numerical_constants) because they appear as if out of thin air with no explanation.

One way to explain a number is with a comment, but a far better way is to declare the number as a named constant with a good, clear name.

`dayOfYear` is full of magic numbers, which illustrate some of the reasons they should be avoided:

- **A number is less readable than a name.** In `dayOfYear`, the month values 2, …, 12 would be far more readable as `FEBRUARY`, …, `DECEMBER`.
- **Constants may need to change in the future.** Using a named constant, instead of repeating the literal number in various places, is more ready for change.
- **Constants may be dependent on other constants.** In `dayOfYear`, the mysterious numbers 59 and 90 are particularly pernicious examples. Not only are they uncommented and undocumented, they are actually the result of a *computation done by hand* by the programmer – summing the lengths of certain months. This is less easy to understand, less ready for change, and definitely less safe from bugs. Don’t hardcode numbers that you’ve computed by hand. Use a named constant that visibly computes the relationship in terms of other named constants.

What about constants that seem constant and eternal, like π, or the total angle of a triangle, or the gravitational constant `G`? Is it necessary to give names to fundamental constants of mathematics and physics that don’t depend on other constants? First, the number itself may be complex to express, so repeating it multiple times is not safe from bugs. Is it easy to tell which is right: `3.14159265358979323846` or `3.1415926538979323846`? Second, even these numbers encode design decisions that might change in the future, such as the number of digits of precision, or the units of measurement. A named constant is more ready for change when those design decisions change.

If you have a profusion of magic numbers in your code, it can be a sign that you need to take a step back and treat those numbers *as data* rather than named constants, and put them into a data structure that allows for simpler code. In `dayOfYear`, the lengths of the months (30, 31, 28, etc.) would be more readable, and far more DRY, if they were instead stored in a data structure like an array, list, or map, e.g. `MONTH_LENGTH[month]`.

#### READING EXERCISES

---

### One purpose for each variable

In the `dayOfYear` example, the parameter `dayOfMonth` is reused to compute a very different value — the return value of the function, which is not the day of the month.

Don’t reuse parameters, and don’t reuse variables. Variables are not a scarce resource in programming. Introduce them freely, give them good names, and just stop using them when you stop needing them. You will confuse your reader if a variable that used to mean one thing suddenly starts meaning something different a few lines down.

Not only is this an ease-of-understanding question, but it’s also a safety-from-bugs and ready-for-change question.

Method parameters, in particular, should generally be left unmodified. (This is important for being ready-for-change — in the future, some other part of the method may want to know what the original parameters of the method were, so you shouldn’t blow them away while you’re computing.) It’s a good idea to use `final` for method parameters, and as many other variables as you can. The `final` keyword says that the variable should never be reassigned, and the Java compiler will check it statically. For example:

```java
public static int dayOfYear(final int month, final int dayOfMonth, final int year) {
    ...
}
```

### Smelly example #2

There was a latent bug in `dayOfYear`. It didn’t handle leap years at all. As part of fixing that, suppose we write a leap-year method.

```java
public static boolean leap(int y) {
    String tmp = String.valueOf(y);
    if (tmp.charAt(2) == '1' || tmp.charAt(2) == '3' || tmp.charAt(2) == 5 || tmp.charAt(2) == '7' || tmp.charAt(2) == '9') {
        if (tmp.charAt(3)=='2'||tmp.charAt(3)=='6') return true; /*R1*/
        else
            return false; /*R2*/
    }else{
        if (tmp.charAt(2) == '0' && tmp.charAt(3) == '0') {
            return false; /*R3*/
        }
        if (tmp.charAt(3)=='0'||tmp.charAt(3)=='4'||tmp.charAt(3)=='8')return true; /*R4*/
    }
    return false; /*R5*/
}
```

What are the bugs hidden in this code? And what style problems that we’ve already talked about?

#### READING EXERCISES

---

### Use good names

Good method and variable names are long and self-descriptive. Comments can often be avoided entirely by making the code itself more readable, with better names that describe the methods and variables.

For example, you can rewrite

```java
int tmp = 86400;  // tmp is the number of seconds in a day (don't do this!)
```

as:

```java
final int secondsPerDay = 86400;
```

In general, variable names like `tmp`, `temp`, and `data` are awful, symptoms of extreme programmer laziness. Every local variable is temporary, and every variable is data, so those names are generally meaningless. Better to use a longer, more descriptive name, so that your code reads clearly all by itself.

Follow the lexical naming conventions of the language. In both Python and Java, classes typically start with a capital letter, and variable names and method names start with a lowercase letter. But the two languages differ in how multi-word phrases are rendered into a method or variable name. Python uses snake_case (underscores separating the words of the phrase), while Java uses camelCase (capitalizing each word after the first, as in `startsWith` or `getFirstName`).

Java also uses capitalization to distinguish global constants (`public static final`) from variables and local constants. `ALL_CAPS_WITH_UNDERSCORES` is used for `static final` constants. But the local variables declared inside a method, including local constants like `secondsPerDay` above, use camelCaseNames.

In any language, method names are usually verb phrases, like `getDate` or `isUpperCase`, while variable and class names are usually noun phrases. Choose short words, and be concise, but avoid abbreviations. For example, `message` is clearer than `msg`, and `word` is so much better than `wd`. Keep in mind that many of your teammates in class and in the real world will not be native English speakers, and abbreviations can be even harder for non-native speakers.

Avoid single-character variable names entirely except where they are easily understood by convention. For example, `x` and `y` make sense for Cartesian coordinates, and `i` and `j` as integer variables in `for` loops. But if your code is full of variables like `e`, `f`, `g`, and `h`, because you’re just picking them from the alphabet, then it will be incredibly hard to read.

[Effectively Naming Software Thingies](https://medium.com/@rabinovichsagi/effectively-naming-software-thingies-fcea9d78a699) has some excellent advice about naming. Robert Martin’s [*Clean Code*](https://lib.mit.edu/record/cat00916a/mit.001511591) (chapter 2) is also highly recommended.

The `leap` method has bad names: the method name itself, and the local variable name. What would you call them instead?

#### READING EXERCISES

##### Better method names

```java
public static boolean leap(int y) {
    String tmp = String.valueOf(y);
    if (tmp.charAt(2) == '1' || tmp.charAt(2) == '3' || tmp.charAt(2) == 5 || tmp.charAt(2) == '7' || tmp.charAt(2) == '9') {
        if (tmp.charAt(3)=='2'||tmp.charAt(3)=='6') return true;
        else
            return false;
    }else{
        if (tmp.charAt(2) == '0' && tmp.charAt(3) == '0') {
            return false;
        }
        if (tmp.charAt(3)=='0'||tmp.charAt(3)=='4'||tmp.charAt(3)=='8')return true;
    }
    return false;
}
```

Which of the following are good names for the `leap()` method?

1. leap
2. isLeapYear
3. IsLeapYear
4. is_divisible_by_4

> `IsLeapYear` might be a fine name in another programming language, but in Java convention, only classes are Capitalized.
>
> `is_divisible_by_4` not only violates the Java convention (camelCase), but is also too low-level, and in fact inaccurate, since leap year computation is more complicated than divisibility by 4.

##### Better variable names

Which of the following are good names for the `tmp` variable inside `leap()`?

1. leapYearString
2. yearString
3. temp
4. secondsPerDay
5. s

> Out of these choices, `yearString` is the best.
>
> `leapYearString` is inaccurate, because the year may not be a leap year after all. It’s premature to call it one.
>
> `temp` is just as bad as tmp.
>
> `secondsPerDay` is completely inaccurate. Copy-and-pasting code can often produce out-of-context names like this, which is another reason to avoid copy and paste.
>
> `s` is a popular name for a string variable when it really is generic, any string, with no more specific meaning than that. It’s a poor choice here because this string is related to the year variable, so it’s more specific and deserves a specific name.

##### Skip the comment

Rename this variable so that the comment can be deleted entirely:

```java
int a = 18;  // this variable stores the age when you can first vote
```

Rename `a` to:

`minimumVotingAge`, `ageCanVote`, `votingAge`, ...

> `age` would not have all the information in the comment. `storesTheAgeWhenYouCanFirstVote` is definitely wordier than it needs to be.

##### Types

Good names imply something about their type – what kinds of values they can represent. Which of these names say something to the reader about their type?

1. b
2. **bookTitle**
3. **date**
4. **getAge()**
5. previous
6. tmp
7. **widthInPixels**

> `b` and `previous` say nothing outside of context, and `tmp` is a notable offender of this guideline. The others constrain possible values even though they may not say specifically whether, say, `widthInPixels` is an `int` or `float`.
>
> Using names that suggest their types can be particularly good for the readability of dynamically-typed languages like Python and JavaScript, because they don’t have static types in variable declarations.
>
> A programming practice that takes this idea to an extreme is [Hungarian notation](https://en.wikipedia.org/wiki/Hungarian_notation). Hungarian notation encodes the type into a highly-abbreviated prefix, such as `arru8NumberList` for an array of 8-bit unsigned ints, so there is a serious cost in readability.

### Use whitespace to help the reader

Use consistent indentation. The [`leap`](https://web.mit.edu/6.031/www/sp21/classes/04-code-review/#leap) example is bad at this. The [`dayOfYear`](https://web.mit.edu/6.031/www/sp21/classes/04-code-review/#dayOfYear) example is much better. In fact, `dayOfYear` nicely lines up all the numbers into columns, making them easy for a human reader to compare and check. That’s a great use of whitespace.

Put spaces within code lines to make them easy to read. The leap example has some lines that are packed together — put in some spaces.

Never use tab characters for indentation, only space characters. Note that we say *characters*, not keys. We’re not saying you should never press the Tab key, only that your editor should never put a tab character into your source file in response to your pressing the Tab key. The reason for this rule is that different tools treat tab characters differently — sometimes expanding them to 4 spaces, sometimes to 2 spaces, sometimes to 8. If you run “git diff” on the command line, or if you view your source code in a different editor, then the indentation may be completely screwed up. Just use spaces. Always set your programming editor to insert space characters when you press the Tab key. The 6.031 Eclipse installation has already been set up this way.

### Smelly example #3

Here’s a third example of smelly code that will illustrate the remaining points of this reading.

```java
public static int LONG_WORD_LENGTH = 5;
public static String longestWord;

public static void countLongWords(String text) {
    String[] words = text.split(' ');
    if (words.length == 0) {
        System.out.println("0");
        return;
    }
    int n = 0;
    longestWord = "";
    for (String word: words) {
        if (word.length() > LONG_WORD_LENGTH) ++n;
        if (word.length() > longestWord.length()) longestWord = word;
    }
    System.out.println(n);
}
```

### Don’t use global variables

Avoid global variables. Let’s break down what we mean by *global variable*. A global variable is:

- a *variable*, a name whose value can be changed
- that is *global*, accessible and changeable from anywhere in the program.

[Global Variables Are Bad](http://c2.com/cgi/wiki?GlobalVariablesAreBad) has a good list of the dangers of global variables.

In Java, a global variable is declared `public static`. The `public` modifier makes it accessible anywhere, and `static` means there is a single instance of the variable.

However, with one additional modifier, `public static final`, and if furthermore the variable’s type is immutable, then the name becomes a global *constant*. A global constant can be read anywhere in the program but never reassigned or mutated, so the risks go away. Global constants are common and useful.

In general, convert global variables into parameters and return values, or put them inside objects that you’re calling methods on. We’ll see many techniques for doing that in future readings.

---

#### READING EXERCISES

---

### Kinds of variables in snapshot diagrams

When we’re drawing snapshot diagrams, it’s important to distinguish between different kinds of variables:

- a *local variable* inside a method
- an instance variable inside an instance of an object
  - an instance variable may also be called a *field* (particularly in Java), a *property* (TypeScript/JavaScript), a *member variable* (C++), or an *attribute* (Python).
- a *static variable* associated with a class

A local variable comes into existence when a method is called, and then disappears when the method returns. If multiple calls to the same method are in progress (for example because of recursion), then each call will have its own independent local variables.

An instance variable comes into existence when an object is created with `new`, and then disappears when the object is no longer accessible and becomes garbage-collected. Each object instance has its own instance variables.

A static variable comes into existence when the program starts (or more precisely when the class containing the variable is first loaded, since that might be delayed), and exists for the rest of the life of the program.

Here is a simple piece of code that uses all three kinds of variables:

```java
class Payment {
    public double value;
    public static double taxRate = 0.05;
    public static void main(String[] args) {
        Payment p = new Payment();
        p.value = 100;
        taxRate = 0.05;
        System.out.println(p.value * (1 + taxRate));
    }
}
```

In the next two exercises, you will construct a snapshot diagram that shows the state of this program.

---

#### READING EXERCISES

---

### Methods should return results, not print them

`countLongWords` isn’t ready for change. It sends some of its result to the console, `System.out`. That means that if you want to use it in another context — where the number is needed for some other purpose, like computation rather than human eyes — it would have to be rewritten.

In general, only the highest-level parts of a program should interact with the human user or the console. Lower-level parts should take their input as parameters and return their output as results. The sole exception here is debugging output, which can of course be printed to the console. But that kind of output shouldn’t be a part of your design, only a part of how you debug your design.

---

#### READING EXERCISES

---

### Avoid special-case code

Programmers are often tempted to write special code to deal with what seem like special cases – parameters that are 0, for example, or empty lists, or empty strings. The `countLongWords` example falls into this trap by handling an empty `words` list specially:

```java
    if (words.size() == 0) {
        System.out.println(0);
        return;
    }
    int n = 0;
    longestWord = "";
    for (String word: words) {
        if (word.length() > LONG_WORD_LENGTH) ++n;
        if (word.length() > longestWord.length()) longestWord = word;
    }
    System.out.println(n);
}
```

The starting `if` statement is unnecessary. If it were omitted, and the `words` list is empty, then the `for` loop simply does nothing, and `0` is printed anyway. In fact, handling this special case separately has led to a possible bug – a difference in the way an empty list is handled, compared to a nonempty list that happens to have no long words on it.

**Actively resist the temptation to handle special cases separately.** If you find yourself writing an `if` statement for a special case, stop what you’re doing, and instead think harder about the general-case code, either to confirm that it can actually already handle the special case you’re worrying about (which is often true!), or put in a little more effort to make it handle the special case. If you haven’t even written the general-case code yet, but are just trying to deal with the easy cases first, then you’re doing it in the wrong order. Tackle the general case first.

Writing broader, general-case code pays off. It results in a shorter method, which is easier to understand and has fewer places for bugs to hide. It is likely to be safer from bugs, because it makes fewer assumptions about the values it is working with. And it is more ready for change, because there are fewer places to update when a change to the method’s behavior is made.

Some programmers justify handling special cases separately with a belief that it increases the overall performance of the method, by returning a hard coded answer for a special case right away. For example, when writing a sort algorithm, it can be tempting to check whether the size of the list is 0 or 1 at the very start of the method, since you can then return immediately with no need to sort at all. Or if the size is 2, just do a comparison and a possible swap. These optimizations might indeed make sense — but not until you have evidence that they actually would matter to the speed of the program! If the sort method is almost never called with these special cases, then adding code for them just adds complexity, overhead, and hiding-places for bugs, without any practical improvement in performance. Write a clean, simple, general-case algorithm first, and optimize it later, *only* if it would actually help.

---

#### READING EXERCISES

---

## Summary

Code review is a widely-used technique for improving software quality by human inspection. Code review can detect many kinds of problems in code, but as a starter, this reading talked about these general principles of good code:

- Don’t Repeat Yourself (DRY)
- Comments where needed
- Fail fast
- Avoid magic numbers
- One purpose for each variable
- Use good names
- Use whitespace to help the reader
- Don’t use global variables
- Methods should return results, not print them
- Avoid special-case code

The topics of today’s reading connect to our three key properties of good software as follows:

- **Safe from bugs.** In general, code review uses human reviewers to find bugs. DRY code lets you fix a bug in only one place, without fear that it has propagated elsewhere. Documenting your assumptions with clear comments makes it less likely that another programmer will introduce a bug. The Fail Fast principle detects bugs as early as possible. Avoiding global variables makes it easier to localize bugs related to variable values, since non-global variables can be changed in only limited places in the code.
- **Easy to understand.** Code review is really the only way to find obscure or confusing code, because other people are reading it and trying to understand it. Using judicious comments, avoiding magic numbers, keeping one purpose for each variable, using good names, and using whitespace well can all improve the understandability of code.
- **Ready for change.** Code review helps here when it’s done by experienced software developers who can anticipate what might change and suggest ways to guard against it. DRY code is more ready for change, because a change only needs to be made in one place. Returning results instead of printing them makes it easier to adapt the code to a new purpose.

Code review is not the only software development practice with strong supporting evidence. Another is: [sleep](https://increment.com/teams/the-epistemology-of-software-quality/).

