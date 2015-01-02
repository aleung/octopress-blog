---
layout: post
title: "Getting start with sbt"
date: 2014-12-26 17:17:50 +0800
comments: true
tags: 
- SoftwareDev
- Scala
---

A shortened sbt tutorial for beginner and quick reference.

## Directory structure

```
lib/
project/
  Build.scala
  plugins.sbt
src/
  main/
    resources/
    scala/
    java/
  test/
built.sbt
```

Source code can be placed in the project's base directory. But usually people organize them into directory structure.

sbt by default uses the same directory structure as Maven for source files, all under `src/`.

sbt build definition files include `build.sbt` in project's base drectory and other `.sbt` or `.scala` files in `project/` subdirectory.

The `project` directory _is another project embedded_ which knows how to build the outer project. So it may have its `build.sbt` and `project` directory structure, which is recursive. _The build definition is an sbt project._

## Running

Common sbt commands:

- clean
- compile
- test
- testOnly &lt;testcase&gt;*
- run &lt;argument&gt;*
- package
- reload

sbt can ran in interactive mode or batch mode. In batch mode, specifying a space-seperated list of commands. For commands that take arguments, enclosing the command and arguments in quotes.

``` sh
$ sbt clean compile "testOnly TestA TestB"
```

Prefixing a command with `~` will make the command run when any source files change. Press enter to stop watching for changes.

```
> ~ compile
```

## Build definition

### .sbt build definition basic

#### Project

`build.sbt` defines one or more `Project`s, which hold a list of Scala expressions called `settings`.

Top-level objects and classes are not allowed in `build.sbt`. Those should go in the `project/` directory as full Scala source files.

``` scala
lazy val commonSettings = Seq(
  organization := "com.example",
  version := "0.1.0",
  scalaVersion := "2.11.4"
)

lazy val root = (project in file(".")).
  settings(commonSettings: _*).
  settings(
    name := "hello"
  )
```

On the left, `name`, `version` and `scalaVersion` are _keys_. Keys have a method called `:=`, which returns a `Setting[T]`, where `T` is the value type.

#### Keys

There are three flavors of key: `SettingKey[T]`, `TaskKey[T]` and `InputKey[T]`.

A `TaskKey[T]` is said to define a _task_. 

A `InputKey[T]` defines a _input task_, which parses user input and produce a task to run.

For a setting, the value will be computed once at project load time. For a task, the computation will be re-run each time the task is executed.

Built-in keys are fields in object `sbt.Keys`, which are implicitly imported so can be directly referred.

Custom keys may be defined with their respective creation methods: `settingKey`, `taskKey`, and `inputKey`. 

``` scala
lazy val hello = taskKey[Unit]("An example task")

lazy val root = (project in file(".")).
  settings(
    hello := { println("Hello!") }
  )
```

### Bare .sbt build definition

_Bare .sbt build definition_ is an old style which is not recommended to use.

Bare .sbt build definition doesn't explicitly define a `Project`, it implicitly deines one based on the location of the `.sbt` file. The `.sbt` file consists of a list of `Setting[_]` expressions.

Before sbt 0.13.7, settings must be separated by blank lines.

## Library dependencies

To add unmanaged dependencies, simplily drop jar files in `lib/`.

Managed dependencies are specified by `libraryDependencies` key in `build.sbt`.

``` scala
libraryDependencies += groupID % artifactID % revision % configuration
```

If you use double %% between the groupID and artifactID, sbt will add your project’s Scala version to the artifact name.

Assuming the scalaVersion for your build is 2.11.1, the following adds `org.scala-tools:scala-stm_2.11.1:0.3`:

``` scala
libraryDependencies += "org.scala-tools" %% "scala-stm" % "0.3"
```

By default, if omitted, configuration is "compile". A common used value is "test".

sbt uses Apache Ivy to implement managed dependencies. Advanced usage of revision and configuration can follow Ivy document.

sbt uses the standard Maven2 repository by default. To add additional repository, add a `resolver`:

``` scala
resolvers += "Local Maven Repository" at "file://"+Path.userHome.absolutePath+"/.m2/repository"

resolvers += "Sonatype OSS Snapshots" at "https://oss.sonatype.org/content/repositories/snapshots"
```

## Multiple projects

Multiple projects can be grouped under one project. 

``` scala
lazy val root = (project in file(".")).
  aggregate(util, core)

lazy val util = project

lazy val core = project
```

Each sub-project has its own directory. In above example the directory name is same as the project's ID. The following is a more explicit way: 

``` scala
lazy val core = project in file("core")
```

If a project is not defined for the root directory in the build, sbt creates a default one that aggregates all other projects in the build. 

A project may depend on other projects:

``` scala
lazy val core = project.dependsOn(util, api)
```

`foo dependsOn(bar)` means that the `compile` configuration in foo depends on the `compile` configuration in bar. You can declare other configuraiton dependency, for example, `dependsOn(bar % "test->test;compile->compile")`.

## Using plugins

Declares the plugin dependency in `project/plugins.sbt`. The file name can be different, you may create one sbt file for one plugin.

``` scala
addSbtPlugin("com.typesafe.sbt" % "sbt-site" % "0.7.0")
```
