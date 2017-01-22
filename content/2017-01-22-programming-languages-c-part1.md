Title: Programming Languages C (Part1)
Date: 2017-01-22 11:20
Category: Programming

The last two days I've continued the coursera course on programming languages by Dan Grossmann. I've finished the first two parts (A and B) last year, but wasn't able to continue with the last part because of reasons. The course offers an in-depth discussion of programming language concepts, a main focus is on the key differences of functional and object oriented programming. 

Current topic of the course is subtyping, but I had a lack of concentration and found it more interesting to play around with lazy, infinite sequences (a topic of part B). First I tried to implement them in Elm, but the type checker went nuts (which is - in fact - not surprising). 

So, why not try the same with JavaScript (ES6 or TypeScript)?

But first things first: A lazy sequence is something, that is produced on demand and not up front. It is infinite because it contains a recipe to produce - in theory - a sequence of infinte length. It is possible to achieve the same effect with [generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*), but I chose to work with [thunks](https://en.wikipedia.org/wiki/Thunk) instead. A thunk is an expression that is wrapped inside an  anonymous function to prevent its evaluation.

    :::javascript
    const example = () => 2 + 3

Here, the value of 2 + 3 = 5 is not evaluated until you call the function.

    :::javascript
    example()
    // 5

To create an infinite sequence we need a thunk, that returns a tuple of a value and another thunk. This second thunk contains the recipe to create more value-thunk-tuples... recursion, you know? So, let's create the infinite sequence of '1's:

    :::javascript
    const ones = () => [1, ones]

In this case the first value is a '1'. Whats about our thunk-recipe to create more '1's? We know, that it should return again a tuple with a '1' and a thunk, that, if called returns a tuple with a '1' and a thunk-recipe. We already know, how that should look like: It is our ones-function! That is why the second thing in our tuple is the ones-function (recursion...).

Call it like:

    :::javascript
    ones()[0]
    // 1
    ones()[1]()[0]
    // 1
    ones()[1]()[1]()[0]
    // 1

Okay, thats hard to read and write and it would be much nicer to have a function, that gives us the first n values of the infinite sequence: 

    :::javascript
    const unfold = function(fn, n) {
      if(n == 0) return [] // 1
      const [s,t] = fn()  // 2
      return [s, ...unfold(t, n - 1)] // 3
    }

This function takes two arguments, a thunk and a number n. We then (1) check if n equals zero. In this case there is nothing to do and we return an empty array. Otherwise (2) we call our thunk, which returns a tuple of a value and the thunk to create our next value-thunk-tuple. We decompose this tuple with destructuring. The function returns (3) an array, where the first element is the current value, the rest is a recursive call to unfold with the new thunk and n decremented. The crazy spread-syntax is just prepending s to the head of the list, so another way to write this is [s].append(unfold(t, n-1)) (in fact this is much faster).

    :::javascript
    unfold(ones, 10)
    // [1, 1, 1, 1, 1, 1, 1, 1, 1, 1]

Unfold is a nice helper-function to check if our sequences work as expected. What else is possible? Let's create a simple function that is the infinite sequence of alternating a's and b's.

    :::javascript
    const abs = () => ['a', () => ['b', abs]]
    unfold(abs, 7)
    // ["a", "b", "a", "b", "a", "b", "a"]

What if we need an additional parameter, e. g. we would like to cycle through an existing array? The trick is to wrap our thunk into a higher-order function:

    :::javascript
    const cycle = (arr) => () => {
      const [head, ...rest] = arr
      return [head, cycle(rest.concat(head))]
    }

Here, the thunk is inside the function that takes the arr-parameter. The thunk itself unpacks the arr into head and rest. The head is the value of the returned tuple. The new thunk is a call to cycle, with a new list, where the head is appended to the rest.

To create our initial thunk we need to call our cycle-function with a list.

    :::javascript
    const dwarves = cycle(["thorin","balin","gloin"])

Now it is possible to feed our dwarves into the unfold-function:

    :::javascript
    unfold(dwarves, 9)
    // ["thorin", "balin", "gloin", "thorin", ...]

To create a sequence of numbers from x to y it is necessary to create a wrapped thunk, too. 

    :::javascript
    const nums = (n) => () => [n, nums(n + 1)]
    unfold(nums(1), 20)
    // [1, 2, 3, 4, 5, 6, 7, ...]

Of course it is possible to use more than one parameter for our outer function. How about the fibonacci series?

    :::javascript
    const fibs = (a, b) => () => [a+b, fibs(b, a + b)]
    unfold(fibs(0, 1), 20)
    // [1, 2, 3, 5, 8, 13, 21, ...]

At first this might be a little bit mind bending, but I hope the examples help to understand the underlying principle. 