

# Building Abstractions with Functions

## The Elements of Programming

* primitive expressions
* mean of combination
* means of abstraction

Function and Data

> Data is "stuff" that we want to manipulate, and Functions are descriptions of the rules
>
> for manipulating the data.

### Expressions

``` js
486;

137 + 349; // 486

1000 - 334; // 666

5 * 99; // 495

10 / 4; // 2.5

2.7 + 10; // 12.7

```

Expressions such as these, which contain other expressions as components, are called combinations.

``` js
(3 * 5) + (10 - 6); // 19

3 * 5 + 10 / 2;
// stands for 
(3 * 5) + (10 / 2);

1 - 5 / 2 * 4 + 3;
// stands for 
(1 - ((5 / 2) * 4)) + 3;

3 * 2 * (3 - 5 + 4) + 27 / 6 * 10;

3 * 2 * (3 - 5 + 4)
+
27 / 6 * 10;

```

Reads a statement typed by the user, evaluates the statement, and prints the result.



### Naming and the Environment

A critical aspect of a programming language is the means it provides for using names to refer to computational objects, and our first such means are constants.

``` js
const size = 2;
```

caused the intercepter to associate the value 2 with name *size*.Once the name *size* has been associated with the number2, we can refer to the value 2 by name

``` js
size;

// 2

5 * size;

// 10
```

The interpreter needs to execute the constant declaration for *size* before the name *size* can be used in an expression. 

``` js
const size = 2;
5 * size;

const pi = 3.14159;
const radius = 10;
pi * radius * radius;
// 314.159

const circumference = 2 * pi * radius;

circumference;
// 62.8318
```

Constant declartion is our language's simplest means of abstraction, for it allows us to use simple names to refer to the result of compound operations, such as the *circumference* computed above. In general, computational objects may have very complex structures, and it would be extremely inconvenient to have to remember and repeat their details each time we want to use them. Indeed, complex programs are constructed by building, step by step, computational objects of increasing complexity.

It should be clear that the possibility of associating values with names and later retrieving them means that the interpreter must maintain some sort of memory that keeps track of the name-object pairs.This memory is called the environment.



### Evaluating Operator Combinations

To evaluate an operator combination, do the following:

1. Evaluate the operand expressions of the combination 
2. Apply the function that is denoted by the operator to the arguments that are the values of the operands

First, observe that the first step dictates that in order to accomplish the evaluation process for a combination we must first perform the evaluation process on each operand of the combination .Thus, the evaluation rule is *recursive* in nature; that is , it includes, as one of its steps, the need to invoke the rule itself.

Notice how succinctly the idea of recursion can be used to express what, in the case of a deeply nested combination ,would otherwise be viewed as a rather complicated process.

``` js
(2 + 4 * 6) * (3 + 12)
```

We can obtain a picture of this precess by representing the combination in the form of a tree.

Each combination is represented by a node with branches corresponding to the operator and the operands of the combination stemming from it.The terminal nodes (that is, nodes with no branches stemming from them) represent either operators or numbers.Viewing evaluation in terms of the tree, we can imagine that the values of the operands percolate upward, starting from the terminal nodes and then combining at higher and higher levels.

![](chap1/ch1-Z-G-1.svg)

Observe that the repeated application of the first step brings us to the point where we need to evaluate, not combination , but primitive expressions such as numerals or names. We take care of the primitive cases by stipulating that

* the values of numerals are the numbers that they name, and 
* the values of names are the objects associated with those names in the environment

The key point to notice is the role of the environment in determining the meaning of the names in expressions. In an interactive language such as JavaScript, it is meaningless to speak of the value of an expression such as x + 1 without specifying any information about the environment that would provide a meaning for the name x. The general notion of the environment as providing a context in which evaluation takes place will play an important role in our understanding of program execution.

The purpose of the declaration (const x = 3) is precisely to associate x with a value.(That is, const x = 3; is not a combination.)

The word const is a keyword in JS. Keywords carry a particular meaning, and thus cannot be used as names.

A keyword or a combination of keywords in a statement instructs the JavaScript interpreter to treat the statement in a special way.

Each such syntactic from has its own evaluation rule. The various kinds of statements and expressions (each with its associated evaluation rule) constitute the syntax of the programming language.



### Compound Functions

We have identified in JavaScript some of the elements that must appear in any powerful programming language:

