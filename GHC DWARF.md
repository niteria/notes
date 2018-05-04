# Current goal: make unwinding work with DWARF again (?)

* [Dwarf standard v2](http://www.dwarfstd.org/doc/dwarf-2.0.0.pdf)
* [Dwarf standard v3](http://www.dwarfstd.org/doc/Dwarf3.pdf), needed for `dW_CFA_expression`
* [Source notes](https://ghc.haskell.org/trac/ghc/wiki/SourceNotes)
* [Overview of code generator](https://ghc.haskell.org/trac/ghc/wiki/Commentary/Compiler/CodeGen/Overview)

### Previous work:
* https://ghc.haskell.org/trac/ghc/ticket/11338 Unwind information is incorrect in region surrounding a safe foreign call
* https://ghc.haskell.org/trac/ghc/ticket/11337 Unwind information incorrect between Sp adjustment and end of block
* https://phabricator.haskell.org/D2741 Generalize CmmUnwind and pass unwind information through NCG
* https://phabricator.haskell.org/D3104 CmmLayoutStack: Correctly annotate Sp adjustments with unwinding information, 
appears to be the blame for #14999
* https://phabricator.haskell.org/D2742 CmmLayoutStack: Add unwind information on stack fixups,
the code makes sense, but it breaks lint from #15000
* https://phabricator.haskell.org/D169, initial diff
* https://phabricator.haskell.org/D2738, Cmm: Add support for undefined unwinding statements
* https://phabricator.haskell.org/D1279, Output source notes in extended DWARF DIEs
* https://ghc.haskell.org/trac/ghc/ticket/13866, -g doesn't work with -pgma=clang++

### My tickets:
* https://ghc.haskell.org/trac/ghc/ticket/14999, unwinding info for stg_catch_frame is wrong
* https://ghc.haskell.org/trac/ghc/ticket/15000, Add a linter for debug information (-g)
* https://ghc.haskell.org/trac/ghc/ticket/14894, HEAD fails to build with -g1
* https://ghc.haskell.org/trac/ghc/ticket/14779, Compiling with -g fails -lint-core checks, also a runtime bug
* https://sourceware.org/ml/gdb-patches/2018-02/msg00252.html, `gdb` makes assumptions that make sense for C-like languages, patch not merged, because there's a concern about performance

### Known (unticketed) problems:
* some tests in ./validate regress 75% allocations with `-g`
* unwinding past `stg_stop_thread_info` (FFI) doesn't work on `-threaded`. Solution: we need to unwind `rbp` as well.
* 8.2.x will probably not work with `-g`. The status of 8.4.x is unclear. See https://ghc.haskell.org/trac/ghc/ticket/14868

## Future ideas:

### Unwind everything and give access to locals
* Would probably require some extra type info

### Use DWARF for retainer profiles without `-prof`

### Be more strategic about source notes with `-g1`.

* Need to be careful to preserve middle frames


## Good reading material

[CFI directives in assembly files](https://www.imperialviolet.org/2017/01/18/cfi.html)

[CFI directives](https://sourceware.org/binutils/docs-2.24/as/CFI-directives.html#CFI-directives)

[6.45.2 Extended Asm - Assembler Instructions with C Expression Operands](https://gcc.gnu.org/onlinedocs/gcc/Extended-Asm.html#Extended-Asm)
