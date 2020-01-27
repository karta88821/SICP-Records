# Overview

The language serves as a framework within which we organize our ideas about processes, we should pay particular attention to the means that the language provide for combining simple ideas to form more complex idea. Every powerful language has three mechanisms for accomplishing this:

* primitive expressions (基本表達形式)
* meas of combination (組合的方法)
* means of abstraction (抽象的方法)

## Experssions

combined numbers with an expression representing a primitive procedure

```Lisp
(+ 111 222) // (operator operands...)
333

(* 5 5)
25
```

## Naming and the Environment

define a variable

```Lisp
(define size 2)
size
2
```

```Lisp
(define pi 3.14159)
(define radius 10)
(define circumference (* 2 pi radius)
circumference
62.8318
```

## Evaluating Combinations

In evaluating combiations, the interpreter is itself following a procedure.
To evaluate a combination, do the following:

1. Evaluate the subexpressions of the combiation.
2. Apply the procedure that is the value of the lefttmost subexpression(the operator) to the arguments that are values of the other subexpressions(the operands).

## Compound Procedures

Some of the elements that must appear in any powerful programming language:

* Numbers and arithmetic operations are primitive data and procedures.
* Nesting of combinations provides a means of combining operations.
* Definitions that associate names with values provide a limited means of abstration.

express the idea of “squaring.”

```Lisp
(define (⟨name⟩ ⟨formal parameters⟩) ⟨body⟩)
(define (square    x)      (*        x         x))
   |        |      |        |        |         |
  To     square something, multiply  it  by  itself.
```

```Lisp
(define (sum-of-squares x y) 
    (+ (square x) (square y)))
(sum-of-squares 3 4)
25
```

## The Substitution Model for Procedure Application

normal-order evaluation(正則序求值)：fully expand and then reduce

```Lisp
(f 5)
(sum-of-squares (+ a 1) (* a 2))
(sum-of-squares (+ 5 1) (* 5 2))
(+ (square 6) (square 10))
(+ (* 6 6) (* 10 10))
(+ 36 100)
136
```

applicative-order evaluation(應用序求值)：evaluate the arguments and then apply

```Lisp
(f 5)
(+ (square (+ 5 1)) (square (* 5 2)) )
(+ (*(+ 5 1)(+ 5 1)) (*(* 5 2)(* 5 2)))
(+ (* 6 6) (* 10 10))
(+ 36 100)
136
```

## Conditional Expressions and Predicates

conditional experssion

```Lisp
(cond (⟨p1⟩ ⟨e1⟩) (⟨p2⟩ ⟨e2⟩)
...
(⟨pn⟩ ⟨en⟩))
```

define an abs

```Lisp
(define (abs x) 
    (cond ((> x 0) x) 
          ((= x 0) 0)
          ((< x 0) (- x))))
```
>>  If x is less than zero return −x ; otherwise return x 
```Lisp
(define (abs x)
    (cond ((< x 0) (- x))
          (else x)))
```
```Lisp
(define (abs x)
    (if (< x 0)
        (- x) 
        x))
        
(if ⟨predicate⟩ ⟨consequent⟩ ⟨alternative⟩)
```

conpound predicates

```Lisp
(and⟨e1⟩ ... ⟨en⟩)
(or⟨e1⟩ ... ⟨en⟩)
(not ⟨e⟩)
```

### Example: Square Roots by Newton's Method

Definiation

```Lisp
√x = the y such that y ≥ 0 and y^2 = x.

(define (sqrt x)
(the y (and (>= y 0)
              (= (square y) x))))
```

Newton's method of successive approximations

Whenever we have a guess y for the value of the square root off a number x, we can perform a simple manipulation to get a better guess(one closer to the actual square root) by averaging y with x/y. For example, we cam compute the square root of 2 as follows. Suppose our initial guess is 1:

```none
Guess           Quotient            Average
1               (2/1) = 2           ((2 + 1)/2) = 1.5
1.5             (2/1.5) = 1.3333    ((1.3333 + 1.5)/2) = 1.4167
1.4167          (2/1.4167) = 1.4118 ((1.4167 + 1.4118)/2) = 1.4142
1.4142          ...                 ...
```

If the guess is good enough for our purposes, we are done; if not, we must repeat the process with an improved guess.

```Lisp
(define (sqrt-iter guess x)
    if (good-enough? guess x)
        guess
        (sqrt-iter (improve guess x) x))
```

A guess is improved by averaging it with the quotient of the radicand and the old guess

```Lisp
(define (improve guess x)
    (average guess (/ x guess)))
    
(define (average x y)
    (/ (+ x y) 2))
```

Improve the answer until it's close enough

```Lisp
(define (good-enough? guess x)
(< (abs (- (square guess) x)) 0.001))
```

Finally, we need a way to get started. For instance, we can always guess that the suqare root of any number 1

```Lisp
(define (sqrt x)
(sqrt-iter 1.0 x))
```
If we type these definitions to the interpreter, we can use sqrt just as we can use procedure:

```Lisp
(sqrt 9) 
3.00009155413138

(sqrt (+ 100 37)) 
11.704699917758145

(sqrt (+ (sqrt 2) (sqrt 3))) 
1.7739279023207892

(square (sqrt 1000)) 
1000.000369924366
```