* Numbers and arithmetic operations are primitive data and functions
* Nesting of combinations provides a means of combining operations
* Constant declarations that associate means with values provide a limited means of abstraction

Now we will learn about function declarations, a much powerful abstraction technique by which a compound operation can be given a name and then referred to as a unit.

We begin by examining how to express the idea of "squaring". We might say, "To square something, take it times itself."This is expressed in our language as

``` js
function square(x) {
  return x * x;
}
```

``` txt
function square(    x   ) { return x    *     x; }
//  ^       ^       ^         ^    ^    ^     ^
// To    square something,  take   it times itself.
```

The simplest form of a function declaration is 

``` txt
function name(parameters) { return expression; }
```

Having declared *square*, we can now use it in a function application expression

``` js
square(21);
// 441
```

Function applications are-after operator combinations-the second kind of combination of expression into large expressions.The general form of a function application is

``` text
function-expression( argument-expressions )
```

Where the function-expression of the application specifies the function to be applied to the comma-separated argument-expressions.

* To evaluate a function application, do the following:

  1. Exaluate the subexpressions of the application, namely the function expression and the argument expressions
  2. Apply the function that is the value of the function expression to the values of the argument expression.

  ``` js
  square(2 + 5);
  // 49
  ```

  Of course function application expressions can also serve as argument expressions.

  ``` text
  square(x) + square(y)
  ```

  Declare a function *sum_of_squares* that, given any two numbers as arguments,produces the sum of their squares:

  ``` js
  function sum_of_squares(x, y) {
    return square(x) + square(y);
  }
  
  sum_of_square(3,4);
  ```

  Now we can use sum_of_squares as a building block in constructinf future functions:

  ``` js
  function f(a) {
    return sum_of_squares(a + 1, a * 2);
  }
  f(5);
  ```

  In addition to compound functions, any JS environment provides primitive functions that are build into the interpreter or loaded from libraries. Indeed, one could not tell by looking at the definition of *sum_of_squares* given above whether *square* was build into the interpreter, loaded from a library, or defined as a compound function.



### The Substitution Model for Function Application

To evaluate a function application, the interpreter evaluates the elements of the application and applies the function (which is the value of the function expression of the application) to the arguments (which are the values of the argument expressions of the application).

We can assume that the application of primitive functions is handed by the interpreter or libraries. For compound functions, the application process is as follows:

* To apply a compound function to arguments, evaluate the return expression of the function with each parameter replaced by the corresponding argument

Let's evaluate the application

``` js
f(5)
```

We begin by retrieving the return expression of f:

``` text
sum_of_squares(a + 1, a * 2)
```

Then we replace the parameter a by the argument 5:

``` text
sum_of_squares(5 + 1, 5 * 2)
```

Thus the problem reduces to the evaluation of an application with two arguments and a functionc expression *sum_of_squares*.Evaluating this application involves three subproblems. We must evaluate the function expression to get the function to be applied, and we must evaluate the argument expressions to get the arguments. Now *5 + 1* produces 6 and *5* * *2* produces 10, so we must apply the *sum_of_squares* function to 6 and 10. These values are substituted for the parameters x and y in the body of *sum_of_squares*,reducing the expression to 

``` text
square(6) + square(10)
```

If we used the declaration of *square*, this reduces to 

``` text
(6 * 6) + (10 * 10)
```

which reduces by multiplication to 

``` text
36 + 100
```

and finally to 

``` text
136
```

The process we have just described is called the *substitution model* for function application. It can be taken as a model that determinis the "meaning" of function application, in so far.

Two points that should be stressed:

* The purpose of the substitution is to help us think about function application, not to provide a description of how the interpreter really works. Typical interpreters do not evaluate function applications by manipulating the text of a function to substitute values for the parameters.In practice, the "substitution" is accompanied by using a local environment for the parameters.
* We will present a sequence of increasingly elaborate models of how interpreter work. The substitution model is only the first of these models-a way to get started thinking formally about the evaluation process.In general, when                 modeling phenomena in science and engineering, we begin with simplified, incomplete models. As we examine things in greater detail, these simple models become inadequate and must be replaced by more refined models. 

#### Applicative order versus normal order 

According to the description of evaluation, the interpreter first evaluates the function and argument expressions and then applies the resulting function to the resulting arguments. An alternative evaluation model would not evaluate the arguments until their values were needed.Instead it would first substitute argument expressions for parameters until it obtained an expression involving only operators and primitive functions, and would then perform the evaluation .

