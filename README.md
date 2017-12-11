<p align="left"><img src="http://takenobu-hs.github.io/downloads/images/haskell-logo-s.png"/></p>

Syntax highlight for numeric literals of Haskell/GHC
====================================================


Introduction
------------

The GHC has several language extensions for numeric literals:
  * `BinaryLiterals` (e.g. 0b1100) Since [GHC 7.10][trac9224]
  * `HexFloatLiterals` (e.g. 0x1fff.b0ffp3) Since [GHC 8.4][trac13126]
  * `NumericUnderscores` (e.g. 1_000_000) Since [GHC 8.6][trac14473]

Here is the latest specification about them.
 * https://github.com/ghc-proposals/ghc-proposals/blob/master/proposals/0009-numeric-underscores.rst#new-syntax-this-proposal

If you need syntax highlighting about them, you can use the following definitions in each editors etc.
I prepared two kinds of fast version and exact version.
 * **Fast version**:
   Pattern matching is approximate, but the response of the editor is fast.
 * **Exact version**:
   Pattern matching is accurate. You can check the lexers on the editor in real time without compiling.

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
Substituting `syntax/haskell.vim` as follows:
```diff
-syn match   hsNumber		"\<[0-9]\+\>\|\<0[xX][0-9a-fA-F]\+\>\|\<0[oO][0-7]\+\>"
-syn match   hsFloat		"\<[0-9]\+\.[0-9]\+\([eE][-+]\=[0-9]\+\)\=\>"
+syn match   hsNumber		"\v<[0-9][0-9_]*>|<0[xX][0-9a-fA-F_]+>|<0[oO][0-7_]+>|<0[bB][01_]+>"
+syn match   hsFloat		"\v<[0-9][0-9_]*%(\.[0-9_]+%([eE][-+]?[0-9_]+)?>|[eE][-+]?[0-9_]+)>|<0[xX][0-9a-fA-F_]+%(\.[0-9a-fA-F_]+%([pP][-+]?[0-9_]+)?>|[pP][-+]?[0-9_]+)>"
```

#### Exact version
Substituting `syntax/haskell.vim` as follows:
```diff
-syn match   hsNumber		"\<[0-9]\+\>\|\<0[xX][0-9a-fA-F]\+\>\|\<0[oO][0-7]\+\>"
-syn match   hsFloat		"\<[0-9]\+\.[0-9]\+\([eE][-+]\=[0-9]\+\)\=\>"
+syn match   hsNumber		"\v<[0-9]%(_*[0-9])*>|<0[xX]_*[0-9a-fA-F]%(_*[0-9a-fA-F])*>|<0[oO]_*%(_*[0-7])*>|<0[bB]_*[01]%(_*[01])*>"
+syn match   hsFloat		"\v<[0-9]%(_*[0-9])*\.[0-9]%(_*[0-9])*%(_*[eE][-+]?[0-9]%(_*[0-9])*)?>|<[0-9]%(_*[0-9])*_*[eE][-+]?[0-9]%(_*[0-9])*>|<0[xX]_*[0-9a-fA-F]%(_*[0-9a-fA-F])*\.[0-9a-fA-F]%(_*[0-9a-fA-F])*%(_*[pP][-+]?[0-9]%(_*[0-9])*)?>|<0[xX]_*[0-9a-fA-F]%(_*[0-9a-fA-F])*_*[pP][-+]?[0-9]%(_*[0-9])*>"
```

[language-haskell][atom1] for [Atom][atom2] and [linguist][atom3]
-----------------------------------------------------------------

#### Fast version
It has already been officially implemented. Since [1.15.0][atom4]


#### Exact version
After making the following substitution, execute `make` command in the top directory.

Substituting `src/include/repository.coffee` as follows:
```diff
   lit_num: [
     name: 'constant.numeric.hexfloat.haskell'
-    match: "0[xX]#{floatPattern('[0-9a-fA-F_]','[pP]')}"
+    match: "0[xX]_*#{floatPattern('[0-9a-fA-F]','[pP]')}"
   ,
     name: 'constant.numeric.hexadecimal.haskell'
-    match: '0[xX][_0-9a-fA-F]+'
+    match: '0[xX]_*[0-9a-fA-F](_*[0-9a-fA-F])*'
   ,
     name: 'constant.numeric.octal.haskell'
-    match: '0[oO][_0-7]+'
+    match: '0[oO]_*[0-7](_*[0-7])*'
   ,
     name: 'constant.numeric.binary.haskell'
-    match: '0[bB][_01]+'
+    match: '0[bB]_*[01](_*[01])*'
   ,
     name: 'constant.numeric.float.haskell'
-    match: "[0-9]#{floatPattern('[0-9_]', '[eE]')}"
+    match: "#{floatPattern('[0-9]', '[eE]')}"
   ,
     name: 'constant.numeric.decimal.haskell'
-    match: '[0-9][_0-9]*'
+    match: '[0-9](_*[0-9])*'
   ]
```

Furthermore, substituting `src/include/util.coffee` as follows:
```diff
 floatPattern = (digit, exp) ->
-  exponent = "#{exp}[+-]?[0-9_]+"
-  "#{digit}*(?:\\.#{digit}+(?:#{exponent})?|#{exponent})"
+  u_digits = "#{digit}(_*#{digit})*"
+  exponent = "_*#{exp}[+-]?[0-9](_*[0-9])*"
+  "#{u_digits}(?:\\.#{u_digits}(?:#{exponent})?|#{exponent})"
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
             #  Numbers
-            (r'\d+[eE][+-]?\d+', Number.Float),
-            (r'\d+\.\d+([eE][+-]?\d+)?', Number.Float),
-            (r'0[oO][0-7]+', Number.Oct),
-            (r'0[xX][\da-fA-F]+', Number.Hex),
-            (r'\d+', Number.Integer),
+            (r'0[xX]_*[\da-fA-F](_*[\da-fA-F])*_*[pP][+-]?\d(_*\d)*', Number.Float),
+            (r'0[xX]_*[\da-fA-F](_*[\da-fA-F])*\.[\da-fA-F](_*[\da-fA-F])*(_*[pP][+-]?\d(_*\d)*)?', Number.Float),
+            (r'\d(_*\d)*_*[eE][+-]?\d(_*\d)*', Number.Float),
+            (r'\d(_*\d)*\.\d(_*\d)*(_*[eE][+-]?\d(_*\d)*)?', Number.Float),
+            (r'0[bB]_*[01](_*[01])*', Number.Bin),
+            (r'0[oO]_*[0-7](_*[0-7])*', Number.Oct),
+            (r'0[xX]_*[\da-fA-F](_*[\da-fA-F])*', Number.Hex),
+            (r'\d(_*\d)*', Number.Integer),
             #  Character/String Literals
```


[vim1]: https://github.com/vim

[atom1]: https://github.com/atom-haskell/language-haskell
[atom2]: https://github.com/atom/atom
[atom3]: https://github.com/github/linguist
[atom4]: https://github.com/atom-haskell/language-haskell/releases/tag/v1.15.0

[vscode1]: https://github.com/Microsoft/vscode

[pygments1]: http://pygments.org/

[trac9224]: https://ghc.haskell.org/trac/ghc/ticket/9224
[trac13126]: https://ghc.haskell.org/trac/ghc/ticket/13126
[trac14473]: https://ghc.haskell.org/trac/ghc/ticket/14473

