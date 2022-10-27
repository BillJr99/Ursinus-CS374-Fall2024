---
layout: activity
permalink: /Activities/LambdaCalculus4
title: "CS374: Programming Language Principles - The Lambda Calculus"
excerpt: "CS374: Programming Language Principles - The Lambda Calculus"

info: 
  prev: ./LambdaCalculus2
  goals: 
    - To derive numeric and recursive structures using the Lambda Calculus, beta-reductions, and the Y Combinator         
        
tags:
  - lambdacalculus
  
---

(define n0 (lambda (f a) a))

(define n1 (lambda (f a) (f a)))

(define n2 (lambda (f a) (f (f a))))

const jsnum = n =&gt; n(x =&gt; x + 1)(0);

 

n0 = f =&gt; a =&gt; a

n1 = f =&gt; a =&gt; f(a)

n2 = f =&gt; a =&gt; f(f(a))

n3 = f =&gt; a =&gt; f(f(f(a)))

n4 = f =&gt; a =&gt; f(f(f(f(a))))

 

console.log(jsnum(n4))

**What is the pattern?**

 

The Church numeral **n** is the application of a function **f**, **n** times, to a variable **a**.

(f….f a)

 

Suppose you wanted to define a function that accepts a Church numeral n, like n2, and add 1 to it. What would you do? Hint: Recall that **n** is a function on **f** and **a** that is already a certain number of applications of **f**!

 

(define succ (lambda (n f a) (\_\_\_\_ (n f a))))

 

 

Mathematics on Church Numerals in Scheme:

 

Add together **n** and **k** by applying **succ** to **k**, **n** times

(define add (lambda (n k) (succ n k)))

 

(define mult (lambda (n k f) (n (k f)))) ; n times, apply k times to f

> ; This is the Bluebird Combinator
>
>  

(define bluebird (lambda (f g a) (f (g a)))) ; composition

(define mult (lambda (n k f) (bluebird n k f)))

 

 

(define pow (lambda (n k) (k n)))

 

(define cardinal (lambda (f a b) (f b a))) ; Flip parameters - there are 3

> ; parameters, how can we
>
> ; "eliminate" one?

(define thrush (lambda (a f) (f a))) ; (cardinal identity)

(define pow (lambda (n k) (thrush n k)))

 

And in node.js:

 

succ = n =&gt; f =&gt; a =&gt; f(n(f)(a))

 

console.log(jsnum(succ(n0)))

 

B = f =&gt; g =&gt; a =&gt; f(g(a))

 

succB = n =&gt; f =&gt; B(f)(n(f))

console.log(jsnum(succB(n1)))

 

add = n =&gt; k =&gt; n(succ)(k)

 

console.log(jsnum(add(n2)(n3)))

 

mult = n =&gt; k =&gt; f =&gt; n(k(f))

multB = n =&gt; k =&gt; f =&gt; B(n)(k)(f)

 

console.log(jsnum(mult(n2)(n3)))

console.log(jsnum(multB(n2)(n3)))

 

pow = n =&gt; k =&gt; k(n)

 

console.log(jsnum(pow(n3)(n2)))

 

 

 

Via Lambda Calculus Reductions:

 

succ(n) = λwyx.y(wyx) (n)

succ(0) = λwyx.y(wyx) (0)

= λwyx.y(wyx) (λnz.z)

= λnz.n(z) = 1

 

mult(x, y) = λxyz.x(yz)

mult(3, 2) = λxyz.x(yz) 3 2 ; 3 f's and 2 f's

= λz.3(2z) = 6 ; apply the 3 f's to a 2 times

; by substituting 2z for each of the 3 f's

 
*From <https://math.stackexchange.com/questions/595518/how-to-multiply-in-lambda-calculus>*

 

Recursion with the Y-Combinator

Y = λf.(λx.fr(x x))(λx.f(x x))

