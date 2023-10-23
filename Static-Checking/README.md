# Reading 1: Static Checking

地址：https://web.mit.edu/6.031/www/sp22/classes/01-static-checking/

## Static checking, dynamic checking, no checking

It's useful to think about three kinds of automatic checking that a language can provide:

- **Static checking**: the bug is found automatically before the program even runs.-   **Dynamic checking**: the bug is found automatically when the code is executed.-   **No checking**: the language doesn't help you find the error at all. You have to watch for it yourself, or end up with wrong answers.

Needless to say, catching a bug statically is better than catching it dynamically, and catching it dynamically is better than not catching it at all.

Here are some rules of thumb for what errors you can expect to be caught at each of these times.

**Static checking** can catch:

- syntax errors, like extra punctuation or spurious words. Even dynamically-typed languages like Python do this kind of static checking. If you have an indentation error in your Python program, you'll find out before the program starts running.

- misspelled names, like `Math.sine(2)`. (The correct spelling is `sin`.)

- wrong number of arguments, like `Math.sin(30, 20)`.

- wrong argument types, like `Math.sin("30")`.

- wrong return types, like `return "30";` from a function that's declared to return a `number`.

**Dynamic checking** can catch, for example:

- specific illegal argument values. For example, the expression `x/y` is **erroneous**（错误的） when `y` is zero, but well-defined for other values of `y`. So divide-by-zero is not a static error, because you can't know until runtime whether `y` is actually zero or not. But divide-by-zero can be caught as a dynamic error; Python throws `ZeroDivisionError` when it happens.

- illegal conversions, i.e., when the specific value can't be converted to or represented in the target type. For example, in Python, `int("hello")` throws `ValueError`, because the string `"hello"` cannot be parsed as a decimal integer.

- out-of-range indices, e.g., using a too-large index on a string or array. In Python, `"hello"[13]` throws `IndexError`.

- calling a method on a bad object reference (`None` in Python, or `undefined` or `null` in TypeScript).

Static checking can detect errors related to the *type* of a variable -- the set of values it is allowed to have, which is known at compile time in statically-typed languages like TypeScript -- but is generally unable to find errors related to a specific value from that type.

But you may have noticed that the dynamic-checking examples above mostly came from Python. What about dynamic checking in TypeScript? TypeScript does the static checking, but its runtime behavior is entirely provided by *JavaScript*, and JavaScript's designers decided to do **no checking** for many of these cases. So, for example, when a string or array index is out of bounds, JavaScript returns the special value `undefined`, rather than throwing an error as Python would. When dividing by zero, JavaScript returns a special value representing infinity, rather than throwing an error. No checking makes bugs harder to find than they might otherwise be, because the special values can propagate through further computations until a failure finally occurs, much farther away from the original mistake in the code.

### Surprise: `number` is not a true number

Another trap in TypeScript -- which is shared by many other programming languages -- is that its numeric type has corner cases that do not behave like the integers and real numbers we're used to. As a result, some errors that really should be dynamically checked are not checked at all. Here are some of the **traps hiding**（隐藏陷阱） in TypeScript:

