---
slideOptions:
  transition: slide
---

# TEST COVERAGE - A TEST FOR UR TESTS

<iframe src="https://giphy.com/embed/Rkis28kMJd1aE" width="480" height="317" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p></p>

---

## What is test coverage?



### First: 'Test coverage' === 'Code coverage'

Test coverage is a measure used to describe the **degree to which the source code of a program is tested** by a particular test suite.


Test coverage lets you know _exactly_ which lines of code you have written are "_covered_" by your tests (_i.e. helps you check if there is "dead", "un-used" or just "un-tested" code_)

---

### There are a number of coverage criteria, the main ones being:


-   **Function coverage** – Has each function (or [subroutine](https://en.wikipedia.org/wiki/Subroutine "Subroutine")) in the program been called?
-   **Statement coverage** – Has each [statement](https://en.wikipedia.org/wiki/Statement_(computer_science) "Statement (computer science)") in the program been executed?
-   **Branch coverage** – Has each branch of each control structure (such as in [_if_ and _case_ statements](https://en.wikipedia.org/wiki/Conditional_(programming) "Conditional (programming)")) been executed? For example, given an _if_ statement, have both the true and false branches been executed? Another way of saying this is, has every **[edge](https://en.wikipedia.org/wiki/Graph_theory "Graph theory")** in the program been executed?
-   **Condition coverage** (or predicate coverage) – Has each Boolean sub-expression evaluated both to **true** and **false**?

For example, consider the following C function:

```javascript
function  example(int x, int y)
{
    var z = 0;
    if ((x > 0) && (y > 0))
    {
        z = x;
    }
    return z;
}
```

---

-   If during this execution function _'example'_ was called at least once, then _**function coverage**_ for this function is satisfied.
-   _**Statement coverage**_ for this function will be satisfied if it was called e.g. as `example(1,1)`, as in this case, every line in the function is executed including `z = x;`.
-   Tests calling `example(1,1)` and `example(0,1)` will satisfy _**branch coverage**_ because, in the first case, both `if` conditions are met and `z = x;` is executed, while in the second case, the first condition `(x>0)` is not satisfied, which prevents executing `z = x;`.
-   _**Condition coverage**_ can be satisfied with tests that call `foo(1,0)` and `foo(0,1)`. These are necessary because in the first cases, `(x>0)` evaluates to `true`, while in the second, it evaluates `false`. At the same time, the first case makes `(y>0)` `false`, while the second makes it `true`.


### Condition coverage does not necessarily imply branch coverage. 
For example, consider the following fragment of code:

 ```javascript 
 if (a && b) {
 return c
 }
 ```


_**Condition**_ coverage can be satisfied by two tests:

-   `a=true`, `b=false`
-   `a=false`, `b=true`

However, this set of tests does not satisfy branch coverage since neither case will meet the `if` condition - and not test the _**branch**_ .

[Fault injection](https://en.wikipedia.org/wiki/Fault_injection "Fault injection") may be necessary to ensure that all conditions and branches of [exception handling](https://en.wikipedia.org/wiki/Exception_handling "Exception handling") code have adequate coverage during testing.

### More info

The [Wikipedia article]("https://en.wikipedia.org/wiki/Code_coverage") ontest coverage is actually very good  - worth checking out.


## Why is 'Code Coverage' or 'Test Coverage' Useful?


The degree of code coverage can be interpreted in two ways: 

1. If the suite of tests employed are considered to be as comprehensive as possible (if the develpment process has been truly 'test driven'), then poor code coverage during the testing phase would indicate that there is **redundant** code in the program. Ideally, there would be no circumstance in which this code could run, in which case it would never be the source of a bug. Even so, it would be better removed so that the program is as legible as possible. But it is more likely that the testing suite is not perfect and that there could be circumstances in which that code could execute and therefore, being untested, could result in bugs etc.
2. If the suite of tests are not assumed to be highly comprehensive then poor code coverage during the testing phase is an indicator that there will be **functionality** in the program that is not being tested for and that therefore, there are potential sources of bugs that will survive the test cycle. In this case por coverage gives an idea of what kinds of new tests need to be written.

Having 100% code coverage doesn't mean that every eventuality is tested for, only that all statements etc. are executed at least once. These could still produced bugs when run under different conditions, for example, if a numeric value is passed to the function as opposed to a string value or if 0 is passed instead of a natural number.



## What are Istanbul and nyc?

## Istanbul

Istanbul a JS code coverage analysis script you run when executing your unit tests. It runs in the command line and browser, and helps make code coverage more simple.

**PROS:**
- Computes statement, line, function and branch coverage with module loader hooks to transparently add coverage when running tests. 
- Supports all JS coverage use cases including:
    - unit tests
    - server side functional tests 
    - browser tests. 
- Built for scale.
- Works for ES5 and ES2015+(ES6)


We used Istanbul 1.0 API in out TDD workshop which has now been deprecated, ie. no longer being developed.
Info here: https://github.com/gotwarlost/istanbul/ (deprecated)

But don't worry, the **Istanbul 2.0 API** is now available and is being actively developed in the new [istanbuljs organization](https://github.com/istanbuljs), under the new project repo name **nyc**
 
<iframe src="https://giphy.com/embed/Ni4cpi0uUkd6U" width="480" height="313" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

https://istanbul.js.org/ - has tutorials and FAQ
Repo: https://github.com/istanbuljs


## nyc
Repo: [nyc](https://github.com/istanbuljs/nyc)

**nyc** is the command-line-client for Istanbul and works well with most JavaScript testing frameworks: [tap](https://github.com/tapjs/node-tap) *(Used by Tape)*.

**nyc** started as a separate package that handled applications that spawned subprocesses (don't ask us what this means).

It was decided to merge Istanbul and nyc projects into one, under the new Istanbul 2.0 API umbrella, the idea being that nyc would provide the the all-in-one, simple-to-use, command line interface for the Istanbul 2.0 API.

You should consider installing and using **nyc** for test coverage going forward. Demo to follow :smiley: 


## How would you use them to improve your testing?

You can improve your testing by generating a report of your current test coverage to see and determine if more tests should be added

Code coverage tools can help determine if you have extra functions/code that you are not using and can help declutter your codebase

There are two views for the coverage report:

**LCOV view**
![LCOV view](https://cdn-images-1.medium.com/max/1600/0*BlKQEGJ3qgJRrL_L.png)

**CLI view**
![CLI view](https://ariya.io/images/2013/05/istanbul_limits.png)

#### Extra Fun Fact: 

Atom has a package called LCOV-INFO that will highlight code coverage in your editor
![atom lcov-info](https://i.imgur.com/WHfwMrB.png)


## Using Istanbul/nyc to calculate your code coverage for the TDD workshop.

### Install nyc:

Instructions here:
https://github.com/istanbuljs/nyc
    
You can install nyc as a development dependency:

```shell
$ npm i nyc --save-dev
```
Add it to the test stanza in your package.json.
```shell
{
  "scripts": {
    "test": "nyc tape <test.js filename> | tap-spec "
  }
}
```
You can use is like the old Istanbul like so:

```shell
$ npm test
```

# LIVE DEMO TIME!
<iframe src="https://giphy.com/embed/5GoVLqeAOo6PK" width="480" height="375" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>