Y g = (λf.(λx.f(x x))(λx.f(x x))) g

Y g = (λx.g (x x)) (λx.g (x x)) : after b-reduction of g for f

Y g = g ((λx.g (x x)) (λx.g (x x))) : after b-reduction of right parameter into left function for x

Y g = g (Y g) : substitution of Y = lambda f.(lambda x.f(x x))(λx.f(x x)) in inner parameter

 

*From <https://en.wikipedia.org/wiki/Fixed-point_combinator#Fixed-point_combinators_in_lambda_calculus>*

 

(define ycombinator ((lambda (f) (lambda (x) (f (x x)))) (lambda (x) (f (x x)))))

 

Y = f =&gt; x =&gt; f(f(x)(x))(f(x)(x))

console.log(Y('g'))

 

A Lambda Calculus Program:

 

// <https://glebec.github.io/lambda-talk/>

fib = n =&gt; n(f =&gt; a =&gt; b =&gt; f(b)(add(a)(b)))(T)(n0)(n1)

console.log(jsnum(fib(n4)))

 

(define fib (lambda (n) (n ((lambda (f a b) (f (b (add a b))) (kite 0 1))))))

 

 

Factorial?

 

; n containing a function f will invoke krestel false,

; otherwise if n is a with no f calls, it will select true

(define is0 (lambda (n) (n (krestel false) true)))

 

; <https://courses.cs.washington.edu/courses/cse341/03au/slides/Lambda/tsld015.htm>

(define factorial ((lambda (x) (lambda (y) x (y y)) (lambda (y) x (y y)))  
(lambda (f) (lambda (n) (is0 n) 1 (mult n (f (pred n)))))))

 

 

&lt;--- y combinator!

 

Can you reduce all the function calls to their lambda states for substitution?

 

// <https://gist.github.com/TeWu/96cc5a49fce3fa0c6defcf5c50bdf309>

 

///

/// Factorial function in lambda calculus - implemented in JS

///

 

//// Lambda calculus

// zero = λs.λz.z

// succ = λn.λs.λz.s (n s z)

// mult = λn.λm.λs.m (n s)

// pred = λn.λf.λx.n(λg.λh.h (g f))(λu.x)(λu.u)

// minus = λn.λm.m pred n

// true = λt.λf.t

// false = λt.λf.f

// not = λp.p false true

// and = λp.λq.p q p

// is\_zero = λn.n (λx.false) true

// leq = λn.λm.is\_zero (minus n m)

// Z = λf.( λx.(x x) λx.f(λy.x x y) )

// factorial = Z (λf.λn.( (leq n zero) (succ zero) (λy.(mult n (f (pred n))) y) ))

 

 

//// Lambda JavaScript - Only lambda definition and application allowed (recursion is not allowed - "const"s used for clarity, but every expression should be inlinable)

const zero = s =&gt; z =&gt; z

const succ = n =&gt; s =&gt; z =&gt; s(n(s)(z))

const mult = n =&gt; m =&gt; s =&gt; m(n(s))

const pred = n =&gt; f =&gt; x =&gt; n(g =&gt; h =&gt; h(g(f)))(u =&gt; x)(u =&gt; u)

const minus = n =&gt; m =&gt; m(pred)(n)

const bTrue = t =&gt; f =&gt; t

const bFalse = t =&gt; f =&gt; f

const and = p =&gt; q =&gt; p(q)(p)

const isZero = n =&gt; n(x =&gt; bFalse)(bTrue)

const leq = n =&gt; m =&gt; isZero(minus(n)(m))

const Z = f =&gt; (x =&gt; x(x))(x =&gt; f(y =&gt; x(x)(y)))

 

const factorial = Z(f =&gt; n =&gt;

leq(n)(zero)(

succ(zero)

)(

(y =&gt;

mult(n)(f(pred(n)))(y)

)

)

)

 

