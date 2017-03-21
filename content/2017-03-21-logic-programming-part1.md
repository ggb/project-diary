Title: Logic Programming (Part 1)
Date: 2017-03-21 11:20
Category: Programming

One of my goals for 2017 is to learn (more about) logic programming. 

As I recently noticed the great and famous *Structure and Interpretation of Computer Programs* contains a [chapter](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-29.html#%_sec_4.4) dedicated to this topic. Instead of solving the exercises in the chapter with the language introduced in the book, I used [Datalog](https://en.wikipedia.org/wiki/Datalog) in the [Racket-flavour](https://docs.racket-lang.org/datalog/index.html?q=datalog) to tinker around. Datalog is a query language that is heavily inspired by Prolog (the grandfather of all logic programming languages), but only contains a limited (sub-)set of features. 

[This gist](https://gist.github.com/ggb/0ed4d5be55b75dfd62075e9cbfd29483) contains my database as well as my solutions so far. I think I'll write more about Datalog later on as it is very interesting to compare it with [sparql](https://en.wikipedia.org/wiki/SPARQL). 