``` js
f(5)

sum_of_squares(5 + 1, 5 * 2)
square(5 + 1)				+ square(5 * 2)
(5 + 1) * (5 + 1) + (5 * 2) * (5 * 2)
6 * 6 + 10 * 10
 36       100
     136
```

This gives the same answer as out previous evaluation model, but the process is different.In particular, the evaluations of *5 + 1* and *5* * *2* are each performed twice here.

This alternative "fully expand and then reduce" evaluation method is known as *normal-order evaluation, in contrast to the "evaluate the arguments and then apply" method that the interpreter actually uses, which is called *applicative-order evaluation*. It can be shown that, for function applications that can be modeled using substitution and that yield legitimate values, normal-order and applicative-order evaluation produce the same value.

JS uses applicative-order evaluation,partly because of the additional efficiency obtained from avoiding multiple evaluations of expressions such as those illustrated with 5 + 1 and 5 * 2 above and , more significantly, because normal-order evlaluation becomes much more complicated to deal with when we leave the realm of functions that can be modeled by substitution.



### Conditional Expressions and Predicates

Case analysis can be written in JS usiing a conditional expression as 

``` js
function abs(x) {
  return x >= 0 ? x : - x;
}
```

which could be expressed as "If x is great than or equal to zero, return x; otherwise return -x"

The general form of a conditional expression is 

``` text
predicate ? consequent-expression : alternative-expression
```

Conditional expressions begin with a predicate-that is, an expression whose value is either true or false

To evaluate a conditional expression, the interpreter starts by evaluating the predicate of the expression.If the predicate evaluates to true, the interpreter evalautes the *consequent-expression* and return its value as the value of the conditional.If the predicate evaluates to false, it evaluates the *alternative-expression* and returns its value as the value of the conditional.

The word *predicate* is used for operators and functions that return true or false, as well as for expressions that evaluate to true or false.

If we prefer to handle the zero case separately, in js, we can express 

``` js
function abs(x) {
  return x > 0
  			? x
  			: x === 0
  			? 0
  			: -x;
}
```

In addition to primitive predicates such as >=, >, <, <=, ===, and !== that are applied to numbers, there are logical composition operations ,which enable us to construct compound predicates. The three most frequently used are these:

* *expression1 && expression2*

  This operation expresses *logic conjunction*, meaning roughly the same as the English word "and". This syntactic form is syntactic sugar for *expression1 ? expression2 : false*

* *expression1 || expression2*

  This operation expresses *logic disjunction*, meaning roughly the same as the English word "or". This syntactic form is syntactic sugar for *expression1 ? true : expression2 *

* *! expression*

  This operation expresses *logic negation*, meaning roughly the same as the English word "not".The value of the expression is true when *expression* evalautes to false, and false when *expression* evaluates to true.

As an example of how these predicates are used, the condition that a number *x* can be in range *5<x<10* may be expressed as

``` text
x > 5 && x < 10
```

The syntactic form *&&* has low precedence than the comparison operators > and <

As another example, we can declare a predicate to test whether one number is greater than or equal to another as

``` js
function greater_or_equal(x, y) {
  return x > y || x === y
}
```

or alternatively as

``` js
function greater_or_equal(x, y) {
  return !(x < y)
}
```

The function *greater_or_equal*, when applied to two numbers, behaves the same as the operator >=. Unary operators have higher precedence than binary operators, which makes the parentheses in this example necessary.



#### Exercise 1.1

#### Exercise 1.2

#### Exercise 1.3

#### Exercise 1.4

#### Exercise 1.5



### Example: Square Roots by Newtom's Method

Functions, as introduced above, are much like ordinary mathematical functions. They specify a value that is determined by one or more parameters.But there is an important difference between mathematical functions and computer functions.Computer functions must be effective.

As a case in point, consider the problem of computing square roots. We can define the square-root function as 

![](chap1/ch1-1.1.7-square-root.png)

This describes a perfectly legitimate mathematical function. But, the definition does not describe a computer function.Indeed, it tells us almost nothing about how to actually find the square root of a given number.It will not help matters to rephrase this defintion in pseudo-JS

``` js
function sqrt(x) {
  return the y with y >= 0 && square(y) === x;
}
```

