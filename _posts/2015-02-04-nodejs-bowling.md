---
layout: post
title:  "Node.js Bowling Scorecard"
date:   2015-02-04
categories: nodejs recursion bowling
---

[Here](https://github.com/henrygarner/BowlingNode) is an implementation of a linearly recursive bowling scorer following the rules from [Wikipedia](http://en.wikipedia.org/wiki/Ten-pin_bowling#Rules_and_regulations).

The main entrypoint is the `Game.score()` function which accepts rolls from one player as a sequence of arguments. The `Game.score()` function delegates work to the recursive `Game._scorer()`. This iterates over all provided rolls keeping track of any bonuses which are due.

```javascript
Game._scorer = function(previousRoll, shot, score, rolls) {
    var roll = rolls[0];
    if (roll === undefined) {
        return score;
    } else {
        var bonus = this._bonus(previousRoll, shot, roll, rolls);
        return this._scorer(roll,
                            shot.next(roll),
                            score + roll + bonus,
                            rolls.slice(1));
    }
};
```

The variadic `Game.score()` function is implemented using the `arguments` object.

```javascript
Game.score = function() {
    var init = new Shot(0, 0);
    return this._scorer(0, init, 0, [].slice.call(arguments));
};
```

#### To Run Tests ####

```bash
npm test
```

## On Recursion

For more information on recursion, consult the **Factorial** example from [SICP](https://mitpress.mit.edu/sicp/full-text/sicp/book/node15.html).
