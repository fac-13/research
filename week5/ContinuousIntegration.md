# Continuous Integration (CI) & Travis 

## What

* A software development process in which all development work is integrated regularly & frequently and the code is automatically tested. 

* The development team does multiple small merges a day and they use an automatic verification system to check for problems


## Why

* Testing our work and making sure it is **working as expected** is the most important part of a project.
* **Test Early, Test Often** to spot "_integration issues_" _before its too late ..._
* Helps avoid chasing bugs in large merges
* Develop cohesive software more rapidly

## How

* Break tasks into small parts and merge multiple times per day, so you go in and out of your master fast and the code is not changing under your feet.
* This significantly reduces time-waste looking for bugs (if looking for a bug in your giant merge is like looking for a needle in a haystack, CI reduces the size of this hay stack)

#### but

* An automatic verification system should be in place:

### Steps:
1. copy current code base
2. make a change - pull request
3. Travis will run test automatically and continuously 
4. if pass - keep making changes
5. if fail -  must fix code
6. Travis will then update the code base accordingly 

## Travis CI

Why Travis: web based, free and integrates well with GitHub

* Build and test projects hosted on github open source
* Helps identify errors at early stage
* Trigger automatically when dev checks in coded into their repo



## TRAVIS DEMO

Link your Github account to Travis & enable Travis on your selected project. 

Add a .travis.yml file to your project and continue to develop (create branches, push to github, and create pull requests). Make sure it's in the root folder.

Your ```.travis.yml``` file should include:

```
language: node_js
node_js: "node"
```
If you don't want to receive Travis email notifications, also add: 

```
notifications:
- email:   false
```

Whenever you push a branch and create a PR, Travis will automatically run!!

### Failed Tests

![](https://i.imgur.com/YlrURfr.png)

### Why do you see two Travis builds? 

![](https://i.imgur.com/II5XyIq.png)


You can uncheck either building pushes or PRs in the TravisCI settings for the repository: ![settings SS](https://i.imgur.com/etJrSEZ.png)

The difference between them is:

-   `/push` builds for the current state of the branch you pushed to (as if you ran the tests on your local copy you just pushed),
-   `/pr` builds automerged state (as if you merged the PR and ran the tests on that, note: it won't run if the PR can't be automerged).



## Resources
[CI | wikipedia](https://en.wikipedia.org/wiki/Continuous_integration)

[Martin Fowler's Article on CI](https://www.martinfowler.com/articles/continuousIntegration.html)

