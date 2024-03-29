# Interactive BrainF\*ck Interpreter
[![Build Status](https://travis-ci.com/gordonhart/bfi.svg?branch=master)](https://travis-ci.com/gordonhart/bfi)
[![codecov](https://codecov.io/gh/gordonhart/bfi/branch/master/graph/badge.svg)](https://codecov.io/gh/gordonhart/bfi)

Are you tired of writing Python and JS at your day job? Looking to learn a
lower-level language but dreading leaving the comforts of your REPL? Well, your
search is over: [BrainF\*ck](https://en.wikipedia.org/wiki/Brainfuck) is your
language and `bfi` is your interpreter.

Once you bite the bullet, jump in, and get acclimated, you'll wonder why you
ever waited. Without built-in support for variable names, a garbage collector,
concurrency, syscalls, literals, objects, inheritance, types, syntax, floating
point, keywords, data structures, best practices, or Stack Overflow there are
almost no ways left for you to shoot yourself in the foot.

Of course, if you're feeling a moment of human weakness, it would be possible
to implement any of those things.


## The Basics

The ethos of BrainF\*ck is to leave difficult things like critical thinking and
analysis to the machines like [GPT-2](https://github.com/openai/gpt-2) that are
good at them and focus only on stuff you, as a human, are naturally good at:
pointer arithmetic, rigid control flow, and data encoding.

You know that feeling of dejection when you go to learn a new language and see
that it has an [entire book](https://doc.rust-lang.org/book/) written about it,
an [estranged sequel](https://doc.rust-lang.org/nomicon/) whose existence is
denied by most, and even some
[aspirational fanfiction](https://rust-lang.github.io/async-book/01_getting_started/01_chapter.html)
in the works? Don't you just wish everything you needed to know about your new
language fit in a single README section? Bear with me here for a little longer.

Imagine you're back on the primordial savannah but instead of a spear in your
hand it's a pointer and instead of lush grassland around you it's a single
dimensional roll of tape extending forever off into the point horizon. If you
have properly engaged your lizard brain this scene should come naturally.

You have 8 standard language commands to memorize:

| Command | Description |
| ------- | ----------- |
| `+` | Increment the current cell (with rollover) |
| `-` | Decrement the current cell (will rollunder) |
| `>` | Move to the next cell |
| `<` | Move to the previous cell |
| `.` | Output the current cell |
| `,` | Read input into the current cell |
| `[` | If the current cell is zero, skip ahead to the matching `]` |
| `]` | If the current cell is not zero, skip back to the matching `[` |

Plus two more `bfi` extension commands:

| Command | Description |
| ------- | ----------- |
| `#` | Dump program internals to `stderr` |
| `%` | Enter into a REPL |

Every other character is a comment. Feel free to annotate your code with as many
emoji as you think are reasonable for an adult to put into a text file and use
whatever limp or virile indentation strategy floats your boat.


## An Example

Now that you have the language down pat we can jog our legs with a gumball
program:

```
,[.[-],]
```

Recognize it? It's our friend, `cat`. It reads from stdin and spits it out to
stdout until a NUL terminator is received. My productivity skyrocketed after
aliasing 😸 to it in my shell. Not having to worry about a `cat` implementation
I don't understand freed up a ton of headspace better spent visualizing tape.
Who _really_ needs options like `--help` from their command line utilities?

Example usage:

```
$ <README.md bfi ',[.[-],]'
```

Yes, GNU `cat` supports interior NUL bytes and this program does not. Those
invalid unicode sequences in that binary you accidentally catted would only
have messed up your terminal's encoding anyway.


## The Details

To clear up some of the ambiguity around the behavior of this BrainF\*ck
implmentation:

- The roll of tape is infinite in both directions. You are free to travel along
  it as you wish and fresh cells will be allocated ahead of you until your OS
  decides otherwise.
- Cells hold a single byte (i.e. value on `[0, 255]`), are initialized to zero,
  and wrap on over or underflow.
- If input is requested (`,`) when none is available the interpreter will move
  on without action.


## Usage

Compile `bfi` as you would any other Rust project not provided prepacked through
standard channels:

```
$ cargo build --release
```

From there, figure it out:

```
$ ./bfi --help
```


## `bfi` as a Library

Luckily for you Rust programmers, `bfi` has a library interface! See
`examples/toy.rs` for a starting point.

BrainF\*ck is an excellent language to implement the workload of your networked
application in. See `examples/{server,client}.rs` for a simple number cruncher
microservice and example client communicating using
[ZeroMQ](https://zeromq.org/socket-api/).

### Foreign Usage

Some hope remains for those of us forced to use a tired language like Python
in our professionial environments. Check your despair at the gate — you can work
with `libbfi` the same way you'd incorporate any foreign object into your
project. See `examples/python/bindings.py` for a Python integration using
`ctypes`.

Further, `examples/python/trick_your_boss.py` contains a minimal framework for
surreptitiously programming in BrainF\*ck at work under your manager's nose.
Don't worry about the resulting proliferation of binary blobs in your repo, odds
are s/he doesn't even review your code. On the off chance you get a question
about all of the "corrupt GIFs" appearing, make something up about the pixel
depth or proprietary codecs or just let GPT-2 make your excuse up for you.


---
---
---


Serious face: the secondary purpose of this project is the interpreter; its
primary purpose is as a playground to learn Rust. It's my first stab at the
language and its ecosystem and it's probably not the highest quality or most
idiomatic codebase out there. Take what you see here with a grain of salt.
