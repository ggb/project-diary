Title: Lazy, infinite sequences with Generators
Date: 2017-01-24 11:20
Category: Programming

I was curious how to implement the stuff from the [last post of this series](./lazy-infinite-sequences-part-1.html) with [generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*) (if you don't know what generators are or how to use them, there is a good [introduction by Kyle Simpson](https://davidwalsh.name/es6-generators)). It is easier than I thought. 

This is my *ones*-function: 

    :::javascript
    function* ones() {
      while(true) {
        yield 1
      }
    }

To actually run it, it is necessary to construct an iterator.

    :::javascript
    const o = ones()

    o.next().value;
    // 1

To mimic the behaviour of *unfold* you need something like:

    :::javascript
    const unfold2 = function(gen, n) {
      let result = []
      for(let i = 0; i < n; i++) {
        result.push(gen.next().value)
      }
      return result
    }

    unfold2(o, 7)
    // [1,1,1,1,1,1,1]

You may recognize that this code does not use recursion and looks much more imperative. That is true as I wasn't able to get a version with recursion running (if you know why that is, feel free to contact me).

*fibs* now looks like: 

    :::javascript
    function* fibs() {
      let a = 0
      let b = 1
      while(true) {
        let _a = a
        a = b
        b = _a + b
        yield a
      }
    }

    const f = fibs()
    
    unfold2(f, 20)
    // [1, 2, 3, 5, 8, 13, 21, ...]

So, to be quite honest, I like the *thunked*-version much more: It is nearer to my heart as it looks more functional and descriptive. I'm not particularly happy with this special new syntax, although you get quite fast used to it. 

The generator-version has one clear advantage: To calculate 1.000 numbers of the fibonacci series is almost 10x faster.