- **Limited precision for integers**. All numbers in TypeScript are floating-point numbers, which means that **large-magnitude integers**（大整数） can only be represented approximately. Integers from $-2^{53}$ to $2^{53}$ can be represented exactly, but beyond that range, the floating-point representation preserves only the most-significant binary digits of the number. What does that mean if you have, say, $2^{60}$ and try to increment it? You get the same number back again: `2**60 + 1 === 2**60` in TypeScript. So this is an example of where what we might have hoped would be a dynamic error (because the computation we wrote can't be represented correctly) produces the wrong answer instead.

    These limits on representable integers are available as the built-in constants `Number.MAX_SAFE_INTEGER` and `Number.MIN_SAFE_INTEGER`.

- **Special values**. The `number` type has several special values that aren't real numbers: `Number.NaN` (which stands for "Not a Number"), `Number.POSITIVE_INFINITY`, and `Number.NEGATIVE_INFINITY`. So when you apply certain operations to a `number` that you'd expect to produce dynamic errors, like dividing by zero or taking the square root of a negative number, you will get one of these special values instead. If you keep computing with it, you'll end up with a bad final answer.

- **Overflow and underflow**. Very large and very small numbers can't be represented either, beyond a certain point. `Number.MAX_VALUE` is roughly 10^308^, and `Number.MIN_VALUE` is roughly 10^\-324^. What happens when you do a computation whose answer is too large or too small (too close to zero) to fit in that finite range? The computation quietly *overflows* (becoming `POSITIVE_INFINITY` or `NEGATIVE_INFINITY`) or *underflows* (becoming zero).

#### READING EXERCISES

Let's try some examples of **buggy code**（有缺陷的代码） and see how they behave in TypeScript. Are these bugs caught statically, dynamically, or not at all?

1

```javascript
let n: number = 5;
n = n + ".0";`
```

static error

dynamic error

no error, wrong answer

CHECK

2

```javascript
let odd: number = 2**30 - 1;         // an odd number
let oddSquared: number = odd * odd;  // the square of an odd number should be odd`
```

static error

dynamic error

no error, wrong answer

CHECK

3

```javascript
let sum: number = 7;
let n: number = 0;
let average: number = sum/n;`
```

static error

dynamic error

no error, wrong answer

CHECK

4

```javascript
let x: number = 0;
x += 0.1; x += 0.1;
x += 0.1; x += 0.1;
x += 0.1; x += 0.1;
x += 0.1; x += 0.1;
x += 0.1; x += 0.1;
// x should be 1.0`
```

static error

dynamic error

no error, wrong answer

CHECK

5

```javascript
let n: number = 2**53;
while (n !== 2**60) {
    n = n + 1;
}
```

static error

dynamic error

no error, infinite loop

Note that these readings don't keep track of the exercises you've previously done. When you reload the page, all exercises reset themselves.

If you're a registered student, you can see which exercises you've already done by looking at [Omnivore](https://omni.mit.edu/6.031/sp22/user/classes/).

## Arrays

Let's change our hailstone computation so that it stores the sequence in a data structure, instead of just printing it out.

For that, we can use an array type. Arrays are variable-length sequences of another type, similar to Python lists. Here's how we can declare an array of numbers and make an empty array value:

```javascript
let array: Array<number> = [];
```

And here are some of its **operations**（操作）, which are very similar to Python:

- indexing: `array[2]`

- assignment: `array[2] = 0`

- length: `array.length`

- add an element to the end: `array.push(5)`

- remove an element from the end: `array.pop()`

You can see all the operations of `Array` in the Mozilla Developer Network (MDN) documentation; find it with a web search like ["mdn array"](https://www.google.com/search?q=mdn+array). Get to know the MDN docs, they're your friend.

Here's the hailstone code written with arrays:

```javascript
let array: Array<number> = [];
let n: number = 3;
while (n !== 1) {
    array.push(n);
    if (n % 2 === 0) {
        n = n / 2;
    } else {
        n = 3 * n + 1;
    }
}
array.push(n);
```

## Iterating

A `for` loop steps through the elements of an array, just as in Python, though the syntax looks a little different. For example:

```javascript
// find the maximum point of a hailstone sequence stored in array
let max: number = 0;
for (let x of array) {
    max = Math.max(x, max);
}
```

Be careful! Where in Python you use `for ... in ...`, the equivalent in TypeScript is `for ... of ...`. TypeScript also has a `for...in` construct which iterates over the keys of a collection, instead of its values. In the case of an array, the keys are the indices of the array. Note the difference that one word makes:

```javascript
for (let x in ["a", "b", "c"]) {    
    console.log(x);
}// prints 0, 1, 2
```

```javascript
for (let x of ["a", "b", "c"]) {    
    console.log(x);
}// prints "a", "b", "c"
```

**When you're iterating over an array, you almost always want `for...of`**, so you will need to untrain your Python habits here.

The example above also used `Math.max()`, which is a handy function from the JavaScript library. The `Math` class is full of useful functions like this -- web search ["mdn math"](https://www.google.com/search?q=mdn+math) to find its documentation in MDN.

## Functions

If we want to wrap this code into a reusable module, we can write a function:

```javascript
/**
 * Compute a hailstone sequence.
 * @param n  starting number for sequence.  Assumes n > 0.
 * @returns hailstone sequence starting with n and ending with 1.
 */
function hailstoneSequence(n: number): Array<number> {
    let array: Array<number> = [];
    while (n !== 1) {
        array.push(n);
        if (n % 2 === 0) {
            n = n / 2;
        } else {
            n = 3 * n + 1;
        }
    }
    array.push(n);
    return array;
}
```

Take note of the `/** ... */` comment before the function, because it's very important. This comment is a specification of the function, describing the inputs and outputs of the operation. The specification should be concise, clear, and precise. The comment provides information that is not already clear from the function types. It doesn't say, for example, that the return value is an array of numbers, because the `Array<number>` return type declaration just below already says that. But it does say that the sequence starts with `n` and ends with 1, which is not captured by the type declaration but is important for the caller to know.

We'll have a lot more to say about how to write good specifications in a few classes, but you'll have to start reading them and using them right away.

## Mutating values vs. reassigning variables

**Change is a necessary evil**（改变是必要之恶）. But good programmers try to avoid things that change, because they may change unexpectedly. Immutability -- **intentionally**（有意地） forbidding certain things from changing at runtime -- will be a major design principle in this course.

For example, an immutable type is a type whose values can never change once they have been created. The string type is immutable in both Python and TypeScript.

TypeScript also allows us to declare immutable references: variables that are assigned once and never reassigned. To make a reference unreassignable, declare it with the keyword `const` instead of `let`:

```javascript
const n: number = 5;
```

If the TypeScript compiler isn't convinced that your `const` variable will only be assigned once at runtime, then it will produce a compiler error. So `const` gives you static checking for unreassignable references.

It's good practice to use `const` for as many variables as possible. Like the type of the variable, these declarations are important documentation, useful to the reader of the code and statically checked by the compiler.

#### READING EXERCISES

const

Consider the variables in our `hailstoneSequence` function:

```javascript
function hailstoneSequence(n: number): Array<number> {
    let array: Array<number> = [];
    while (n !== 1) {
        array.push(n);
        if (n % 2 === 0) {
            n = n / 2;
        } else {
            n = 3 * n + 1;
        }
    }
    array.push(n);
    return array;
}
```

Which variables might we declare `const`, because they are never reassigned in the code?

`n: number`
`array: Array<number>`

## Documenting assumptions

Writing the type of a variable down documents an assumption about it: e.g., the variable `n` will always refer to a number. TypeScript actually checks this assumption at compile time, and guarantees that there's no place in your program where you violated this assumption.

Declaring a variable `const` is also a form of documentation, a claim that the variable will never be reassigned after its initial assignment. TypeScript checks that too, statically.

We documented another assumption that TypeScript (unfortunately) doesn't check automatically: that `n` must be positive.

Why do we need to write down our assumptions? Because programming is full of them, and if we don't write them down, we won't remember them, and other people who need to read or change our programs later won't know them. They'll have to guess.

Programs have to be written with two goals in mind（编写程序时必须牢记两个目标）:

- communicating with the computer. First persuading the compiler that your program is sensible -- syntactically correct and type-correct. Then getting the logic right so that it gives the right results at runtime.

- communicating with other people. Making the program easy to understand, so that when somebody has to fix it, improve it, or adapt it in the future, they can do so.

## Hacking vs. engineering

We've written some hacky code in this reading. Hacking is often marked by **unbridled optimism**（无限乐观）:

- Bad: writing lots of code before testing any of it.

- Bad: keeping all the details in your head, assuming you'll remember them forever, instead of writing them down in your code.

- Bad: assuming that bugs will be nonexistent or else easy to find and fix.

But software engineering is not hacking. Engineers are **pessimists**（悲观主义者）:

- Good: write a little bit at a time, testing as you go. In a future class, we'll talk about test-first programming.

- Good: document the assumptions that your code depends on.

- Good: **defend your code against stupidity**（保护你的代码免遭愚蠢之举） -- especially your own! Static checking helps with that.

#### READING EXERCISES

Consider the following simple Python function:

```python
from math import sqrt
def funFactAbout(person):
  if sqrt(person.age) == int(sqrt(person.age)):
    print("The age of " + person.name + " is a perfect square: " + str(person.age))`
```

Assumptions

Checkable assumptions

## The goal of 6.031
-----------------

Our primary goal in this course is learning how to produce software that is:

- **Safe from bugs**. Correctness (correct behavior right now) and defensiveness (correct behavior in the future) are required in any software we build.-   **Easy to understand**. The code has to communicate to future programmers who need to understand it and make changes in it (fixing bugs or adding new features). That future programmer might be you, months or years from now. You'll be surprised how much you forget if you don't write it down, and how much it helps your own future self to have a good design.-   **Ready for change**. Software always changes. Some designs make it easy to make changes; others require throwing away and rewriting a lot of code.

There are other important properties of software (like performance, usability, security), and they may trade off against these three. But these are the Big Three that we care about in 6.031, and that software developers generally put foremost in the practice of building software. It's worth considering every language feature, every programming practice, every design pattern that we study in this course, and understanding how they relate to the Big Three.

#### READING EXERCISES

Which of the following ideas discussed in this reading generally help make your code more **safe from bugs**?

dynamic checking

const variables

overflow

static typing

> Dynamic checking and static typing both catch bugs earlier than no checking at all. `Const` variables can prevent bugs associated with reassigning a variable that shouldn't have been reassigned. But overflow is unfortunately a *cause* of bugs.

ETU

Which of the following ideas discussed in this reading generally help make your code more **easy to understand**?

documented assumptions in comments

const variables

overloading

static typing

> Documentation makes code easier to understand, whether it comes in the form of comments, `const`, or type declarations. But overloading (using the same operator with different meanings) can make code harder to understand.

RFC

Which of the following ideas discussed in this reading generally help make your code more **ready for change**?

documented assumptions in comments

functions

static typing

> Documenting assumptions, and wrapping code into functions, make it easier for subsequent programmers to understand and change it. Static checking makes it easier to change your code by identifying other places that need to change in tandem. For example, when you change the name or type of a variable, the compiler immediately displays errors at all the places where that variable is used, reminding you to update them as well.

### Why we use TypeScript in this course

Since you've had 6.009, we're assuming that you're comfortable with Python. So why aren't we using Python in this course? Why do we use TypeScript/JavaScript in 6.031?

**Safety** is the first reason. TypeScript has static checking (primarily type checking, but other kinds of static checks too, like that your code returns values from methods declared to do so). We're studying software engineering in this course, and safety from bugs is a key **tenet**（原则） of that approach. TypeScript dials safety up to a high level, which makes it a good language for learning about good software engineering practices. It's certainly possible to write safe code in dynamic languages like Python or JavaScript, but it's easier to understand what you need to do if you learn how in a safe, statically-checked language.

**Ubiquity** is another reason. TypeScript compiles into pure JavaScript, which is widely used in research, education, and industry. JavaScript runs on many platforms, not just in the web browser where it originally became widespread, but now also in web servers and even in desktop applications for Windows/Mac/Linux.

Compared to Java, TypeScript has a richer type system, needs less **boilerplate code**（样板代码） to write a program, and is a better choice for creating modern user interfaces and web apps.

In any case, a good programmer must be **multilingual**. Programming languages are tools, and you have to use the right tool for the job. You will very likely have to pick up other programming languages (such as Java, C/C++, or Scheme or Ruby or Haskell) for classes, internships, or UROPs before you even finish your MIT career, so we're getting started now by learning a second one.

As a result of its ubiquity, JavaScript has a wide array of interesting and useful **libraries**, and excellent free **tools** for development (IDEs like VS Code, editors, compilers, test frameworks, profilers, code coverage, style checkers). JavaScript and Python are **close competitors**（激烈的竞争对手） in the richness of their ecosystems, and TypeScript can take full advantage of JavaScript libraries.

There are some reasons to regret using JavaScript. It's large, having accumulated many features in the decades since it was originally designed. It's weighed down by the baggage of older languages like C/C++ --- the [`switch` statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) is a good example of an **antiquated（过时的）** and unsafe language construct inherited from C. And JavaScript itself started with some poor design decisions, including type conversion rules that make [`0 == ""` return true](https://web.mit.edu/6.031/www/sp22/classes/01-static-checking/#triple-equals) and [`[] + []` somehow produce the empty string](https://www.destroyallsoftware.com/talks/wat), as well as [less dynamic checking](https://web.mit.edu/6.031/www/sp22/classes/01-static-checking/#no-checking-in-JS) than other languages. TypeScript fixes some of these **warts**（缺点） in JavaScript, but not all. There are many parts of JavaScript that are now deprecated and avoided by programmers, because they're risky to use.

But on the whole, TypeScript is a reasonable choice of language right now to learn how to write code that is safe from bugs, easy to understand, and ready for change. And that's our goal.

Don't get us wrong: the real skills you'll get from this course are not language-specific, but carry over to any language that you might program in. The most important lessons from this course will survive language fads: safety, clarity, abstraction, engineering instincts.

## Summary


The main idea we introduced today is **static checking**. Here's how this idea relates to the goals of the course:

- **Safe from bugs.** Static checking helps with safety by catching type errors and other bugs before runtime.

  - **Easy to understand.** It helps with understanding, because types are explicitly stated in the code.

  - **Ready for change.** Static checking makes it easier to change your code by identifying other places that need to change **in tandem**（一起）.