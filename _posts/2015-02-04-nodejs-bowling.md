---
layout: post
title:  "Node.js Bowling Scorecard"
date:   2015-02-04
categories: nodejs recursion bowling
---

[Here](https://github.com/henrygarner/BowlingNode) is an implementation of a linearly recursive bowling scorer following the rules from [Wikipedia](http://en.wikipedia.org/wiki/Ten-pin_bowling#Rules_and_regulations).

It does not use mutable state. Once created [values don't change](http://clojure.org/state).

The main entrypoint is the `Game.score()` function which accepts rolls from one player as a sequence of arguments. The `Game.score()` function delegates work to the recursive `Game._scorer()`. This iterates over all provided rolls keeping track of any bonuses which are due.

```javascript
Game._scorer = function(previousShot, rolls) {
    var roll = rolls[0];
    if (roll === undefined) {
        return previousShot.score;
    } else {
        var bonus = this._bonus(previousShot, roll, rolls);
        return this._scorer(previousShot.next(roll, bonus),
                            rolls.slice(1));
    }
};
```

The variadic `Game.score()` function is implemented using the `arguments` object.

```javascript
Game.score = function() {
    var init = new Shot(0, 0, 0, 0);
    return this._scorer(init, [].slice.call(arguments));
};
```

#### To Run Tests ####

```bash
npm test
```

## On Recursion

For more information on recursion, consult the **Factorial** example from [SICP](https://mitpress.mit.edu/sicp/full-text/sicp/book/node15.html).
