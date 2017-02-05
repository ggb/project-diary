Title: Lazy, infinite sequences (Part 3)
Date: 2017-02-05 11:20
Category: Programming

What else is great about our lazy, infinite sequences? They are composable! That means that you can use functions to combine sequences with other sequences or functions to get new sequences. That's a quite powerful feature.

Before we dive into this topic, look at my redefined unfold-function, now called take: 

    :::javascript
    const take = function(fn, n) {
      return range(n).reduce(([fn, l], ele) => {
        const [s, t] = fn()
        return [t, [...l, s]]
      }, [fn, []])
    }

It now works in terms of a reduce and not only returns the list of resulting values, but also the last recipe-function. So it is easily possible to continue at the last calculation next time you need new values.

Back to composability: First we need some helper functions and streams to later check our new super powers. We create a predicate (for even numbers), something we can use in a map, as well as two streams for the natural numbers and the fibonacci sequence.

    :::javascript
    const even = n => n % 2 === 0
    const inc = n => n + 1

    const n = nums(1)
    const f = fibs(0,1)

If you have followed along and already studied [Part 1](./lazy-infinite-sequences-part-1.html) and [Part 2](./lazy-infinite-sequences-with-generators.html) of this series it comes with no surprise how our functions to combine and create new sequences are defined. In case of the combine-function we take two sequences in, call them and get values for both out, as well as a new recipe for the next values. We combine the value into an array and return this array as our result. The next thunk is a call to combine with the newly created functions f1 and f2.

    :::javascript
    const combine = (s1, s2) => () => {
      const [v1, f1] = s1()
      const [v2, f2] = s2()
      return [[v1, v2], combine(f1, f2)]
    }
    take(combine(n, f), 20)[1]
    // [[1, 1], [2, 1], [3, 2], ... ]

Map and filter both take a sequence and a function. They use their function to manipulate the sequence: In case of map, the function is applied to the returning value. 

    :::javascript
    const map = (s, f) => () => {
      const [v, fn] = s()
      return [f(v), map(fn, f)]
    }
    take(map(n, inc), 20)[1]
    // [2, 3, 4, 5, ... ]

In case of filter, only those values are returned which pass the truth test of the given predicate-function. 

    :::javascript
    const filter = (s, p) => () => {
      let [v, fn] = s()
      while(!p(v)) {
        [v, fn] = fn()
      }
      return [v, filter(fn, p)]
    }
    take(filter(n, even), 20)[1]
    // [2, 4, 6, 8, ... ]

Next we'll look at a beast of a function called *zip*. It takes in a function and a arbitrary large (or small) number of sequences. For every request for new values it generates a value for every sequence and a new recipe for the next value. It then fills all values into the function zip was called with and returns this as the new value for the zip-sequence. 

This allows to combine a huge number of streams in literally every way you can think of.

    :::javascript
    const zip = (f, ...args) => () => {
      const [values, next] = args.reduce(
        ([vl, fl], fn) => {
          const [v, fun] = fn()
          return [[...vl, v], [...fl, fun]]
        }, [[], []])
      return [f(...values), zip(f, ...next)]
    }

    const helper = (...args) => args.join(" - ")
    take(zip(helper, n, f, filter(n, even)), 20)[1]
    // ["1 - 1 - 2", "2 - 1 - 4", "3 - 2 - 6", ... ]

Of course there are other and more higher-order functions to use on sequences, but I think this is enough for a start. 

Next time I'll look at how you can define and use lazy sequences in other languages and why (and how) they are useful in the real world.