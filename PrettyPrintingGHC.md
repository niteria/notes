# End goal

Pretty-printer with annotations and effects in GHC.

# Why?

* Allows to create gcc.godbolt.org for all the stages of the pipe-line (Haskell, Core, Stg, Cmm, Asm).
* Error messages with colors https://ghc.haskell.org/trac/ghc/ticket/8809.
  * Need effects for Windows: https://wiki.haskell.org/wikiupload/4/4c/Hiw-2015-david-christiansen.pdf, slide 33
* Idris style interactive messages: http://www.davidchristiansen.dk/2014/09/06/a-pretty-printer-that-says-what-it-means/

# Problems

* Current pretty printer is a fork of `pretty`: https://github.com/haskell/pretty
  * `pretty` already has annotations: https://github.com/haskell/pretty/pull/19, the fork doesn't
  * Some patches can't be applied because of performance: https://github.com/haskell/pretty/issues/1, https://ghc.haskell.org/trac/ghc/ticket/10735#comment:20
    * For some reason GHC uses pretty-printer to produce Cmm, Asm and Llvm
    * It gets accidentaly quadratic
      * All libraries get accidentally quadratic: https://github.com/jwaldmann/pretty-test
        * https://github.com/ekmett/wl-pprint-extras/issues/16
        * https://github.com/haskell/pretty/issues/32
  * Code size increase? https://ghc.haskell.org/trac/ghc/ticket/10878


# Related

* Rust killed two birds with one stone by crowd-sourcing error message improvements: https://github.com/rust-lang/rust/issues/35233
  * Nice bootcamp task for new contributors
  * Would be tedious for one person to do
  * Great tutorial: http://www.jonathanturner.org/2016/08/helping-out-with-rust-errors.html
