# Unit Testing Jenkins Pipelines, Pt. 1

_How to get started writing your own unit tests for Jenkins Pipelines_

## Intro

### Why do I want to do this?

You have a software project with potentially thousands and thousands of lines of
code, it has its own fully functional unit test suites, and you can be
relatively sure that if your software builds and passes tests, that it'll run
successfully and do what it's supposed to do.

You have a Jenkins system that faithfully builds your software, tests it, and
automatically deploys it when it passes tests.  If you've been doing this for
long enough, you're using pipeline code rather than UI-built pipelines.  If you
haven't switched over to this (Pipeline DSL), then you should.  Like now.

Your software project has its own test suite.  Your (Jenkins) pipelines, while
they run in an off the shelf software product (Jenkins), the pipelines
themselves are still your own code and just as integral to your project's
well-being.  **Why would you _not_ want to also test this software?**

The ordinary build and test cycle for a pipeline is to write pipeline code,
trigger a build, wait for results, and then repeat until the pipeline does what
you need it to do.  No sane software developer would tell you this is a good
development cycle.  The feedback loop gets increasingly long as your pipeline
gets longer and as your build system gets busier, and often this build system is
in-use for software development while pipeline development is also occurring.
The result is unhappy ops and pipelines that are more often duck tape,
workarounds, and heroics than the well oiled machine that it should be.

Pipeline unit testing can change this.

_I'll get off my soapbox now..._

### So I'm sold, how do I do this?

It's really not that complicated.  You just need to...

1. Use a testing framework
2. Write unit tests
3. Have a better pipeline development cycle
4. Live a happier and more fulfilled life

This guide only covers #1 and #2, but hopefully you'll get #3 and #4 as a
result.  The unit testing framework _for Jenkins pipelines_ consists of a test
harness ([`jenkins-pipeline-unit`](https://github.com/jenkinsci/JenkinsPipelineUnit))
and a test suite runner.  This guide focuses on using JUnit for the actual
testing.

It also helps to have an IDE on hand that can handle Groovy.  IntelliJ IDEA does
this quite well, and this post focuses on using IntelliJ to handle pipeline
development.

## Getting Started

### IDE Setup

* JDK
* Groovy SDK

### Gradle setup

_`build.gradle`_
```groovy
apply plugin: "groovy"

repositories {
    mavenCentral();
    maven { url 'https://repo.jenkins-ci.org/releases/' }
}

sourceSets {
    main {
        groovy.srcDirs = ["source/main/groovy"]
    }

    test {
        groovy.srcDirs = ["source/test/groovy"]
    }
}

dependencies {
    implementation "org.codehaus.groovy:groovy-all:2.4.5"
    testImplementation "junit:junit:4.12"
    testImplementation "com.lesfurets:jenkins-pipeline-unit:1.3"
}
```

This is a basic build file that can be used to _just_ run unit tests on pipeline
files.  As your library grows, you may need to add additional dependencies that
your pipelines need to run, or that your test suite needs to run tests.

If you plan to use the provided source repo to test, be sure to also install the
gradle wrapper...  While not strictly necessary, when working on a team, this
helps to make the project more portable across workstations.

```bash
gradle wrapper --gradle-version 6.2.2
```

### The test harness (`jenkins-pipeline-unit`)

This is where most of the magic happens.  This is a suite of classes that work
together to enable you to "run" Jenkins pipelines without running them.  This
will test the groovy code, but, for instance, it won't actually execute shell
commands.  All of these calls are mock-able.  Once the pipeline has run, some
data values are available that allow you to inspect the decision logic and
results from a call stack view, and thus test this data to ensure that your
pipeline is doing what you expect.



<!--
Source code to view / examples
Gradle setup
Dependency overview
Groovy concepts used
LesFurets
-->

## Basic tests

<!--
Run a pipeline
Inspect the call stack
Mock variables & function calls
-->

## Advanced call stack inspection

## Outro

<!--
Advanced pipeline concepts
Subclassing and extending the base pipeline test class
Testing a pipeline from your project
-->