The contrast between mathematical function and computer function is a reflection of the general distinction between describing properties of things and describing how to do things, or, as it is sometimes referred to, the distinction between declarative knowledge and imperative knowledge . In mathematics we are usually concerned with declarative (what is) description, whereas in computer science we are usually concerned with imperative (how to) descriptions.

How does one compute square roots? The most common way is to use Newton's method of successive approximations, which says that whenever we have a guess *y* for the value of the square root of a number *x*, we can perform a simple manipulation to get a better guess (one closer to the actual square root) by averaging *y* with *x/y*.

For example, we can compute the square root of 2 as follows. Suppose our initial guess is 1:

 

| Guess  |     Quotient      |          Average           |
| :----: | :---------------: | :------------------------: |
|   1    |      2/1 = 2      |       (2+1)/2 = 1.5        |
|  1.5   |  2/1.5 = 1.3333   |  (1.3333+1.5)/2 = 1.4167   |
| 1.4167 | 2/1.4167 = 1.4118 | (1.4167+1.4118)/2 = 1.4142 |

Now let's formalize the process in terms of functions. We start with a value for the radicand(the number whose square root we are trying to compute) and a value for the guess.If the guess is good enough for our purposes, we are done; if not, we must repeat the process with an improved guess.

``` js
function sqrt_iter(guess, x) {
  return is_good_enough(guess, x)
  			? guess
  			: sqrt_iter(improve(guess, x), x);
}
```



A guess is improved by averaging it with the quotient fo the radicand and the old guess:

``` js
function improve(guess, x) {
  return average(guess, x / guess)
}
```

where

``` js
function average(x, y) {
  return (x + y) / 2;
}
```

We also have to say that we mean by "good enough." The idea is to improve the answer until it is close enough so that its square differs from the radicand by less than a predetermined tolerance (here 0.001)

``` js
function is_good_enoufh(guess, x) {
  return abs(square(guess) - x) < 0.001;
}
```

Finally, we need a way to get started. For instance, we can always guess that the square root of any number is 1:

``` js
function sqrt(x) {
  return sqrt_iter(1, x)
}
```

The function *sqrt_iter*, demonstrates how iteration can be accomplished using no special construct other than the ordinary ability to call a function.



#### Exercise 1.6

#### Exercise 1.7

#### Exercise 1.8



### Functions as Black-Box Abstractions

The function *sqrt* is our first example of a process defined by a set of mutually defined functions.Notice that the declaration of *sqer_iter* is *recursive*; that is, the function is defined in terms of itself .The idea of being able to define a function in terms of itself may be disturbing ; it may seen unclear how such a "circular" definition could make sense at all, much less specify a well-defined process to be carried out by a computer. Let's consider some other important points illustrated by the *sqrt* example.



Observe that the problem of computing square roots breaks up naturally into a number of subproblems: how to tell whether a guess is good enough, how to improve a guess, and so on. Each of these tasks is accomplished by a separate function. The entire *sqrt* program can be viewed as a cluster of functions that mirrors the decomposition of the problem into subproblems.

![](chap1/ch1-1.18-fig1.4.png)

The improtance of this decomposition strategy is not simply that one is dividing the program into parts. After all ,we could take any largt program and divide it into parts--the first ten lines, the next ten lines, the next ten line, and so on. Rather, it is crucial that each function  accomplished an identifiable task that can be used as a module in defining other functinos. For example, when we define the *is_good_enough* function in terms of *square*, we are able to regard the *square* function as a "black box." We are not at that moment conncerned with *how* the function computes its result, only with the fact that it compute the square. The detail of how to square is computed can be suppressed, to be considered at a later time. Indeed, as far as the *is_good_enough* function is concerned, *square* is not quite a function but rather than an abstraction of a function, a so-called *functional abstraction*.At this level of abstraction, any function that computes the square is equally good.

Thus, considering only the values they return, the following two functions squaring a number should be indistinguishable . Each takes a numerical argument and produces the square of that number as the value.

``` js
function square(x) {
  return x * x;
}
```

``` js
function square(x) {
  return math_exp(double(math_log(x)))
}
function double(x) {
  return x + x;
}
```

So a function should be able to suppress detail.

#### Local names

One detail of a function's implementation that should not matter to the user of the function is the implementer's choice of names for the function's parameters. Thus, the following functions should not be distinguishable:

``` js
function square(x) {
  return x * x;
}

function square(y) {
  return y * y;
}
```

The principle-that the meaning of a function should be independent of the parameter names used by its author. The simplest consequence is that the parameter names of a function must be local to the body of the function.

