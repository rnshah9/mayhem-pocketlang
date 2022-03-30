
<p align="center" >
<img src="https://user-images.githubusercontent.com/41085900/117528974-88fa8d00-aff2-11eb-8001-183c14786362.png" width="500" >
</p>

**Pocketlang** is a small (~3000 semicolons) and [fast](https://github.com/ThakeeNathees/pocketlang#performance)
functional language written in C. It's syntactically similar to Ruby and it can
be learned [within 15 minutes](https://thakeenathees.github.io/pocketlang/getting-started-learn-in-15-minutes.html).
Including the compiler, bytecode VM and runtime, it's a standalone executable
with zero external dependencies just as it's self descriptive name. The pocketlang
VM can be embedded in another hosting program very easily.

[Wren Language](https://wren.io/) and their wonderful book [Crafting Interpreters](http://www.craftinginterpreters.com/)
were used as a reference to write this language.

## What pocketlang looks like

```ruby
# Python like import statement.
from lang import clock as now

# A recursive fibonacci function.
def fib(n)
  if n < 2 then return n end
  return fib(n-1) + fib(n-2)
end

# Prints all fibonacci from 0 to 10 exclusive.
for i in 0..10
  print(fib(i))
end
```

## Try It Now

You can [try pocketlang on your browser](https://thakeenathees.github.io/pocketlang/getting-started-try-it-now.html).
It's a [WebAssembly](https://webassembly.org/) build of the VM compiled using [emscripten](https://emscripten.org/).
Note that in the webassembly version of the language, some features (input, file handling, relative import, etc.)
have disabled, has limited memory allocations, and the stdout calls might be slower.

## Documentation

The pocketlang documentation is hosted at https://thakeenathees.github.io/pocketlang/ which
is built from the `docs` branch generated by a little python script at `docs/generate.py`.
Note that the documentations are WIP and might not be up to date.

## Performance

Pocketlang supports [tail call](https://en.wikipedia.org/wiki/Tail_call) [optimization](https://wiki.c2.com/?TailCallOptimization).
When a function returns a call, the callee can re-use the caller's stack frame, this will optimize memory from O(n) to O(1)
and for [tail recursive](https://www.youtube.com/watch?v=-PX0BV9hGZY) it'll completely prevent stackoverflows and yet it's faster
than tco disabled.

All benchmarks below were executed on: Windows10 (64bit), ASUS N552VX, Intel Core i7-6700HQ 2.6GHz
with 12GB SODIMM Ram. And the language versions are: pocketlang (pre-alpha), wren v0.3.0,
python v3.7.4, ruby v2.7.2.

![performance](https://user-images.githubusercontent.com/41085900/120123257-6f043280-c1cb-11eb-8c20-a42153268a0f.png)

The source files used to run benchmarks can be found at `test/benchmarks/`
directory. They were executed using a small python script in the test directory.

## Building From Source

It can be build from source easily without any dependencies, or additional requirements
except for a c99 compatible compiler. It can be compiled with the following command.

#### GCC / MinGw / Clang (alias with gcc)
```
gcc -o pocket cli/*.c src/*.c -Isrc/include -lm
```

#### MSVC
```
cl /Fepocket cli/*.c src/*.c /Isrc/include && rm *.obj
```

#### Makefile
```
make
```
To run make file on windows with `mingw`, you require `make` and `find` unix tools in your path.
Which you can get from [msys2](https://www.msys2.org/) or [cygwin](https://www.cygwin.com/). Run
`set PATH=<path-to-env/usr/bin/>;%PATH% && make`, this will override the system `find` command with
unix `find` for the current session, and run the `make` script.

#### Windows batch script
```
build
```
You don't have to run the script from a Visual Studio .NET developer command prompt, It'll search
for the MSVS installation path and setup the build environment.

### For other compiler/IDE

1. Create an empty project file / makefile.
2. Add all C files in the src directory.
3. Add all C files in the cli directory (**NOT** recursively).
4. Add `src/include` to include path.
5. Compile.

If you weren't able to compile it, please report us by [opening an issue](https://github.com/ThakeeNathees/pocketlang/issues/new).


## References
- Bob Nystrom.(2021) *craftinginterpreters* [online] Available at www.craftinginterpreters.com/ (Accessed January 2021)

- Mark W. Bailey, Nathan C. Weston (June 2001) Technical report. *Performance Benefits of Tail Recursion Removal in
Procedural Languages* [online] Available at http://cs.hamilton.edu/~mbailey/pubs/techreps/TR-2001-2.pdf

- Leonard schütz.(2020) *Dynamic Typing and NaN Boxing* [online] Available at https://leonardschuetz.ch/blog/nan-boxing/ (Accessed December 2020)

- Bob Nystrom.(2011) *Pratt Parsers: Expression Parsing Made Easy* [online] Avaliable at http://journal.stuffwithstuff.com/2011/03/19/pratt-parsers-expression-parsing-made-easy/ (Accessed December 2020)

- Carol E. (Wolf of Pace University), P. Oser. *The Shunting Yard Algorithm* [online] Available at http://mathcenter.oxford.emory.edu/site/cs171/shuntingYardAlgorithm/ (Accessed September 2020)
