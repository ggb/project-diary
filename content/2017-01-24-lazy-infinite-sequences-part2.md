Title: Lazy, infinite sequences (Part 2)
Date: 2017-01-22 11:20
Category: Programming



    :::javascript
    function* ones() {
      while(true) {
        yield 1
      }
    }

    :::javascript
    const take = function(gen, n) {
      if(n == 0) return
      return [gen.next().value, ...take(gen, n-1)]
    }

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

    :::javascript
    const f = fibs()