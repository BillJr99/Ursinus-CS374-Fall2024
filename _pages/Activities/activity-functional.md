---
layout: activity
permalink: /Activities/Functional
title: "CS374: Programming Language Principles - Functional Programming with Scheme"
excerpt: "CS374: Programming Language Principles - Functional Programming with Scheme"

info:  
  goals: 
    - To explore the functional programming paradigm using Scheme
  models: 
    - model: |
        <script type="syntaxhighlighter" class="brush: scheme"><![CDATA[
        (define L (list 'a 'b 'c))
        (car L)
        (cdr L)
        
        (define x (+ 3 2))
        (+ x 5)
        
        (define add +)
        (add 3 2)
        ]]></script>
        <br>
        <script type="syntaxhighlighter" class="brush: scheme"><![CDATA[
        (define square
          (lambda(n)
            (* n n)
          )
        )

        (define pow
          (lambda(n k)
            (if (= k 0)
              1
              (* n (pow n (- k 1)))
            )
          )
        )
              
        (square (pow 5 3))
        ]]></script> 
        <br>
        <script type="syntaxhighlighter" class="brush: scheme"><![CDATA[
        (define sumlist 
          (lambda(L)
            (if (null? (cdr L))
              (car L)
              (+ (car L) (sumlist (cdr L))) 
            )    
          )  
        )

        (sumlist (list 1 2 3))
        ]]></script> 
        <br>
        <script type="syntaxhighlighter" class="brush: scheme"><![CDATA[
        (define largest
          (lambda(L)
            (if (null? (cdr L))
              (car L)
              (if (>= (car L) (largest (cdr L)))
                (car L)
                (largest (cdr L))
              )
            )  
          )
        )

        (largest (list 1 2 4 3))
        ]]></script>         
      title: "Declarative Languages - Functional"
      questions:
        - "What is a statement in Scheme?"
        - "What shared variables exist in this program?"
        - "What are some potential advantages of Functional Programming as a paradigm?" 
        - "How might you improve upon the implementation of the <code>largest</code> function?"  
    - model: |
        <script type="syntaxhighlighter" class="brush: scheme"><![CDATA[
        (define y
          (lambda(m x b)
            (+ (* x m) b)))
            
        (y 5 6 7)
        ]]></script>
        <br>
        <script type="syntaxhighlighter" class="brush: scheme"><![CDATA[
        (define v0 3)
        (define t 5)
        (define a 9.80665)

        (define square
          (lambda(n)
            (* n n)))
            
        (+ (* v0 t) (* 0.5 (* a (* t t))))

        (+ (* v0 t) (* 0.5 (* a (square t))))
        ]]></script>     
        <br>
        <script type="syntaxhighlighter" class="brush: scheme"><![CDATA[
        (define czr
          (lambda(l)
            (if (null? (cdr l))
              (car l)
              (czr (cdr l))
            )
          )
        )
        ]]></script>  
        <br>
        <script type="syntaxhighlighter" class="brush: scheme"><![CDATA[
        (define plusminus 
          (lambda(a b)
            (
              (lambda (x y) (list (+ x y) (- x y)))
              a b
            )
          )
        )

        (plusminus 6 2)
        ]]></script> 
        <br>
        <script type="syntaxhighlighter" class="brush: scheme"><![CDATA[
        (define L1 '(1 2 3))
        (define L2 '(4 5 6))
        (define L3 (map - L1 L2))
        (apply + L3)
        ]]></script>        
      title: "The Scheme Programming Language"
      questions: 
        - "How are function parameters handled in Scheme?  Are they passed by value or by reference?"        
        - "What is a function in Scheme?  How is it represented?"
        - "What does <code>czr</code> do in your own words?"
        - "Write a function to count the number of items in a list using a recursive call and a base case, using <code>czr</code> as a guide to traversing a list."
        - "Diagram the binding of the values in the call to <code>plusminus</code> to the anonymous lambda function."
        - "What is the result of the <code>map</code>/<code>apply</code> sequence?  What would happen if <code>map</code> were applied to only a single list?"
        - "In your own words, define tail recursion.  Do you see instances of tail recursion in these examples?  Draw a call stack for one of these examples."
        - "Compare and constrast closures and objects."
        - "Using the <code>pair?</code> directive, which returns <code>&#35;t</code> if the parameter is a nonempty list, add a check to one of these list recursion examples to ensure that <code>null</code> is returned if an empty list is passed."
        - "Write a function that accepts a list and an operator as parameters, such as addition.  Apply that operator to the whole list recursively; for example, if the operator is the addition operator, return the sum of the list.  If it is the multiplication operator, return the product of all items in the list."
  additional_reading:
    - title: "The Scheme Programming Language"
      link: "https://www.scheme.com/tspl3/"
    - title: "Closures in Scheme"
      link: "https://www.artificialworlds.net/presentations/scheme-03-closures/scheme-03-closures.html"
    - title: "QuickSort in Scheme"
      link: "https://riptutorial.com/scheme/example/10903/quicksort"
    - title: "Implementing Python as Syntax Rules for Racket"
      link: "https://github.com/pedropramos/PyonR/"
      
tags:
  - paradigms
  
---

