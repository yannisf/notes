# Learning Scala

## How to learn Scala

### Courses

- Coursera:[Functional Programming in Scala Specialization](https://www.coursera.org/specializations/scala)
- edx: [Programming Reactive Systems](https://www.edx.org/course/programming-reactive-systems)
- EPFL:
  - [Functional Programming Principles in Scala](https://courseware.epfl.ch/courses/course-v1:EPFL+progfun1+2018_T1/about)
  -[Functional Program Design in Scala](https://courseware.epfl.ch/courses/course-v1:EPFL+progfun2+2018_T1/about)
  - [Big Data Analysis with Scala and Spark](https://courseware.epfl.ch/courses/course-v1:EPFL+scala-spark-big-data+2018-T1/about)
  - [Programming Reactive Systems](https://courseware.epfl.ch/courses/course-v1:EPFL+scala-reactive+2018_t3/about)
- Pluralsight: [Thinking Functionally in Scala](https://app.pluralsight.com/library/courses/scala-thinking-functionally/table-of-contents)

### Practice

- Hakerrank: [Functional Programming](https://www.hackerrank.com/domains/fp)

### Cheatsheets

- [Scalacheat](https://docs.scala-lang.org/cheatsheets/index.html)
- [ProgFun](https://github.com/lampepfl/progfun-wiki/blob/gh-pages/CheatSheet.md)

### Documentation

- [Twitter Scala School](http://twitter.github.io/scala_school/)


## sbt

Simple Build Tool or Scala Build Tool

### Learn sbt

- [sbt by example](https://www.scala-sbt.org/1.x/docs/sbt-by-example.html)

#### Initialize a project

```
$ mkdir mysbtproject
$ cd mysbtproject
$ touch build.sbt
$ sbt
```

Create a hello world project

```
$ sbt new sbt/scala-seed.g8
```

### Commands

- `about`: Information on sbt installation
- `console`: enter scala REPL with all project code loaded
- `compile`: compiles code
- `test`: run tests
- `run`: run application code
- `submit email token`: submits the assignment to the grader

Prepending  command with ~ watches the files.

Scopes: Project / Configuration / Task / Key
Configuration := Compile | Test | Runtime
Task := compile | package | run | ...
Key := sourceDirectories | scalaOptions ...

## scala

### Noteworthy

- Classes in Scala cannot have static members
- Objects in Scala are like classes, but for every object definition there is only one single instance
- In Scala, everything can be imported, not only class names
- The name of a Scala source file can be chosen freely
- Tail recursive function oscilate between expression and the function call when evaluated

- Anonymous functions are also called function literals

        def cube(x: Int): Int = x * x * x
        (x: Int) => x * x * x

- The type of the parameter can be omitted if can be inferred from the context
- When an object is created with the same name in the same file with a class, it is called a companion object. (`apply` method magic)
- In pattern matching the case statements use the `unapply` method to deconstruct the object. Returns an `Option`.
- Traits cannot have parameters
- def in traits can be overriden either with def or val. val can only be overriden with val.
- If you want to to extract data, create an object and an `unapply` and apply a pattern match.
- a pattern matching anonymous function is always of type `PartialFunction`
- Nothing is a subtype of any type
- In for comprehensions only the first expression needs to be an extraction/iteration (<-).
- **Structural types** use reflection. Use with care and only when needed.
- In case classes **copy** helps to create a clone with immutable values overriden
- : - type ascription, a hint that helps compiler to understand, what type does that expression have

# `map`/`flatMap` for `Option`s and `Future`s

`map` and `flatMap` operate on the enclosed value (they unwrap the `Option`/`Future`), but

- `map`: wraps it again after returning (without intervention)
- `flatMap`: does not wrap it, but expects the code to return a wrapped value

**NOTE**: Use should use `flatMap` to avoid creating an `Option[Option]` or `Future[Future]`
**NOTE**: When operating on a collection of `Option`s, `flatMap` will only map defined `Option`s
**NOTE**: `flatMap` is equivalent with `map` and `flatten`