const factorial\_inlined = (f =&gt; (x =&gt; x(x))(x =&gt; f(y =&gt; x(x)(y))))(f =&gt; n =&gt;

(n =&gt; m =&gt; (n =&gt; n(x =&gt; t =&gt; f =&gt; f)(t =&gt; f =&gt; t))((n =&gt; m =&gt; m(n =&gt; f =&gt; x =&gt; n(g =&gt; h =&gt; h(g(f)))(u =&gt; x)(u =&gt; u))(n))(n)(m)))(n)(s =&gt; z =&gt; z)(

(n =&gt; s =&gt; z =&gt; s(n(s)(z)))(s =&gt; z =&gt; z)

)(

(y =&gt;

(n =&gt; m =&gt; s =&gt; m(n(s)))(n)(f((n =&gt; f =&gt; x =&gt; n(g =&gt; h =&gt; h(g(f)))(u =&gt; x)(u =&gt; u))(n)))(y)

)

)

)

 

const factorial\_inlined\_minified = (f =&gt; (x =&gt; x(x))(x =&gt; f(y =&gt; x(x)(y))))(f =&gt; n =&gt; (n =&gt; m =&gt; (n =&gt; n(x =&gt; t =&gt; f =&gt; f)(t =&gt; f =&gt; t))((n =&gt; m =&gt; m(n =&gt; f =&gt; x =&gt; n(g =&gt; h =&gt; h(g(f)))(u =&gt; x)(u =&gt; u))(n))(n)(m)))(n)(s =&gt; z =&gt; z)((n =&gt; s =&gt; z =&gt; s(n(s)(z)))(s =&gt; z =&gt; z))((y =&gt; (n =&gt; m =&gt; s =&gt; m(n(s)))(n)(f((n =&gt; f =&gt; x =&gt; n(g =&gt; h =&gt; h(g(f)))(u =&gt; x)(u =&gt; u))(n)))(y))))

 

 

//// JavaScript

const to\_church\_int = n =&gt; n &lt;= 0 ? zero : succ(to\_church\_int(n - 1))

const unchurch\_int = n =&gt; n(m =&gt; m + 1)(0)

 

console.log(unchurch\_int(factorial\_inlined\_minified(to\_church\_int(5)))) //=&gt; 120

 

// As ONE big lambda

console.log((n =&gt; (n =&gt; n(m =&gt; m + 1)(0))((f =&gt; (x =&gt; x(x))(x =&gt; f(y =&gt; x(x)(y))))(f =&gt; n =&gt; (n =&gt; m =&gt; (n =&gt; n(x =&gt; t =&gt; f =&gt; f)(t =&gt; f =&gt; t))((n =&gt; m =&gt; m(n =&gt; f =&gt; x =&gt; n(g =&gt; h =&gt; h(g(f)))(u =&gt; x)(u =&gt; u))(n))(n)(m)))(n)(s =&gt; z =&gt; z)((n =&gt; s =&gt; z =&gt; s(n(s)(z)))(s =&gt; z =&gt; z))((y =&gt; (n =&gt; m =&gt; s =&gt; m(n(s)))(n)(f((n =&gt; f =&gt; x =&gt; n(g =&gt; h =&gt; h(g(f)))(u =&gt; x)(u =&gt; u))(n)))(y))))((f =&gt; (x =&gt; x(x))(x =&gt; f(y =&gt; x(x)(y))))(f =&gt; n =&gt; n &lt;= 0 ? s =&gt; z =&gt; z : (n =&gt; s =&gt; z =&gt; s(n(s)(z)))(f(n - 1)))(n))))(5))

 

 

// Sources / See also:

// Church encoding @ Wikipedia: <https://en.wikipedia.org/wiki/Church_encoding>

// Programming with Nothing by Tom Stuart: <https://youtu.be/VUhlNx_-wYk>

// Y Not- Adventures in Functional Programming by Jim Weirich: <https://youtu.be/FITJMJjASUs>