For example, we used *square* in the declaration of *is_good_enough* in out square-root function:

``` js
function is_good_enough(guess, x) {
  return abs(square(guess) - x) < 0.001;
}
```

The intention of the author of *is_good_enough* is to determine if the square of the first argument is within a given tolerance of the second argument.We see that the author of *is_good_enough* used the name *guess* to refer to the first argument and x to reder to the second argument. The argument of *square* is *guess*. If the author of *square* used x (as above) to refer to that argument, we see that the x in *is_good_enough* must be a different x than the one in *square*. Running the function *square* must not affect the value of x that is used by *is_good_enough*, because that value of x may be needed by *is_good_enough* after *square* is done computing.

If the parameters were not local to the bodies of their respective functions, then the parameter x in *square* could be confused with the parameter x in *is_good_enough*, and the behavior of *is_good_enough* would depend upon with version of *square* we used. Thus, *square* would not be the black box we desired.

A parameter of a functino has a very special role in the function declaration , in that it doesn't matter what name the parameter has. Such a name is called bound, and we say that the function declaration *binds* its parameters. The meaning of a function declaration is unchanged if a bound name is consistently renamed throughout the declaration. It a name is not bound, we say that it is *free*.The set of statements for which a binding declares a name is called the *scope* of that name. In a function declaration, the bound names declared as the parameters of the function have the body of the function as their scope.

In the declaration of *is_good_enough* above, *guess* and *x* are bound names but *abs* and *square* are free. The meaning of *is_good_enough* should be independent of the names we choose for *guess* and *x* so long as they are distinct and different from *abs* and *square*.(If we renamed *guess* and *abs* we would have introduced a bud by *capturing* the name *abs*. It would have changed from free to bound.) The meaning of *is_good_enough* is not indepent of the choice of its free names,however. It surely depends upon the face(external to this declaration) that the name *abs* refers to a function for computing the absolute value of a number. The function *is_good_enough* will compute a different function if we substitute *math_cos* (the primitive cosine function) for *abs* in its declaration . 



#### Internal declarations and block structure

We have one kind of name isolation available to us so far: The parameters of a function are local to the body of the function.The square-root program illustrates another way in which we would like to control the use of names. 

``` js
function sqrt(x) {
  return sqrt_iter(1,x);
}
function sqrt_iter(guess, x) {
  return is_good_enough(guess,x)
  			 ? guess
  			 : sqrt_iter(improve(guess, x), x);
}
function is_good_enough(guess, x) {
  return abs(square(guess) - x ) < 0.001;
}
function improve(guess, x) {
    return average(guess, x / guess);
} 
```

The problem with this program is that the only function that is important to users of *sqrt* is *sqrt*. The other functions(sqrt_iter, is_good_enough, and improve) only clutter up their minds.

We would like to localize the subfunctions, hiding them inside *sqrt* so that *sqrt* could coexit with other successive approximations, each having its own private *is_good_enough* function. To make this possible, we allow a function to have internal declarations that are local to that function.

``` js
function sqrt(x) {
    function is_good_enough(guess, x) {
        return abs(square(guess) - x) < 0.001;
    }
    function improve(guess, x) {
        return average(guess, x / guess);
    }
    function sqrt_iter(guess, x) {
        return is_good_enough(guess, x) 
               ? guess
               : sqrt_iter(improve(guess, x), x);
    }
    return sqrt_iter(1, x);
} 
```

Any matching pair of braces designates a *block*, and declarations inside the block are local to the block. Such nesting of declarations, called *block structure*, is basically the right solution to the simplest name-packaging problem.

In addition to internalizing the declarations of the auxiliary functions, we can simplify them.

``` JS
function sqrt(x) {
    function is_good_enough(guess) {
        return abs(square(guess) - x) < 0.001;
    }
    function improve(guess) {
        return average(guess, x / guess);
    }
    function sqrt_iter(guess) {
        return is_good_enough(guess)
               ? guess
               : sqrt_iter(improve(guess));
    }
    return sqrt_iter(1);
} 
```

We will use block structure extensively to help us break up large programs into tractable pieces.

It appears in most advanced programming languages and is an important tool for helping to organize the construction of large programs.

## Reference 

* [Structure and Interpretation of Computer Programs](https://mitpress.mit.edu/books/structure-and-interpretation-computer-programs-1)