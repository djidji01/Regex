<p align="center">
  <img src="https://static.igorora-project.net/Image/Hoa.svg" alt="Hoa" width="250px" />
</p>

---

<p align="center">
  <a href="https://travis-ci.org/igororaproject/regex"><img src="https://img.shields.io/travis/igororaproject/regex/master.svg" alt="Build status" /></a>
  <a href="https://coveralls.io/github/igororaproject/regex?branch=master"><img src="https://img.shields.io/coveralls/igororaproject/regex/master.svg" alt="Code coverage" /></a>
  <a href="https://packagist.org/packages/igorora/regex"><img src="https://img.shields.io/packagist/dt/igorora/regex.svg" alt="Packagist" /></a>
  <a href="https://igorora-project.net/LICENSE"><img src="https://img.shields.io/packagist/l/igorora/regex.svg" alt="License" /></a>
</p>
<p align="center">
  Hoa is a <strong>modular</strong>, <strong>extensible</strong> and
  <strong>structured</strong> set of PHP libraries.<br />
  Moreover, Hoa aims at being a bridge between industrial and research worlds.
</p>

# igorora\Regex

[![Help on IRC](https://img.shields.io/badge/help-%23igororaproject-ff0066.svg)](https://webchat.freenode.net/?channels=#igororaproject)
[![Help on Gitter](https://img.shields.io/badge/help-gitter-ff0066.svg)](https://gitter.im/igororaproject/central)
[![Documentation](https://img.shields.io/badge/documentation-hack_book-ff0066.svg)](https://central.igorora-project.net/Documentation/Library/Regex)
[![Board](https://img.shields.io/badge/organisation-board-ff0066.svg)](https://waffle.io/igororaproject/regex)

This library provides tools to analyze regular expressions and generate strings
based on regular expressions ([Perl Compatible Regular
Expressions](http://pcre.org)).

[Learn more](https://central.igorora-project.net/Documentation/Library/Regex).

## Installation

With [Composer](https://getcomposer.org/), to include this library into
your dependencies, you need to
require [`igorora/regex`](https://packagist.org/packages/igorora/regex):

```sh
$ composer require igorora/regex '~1.0'
```

For more installation procedures, please read [the Source
page](https://igorora-project.net/Source.html).

## Testing

Before running the test suites, the development dependencies must be installed:

```sh
$ composer install
```

Then, to run all the test suites:

```sh
$ vendor/bin/igorora test:run
```

For more information, please read the [contributor
guide](https://igorora-project.net/Literature/Contributor/Guide.html).

## Quick usage

As a quick overview, we propose to see two examples. First, analyze a regular
expression, i.e. lex, parse and produce an AST. Second, generate strings based
on a regular expression by visiting its AST with an isotropic random approach.

### Analyze regular expressions

We need the [`igorora\Compiler`
library](https://central.igorora-project.net/Resource/Library/Compiler) to lex, parse
and produce an AST of the following regular expression: `ab(c|d){2,4}e?`. Thus:

```php
// 1. Read the grammar.
$grammar  = new igorora\File\Read('igorora://Library/Regex/Grammar.pp');

// 2. Load the compiler.
$compiler = igorora\Compiler\Llk\Llk::load($grammar);

// 3. Lex, parse and produce the AST.
$ast      = $compiler->parse('ab(c|d){2,4}e?');

// 4. Dump the result.
$dump     = new igorora\Compiler\Visitor\Dump();
echo $dump->visit($ast);

/**
 * Will output:
 *     >  #expression
 *     >  >  #concatenation
 *     >  >  >  token(literal, a)
 *     >  >  >  token(literal, b)
 *     >  >  >  #quantification
 *     >  >  >  >  #alternation
 *     >  >  >  >  >  token(literal, c)
 *     >  >  >  >  >  token(literal, d)
 *     >  >  >  >  token(n_to_m, {2,4})
 *     >  >  >  #quantification
 *     >  >  >  >  token(literal, e)
 *     >  >  >  >  token(zero_or_one, ?)
 */
```

We read that the whole expression is composed of a single concatenation of two
tokens: `a` and `b`, followed by a quantification, followed by another
quantification. The first quantification is an alternation of (a choice betwen)
two tokens: `c` and `d`, between 2 to 4 times. The second quantification is the
`e` token that can appear zero or one time.

We can visit the tree with the help of the [`igorora\Visitor`
library](https://central.igorora-project.net/Resource/Library/Visitor).

### Generate strings based on regular expressions

To generate strings based on the AST of a regular expressions, we will use the
`igorora\Regex\Visitor\Isotropic` visitor:

```php
$generator = new igorora\Regex\Visitor\Isotropic(new igorora\Math\Sampler\Random());
echo $generator->visit($ast);

/**
 * Could output:
 *     abdcde
 */
```

Strings are generated at random and match the given regular expression.

## Documentation

The
[hack book of `igorora\Regex`](https://central.igorora-project.net/Documentation/Library/Regex)
contains detailed information about how to use this library and how it works.

To generate the documentation locally, execute the following commands:

```sh
$ composer require --dev igorora/devtools
$ vendor/bin/igorora devtools:documentation --open
```

More documentation can be found on the project's website:
[igorora-project.net](https://igorora-project.net/).

## Getting help

There are mainly two ways to get help:

  * On the [`#igororaproject`](https://webchat.freenode.net/?channels=#igororaproject)
    IRC channel,
  * On the forum at [users.igorora-project.net](https://users.igorora-project.net).

## Contribution

Do you want to contribute? Thanks! A detailed [contributor
guide](https://igorora-project.net/Literature/Contributor/Guide.html) explains
everything you need to know.

## License

Hoa is under the New BSD License (BSD-3-Clause). Please, see
[`LICENSE`](https://igorora-project.net/LICENSE) for details.
