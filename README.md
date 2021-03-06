HHVM on Travis-CI
=================

[![Build
Status](https://travis-ci.org/willdurand/hhvm-on-travisci.png?branch=master)](https://travis-ci.org/willdurand/hhvm-on-travisci)

Does your PHP project behave as expected on [HHVM](http://www.hiphop-php.com/)?
Do you want to give HHVM a try? If you are using [Travis-CI](https://travis-ci.org)
to run your test suite, then **this** is for you!

Here is how your `.travis.yml` file should look like:

``` yaml
language: php

php:
    - 5.4
    - 5.5

env:
    - HHVM=true
    - HHVM=false

before_install:
    - PHP_VERSION=`phpenv global`
    #
    # HHVM + PHP 5.5 = <3
    #
    # 1. We install HHVM
    # 2. We configure a script to run the test suite with HHVM
    #
    - if [[ "$HHVM" == true && "5.5" == "$PHP_VERSION" ]] ; then sudo bin/install_hhvm; export SCRIPT="hhvm --version"; fi
    #
    # HHVM but not PHP 5.5, we properly exit
    #
    - if [[ "$HHVM" == true && "5.5" != "$PHP_VERSION" ]] ; then export SCRIPT="exit"; fi
    #
    # Not HHVM? We configure a script to run the test suite using PHP
    #
    - if [[ "$HHVM" == false ]] ; then export SCRIPT="php --version"; fi

script: "$SCRIPT"
```

In the example above, it runs 4 jobs, but only 3 are useful: PHP 5.4, PHP 5.5
and HHVM.

**Note:** You need PHP 5.5 as Composer has a few issues while running on HHVM,
so it is ok to install HHVM in a PHP 5.5 environment. It allows you to run
Composer using PHP, and to run your test suite using HHVM.

Here is a **real example**:
[willdurand/JsonpCallbackValidator](https://github.com/willdurand/JsonpCallbackValidator/commit/b2250f22444b4660fc42357a753f7974024823b1).
It takes less than one minute to install HHVM, so it is not that bad :)

William.
