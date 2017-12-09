<p align="left"><img src="http://takenobu-hs.github.io/downloads/images/haskell-logo-s.png"/></p>

Syntax highlight for numeric literals of Haskell/GHC
====================================================


Introduction
------------

The GHC has several language extensions for numeric literals:
* `BinaryLiterals` (e.g. 0b1100)
* `HexFloatLiterals` (e.g. 0x1fff.b0ffp3)
* `NumericUnderscores` (e.g. 1_000_000)

Here is the latest specification about them.
 * https://github.com/ghc-proposals/ghc-proposals/blob/master/proposals/0009-numeric-underscores.rst#new-syntax-this-proposal

If you need syntax highlighting about them, you can use the following definitions in each editors etc.
I prepared two kinds of fast (approximated) version and exact version.

Contents
--------

* Vim
* Emacs
* Atom (and linguist)
  * language-haskell
* Visual Studio Code
* Pygments

[Vim][vim1]
-----------

#### Fast version
under construction

#### Exact version
under construction


[language-haskell][atom1] for [Atom][atom2] and [linguist][atom3]
-----------------------------------------------------------------

#### Fast version
It has already been officially implemented. Since [1.15.0][atom4]


#### Exact version
After making the following substitution, execute `make` command in the top directory.

Substituting `src/include/repository.coffee` as follows:
```diff
-xxx
+yyy
```

Substituting `src/include/util.coffee` as follows:
```diff
-xxx
+yyy
```


[Visual Studio Code][vscode1]
-----------------------------

#### Fast version
under construction

#### Exact version
under construction


[Pygments][pygments1]
---------------------

#### Fast version
I will not prepare. Let's use the exact version.

#### Exact version
Substituting `pygments/lexers/haskell.py` as follows:
```diff
-xxx
+yyy
```


[vim1]: https://github.com/vim

[atom1]: https://github.com/atom-haskell/language-haskell
[atom2]: https://github.com/atom/atom
[atom3]: https://github.com/github/linguist
[atom4]: https://github.com/atom-haskell/language-haskell/releases/tag/v1.15.0

[vscode1]: https://github.com/Microsoft/vscode

[pygments1]: http://pygments.org/

