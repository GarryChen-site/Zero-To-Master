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







## Reference 

* [Structure and Interpretation of Computer Programs](https://mitpress.mit.edu/books/structure-and-interpretation-computer-programs-1)