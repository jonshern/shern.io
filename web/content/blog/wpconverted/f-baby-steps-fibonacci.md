---
title: 'F# Baby Steps - Fibonacci'
date: Wed, 27 Apr 2011 04:23:40 +0000
draft: false
tags: ['code']
---

F# is the kind of language that can really energize someone who is excited about just sitting down and powering out a solution.

Here is an example of how to sum some fibonacci numbers.

let rec fibonacci n = 

    if n < 2 then 1 

    else fibonacci (n-2) + fibonacci(n-1)

//create a recursive function called fibonacci

//that takes the parameter n 

// if n < 2 then 1  so this is the base case.

//else fib (n - 2) + fib (n - 1)

printfn "fibonacci = %A" (fibonacci 10)