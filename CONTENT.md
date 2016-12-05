---
title: P″ - The original Brainf*ck and mother of the Turing tarpits
published: false
description: A look at the P″ esoteric programming language and it's relation to Brainf*ck
cover_image: 
tags: computer science, brainfuck, turing machines
---

When we think of esoteric programming languages, most of us quickly gravitate towards one in particular: the infamous [Brainfuck](https://en.wikipedia.org/wiki/Brainfuck) - a super minimal programming language that achieves [Turing Completeness](https://en.wikipedia.org/wiki/Turing_completeness) in just six simple instructions (not including the two for I/O).

The incessantly lovely Brainfuck was created in 1993 by Urban Müller in an attempt to construct a usable - albeit barely - programming language with a compiler under 1024 bytes. Why 1024 bytes? Well, he had been inspired by Wouter van Oortmerssen's [FALSE](https://esolangs.org/wiki/FALSE), a stack-based language with a compiler of exactly 1024 bytes. So I guess you could say it was a competition of sorts.

In this article, purely for the joy of it, we will see that **Brainfuck is actually an informal dialect of a programming language invented in Italy more than 50 years ago: Corrado Böhm's P″ (pronounced "P Prime Prime" or "P Double-Prime)**.

I do expect that the reader has basic familiarity with Brainfuck - although, if you've never heard of it, I definitely recommend you take a look. Oh, and don't worry; despite the scary name, it's really very easy to learn (note, I didn't say "easy to use"!).

Let's talk a bit about Corrado Böhm's place in the history of Computer Science before we get into the nuts and bolts of his decidedly strange creation.

## Corrado Böhm and the Structured Program Theorem

**You can skip this section if you're only interested in the details and not the motivation.**

When we talk about "programming" today, we are almost universally referring to what is known as "[structured programming](https://en.wikipedia.org/wiki/Structured_programming)". That is, writing programs that employ the use of block structures, functions and loops. Pretty simple, huh? In fact, this all seems so obvious nowadays that we don't feel the need to qualify so specifically what we mean - so we use more general terms like "imperative" and "declarative" to describe our favorite paradigms of programming. The whole "structured" part is kind of a given.

But it wasn't always this way...

Many of the earliest programming languages, including heavy-hitters like BASIC, FORTRAN and COBOL<sup>1</sup>, were [unstructured](https://en.wikipedia.org/wiki/Non-structured_programming): Statements were sequentially ordered, generally one per-line. And those lines were given labels or numbered in such a way that a program could perform an *unconditional jump* from one part of the program to the other.

And when I say "jump", I really mean jump! Execution was not returned back to the calling context as is the case with a function call (unless, of course, it was physically asked to with another jump). One could, at least theoretically, jump straight into the middle of a `SUBROUTINE` or an `IF`<sup>2</sup>. Following the execution of a program meant tracing all of the jumps. Such a messy form of execution eventually gave rise to a phrase we still hear a lot of today (but never about our own code, of course): "Spaghetti code"<sup>3</sup>!

In case you haven't worked it out already; yes, I am talking about the notorious `GO TO`. 

The early '60s were dominated by the debate over structured vs. non-structured programming. This was further ignited when, in 1966, a landmark paper titled "Flow diagrams, Turing Machines and Languages with only Two Formation Rules" was published by Corrado Böhm and Giuseppe Jacopini. This paper<sup>4</sup> provided the [theoretical foundation for structured programming](https://en.wikipedia.org/wiki/Structured_program_theorem) (and therefore the abolishment of `GO TO`) by proving that one can compute any computable function with only three simple control structures:

1. Execute one subprogram, and then another subprogram (sequence)
2. Execute one of two subprograms according to the value of a boolean expression (selection/branching)
3. Execute a subprogram until a boolean expression is true (iteration)

To demonstrate this in relation to Turing machines, they, of course, used a programming language: **P″**

## P″, the mother of the Turing tarpits

P″ is a very simple. In fact, it's so simple that the whole thing can get a bit confusing. Kinda' like those movies that are so bad they loop back around to being good again. The language consists of only four characters: `R`, `λ`, `(` and `)` that act upon a [Turing machine](https://en.wikipedia.org/wiki/Turing_machine) with infinite tape (main memory) and a finite alphabet (the set of things that can be written in a memory cell - such as the digits `0..9`).

Böhm defined the syntax rules<sup>5</sup> as follows:

1. `λ, R ∈ P″`
2. `q₁, q₂ ∈ P″` implies `q₁q₂ ∈ P″`
3. `q ∈ P″` implies `(q) ∈ P″`
4. Only the expressions that can be derived from rules 1, 2 and 3 belong to P″.

Right. Let's try that again in English:

1. The characters `λ` and `R` are syntactically valid programs in P″.
2. Programs can be composed. So, if `q₁` and `q₂` are programs in P″, then so is their composition: `q₁q₂`.
3. Any valid program can be iterated over. So, if `q` is a program in P″, then so is `(q)`. This will make sense when we get to the semantics.
4. That's it!

Böhm then goes on to explain the language semantics, which I've summarized and simplified here:

- The machine has a finite alphabet of length `N`, where `N > 1`. `0` is considered a special, blank symbol. So each memory cell can contain any value from `0` to `N`. Because we are dabbling in the theoretical world of Turing machines, we can say that the exact value of `N` is precisely what it needs to be for the computation at hand.
- Execution starts at the leftmost symbol and proceeds to the right until there is nothing left to compute.
- `R` is the operation of shifting the tapehead (a.k.a *data pointer*) forward to the right one cell (if there is one).
- `λ` is the operation of replacing the symbol, `c`, at the tapehead with `(c+1) mod (N+1)` and then shifting the tapehead to the left one cell. Note the modulus operation - so if `N = 5` then trying to increment `5` will result in `0` (the blank symbol) because `6 mod 6 = 0`.
- `(q)`, where `q` is any valid program, is interpreted as a *while loop* that iterates while the current cell does not contain the blank symbol (`0`). Endless loops are possible, of course.

So, we can think of P″ as being a literal implementation of a Turing machine in a similar sense that we can see the original Lisp as being an implementation of Church' Lambda Calculus. Just far less useful to the practicing programmer.

In a paper published in 1964 for the International Computation Center in Rome<sup>6</sup>, Böhm proved that P″ was Turing-complete, which makes it the first *structured* programming language that did not contain a `GO TO` instruction but instead relied upon iteration. Djikstra would go on to cite Böhm and Jacopini and in his now-famous paper, [Go To Statement Considered Harmful](http://homepages.cwi.nl/~storm/teaching/reader/Dijkstra68.pdf), which would help to solidify their place in computer science folklore.

Here is a simple program to add two numbers together for a Turing machine where `N = 3` and main memory looks something like this `[2, 1]`:

```
(
 λRλRλR    # Decrement current cell (remember that N+1 wraps back around to 0, so decrementing is equivalent to N increments).
 R         # Move tapehead right.
 λR        # Increment current cell.
 λRλRλRλ   # Move tapehead left by decrementing the current cell and then executing one final λ.
)          # Iterate if the current cell is not 0. This means we iterate twice in this example.
```

At the end of execution main memory will look like this: `[0, 3]`.

Ok, cool, I guess. Maybe you can see some minor similarities to Brainfuck, but we are definitely missing some things. And even in esoteric programming language terms, it's all very clunky!

But, luckily for us, Böhm provided several small abstractions that allow us to write much simpler programs. And in doing so we will see that Brainfuck and P″ are equivalent.

Let's take a look.

## When two become one

There are a few instructions that Brainfuck gives us that we don't get out-of-the-box with P″. Specifically, we still need to fill in the following gaps:

- Increment the current cell (without moving the tapehead!).
- Decrement the current cell.
- Move the tapehead leftward.

Let's define those as follows:

- **Increment**
  - `I = λR`
  - Increment the current cell and move left, then move right.
- **Decrement**
  - `D = I^N`
  - This is defined as `N` `I`'s in a row. So for a Turing machine where `N = 5` it's literally `IIIII`. The best way to think of `D` is as a macro that expands to `N` `I`'s rather than a runtime construct like a function.
- **Left**
  - `L = Dλ`
  - Decrement the current cell and then increment it (to restore the original value) and move left.

To recap, we have now now defined the following instructions:

- **Move right**: `R`
- **Move left**: `L`
- **Increment**: `I`
- **Decrement**: `D`
- **Open loop**: `(`
- **Close loop**: `)`

Now it's starting to look familiar! And so, finally, let's write the previous program again with our new abstractions:

```
(D R I L)
```

And how does the same program look in Brainfuck?

```
[- > + <]
```

Interesting! Although we've decided upon slightly different syntax, these two programs are now semantically identical.

I might just take a moment here to mention that there is something very fulfilling about defining a programming language in which `DRILL` is a valid program (although, I must admit, Böhm used different names). I think I'll call my dialect **"Braindriller"**.

Let's try another program<sup>7</sup> that moves a value from cell `0` two places to the right (cell `2`):

```
R R ( D ) L L ( D R R I L L )
```

And Brainfuck:

```
> > [ - ] < < [ - > > + < < ]
```

Well, I don't know about you, but I'm sold. I implore you to keep trying other Brainfuck programs but, considering the two models of computation are now the same, it's my hypothesis that they will all be instruction-for-instruction identical.

One final note - you'll notice that we've skipped on the I/O instructions `,` and `.` that exist in most dialects of Brainfuck. This is because the theoretical nature of P″ means that practical things like input and output would serve no purpose - we are only interested in the effect of the instructions upon the machine state (memory cells). But if you were to build a P″ interpreter - and I encourage you to do so - then you could implement I/O exactly as Brainfuck does.

## Thanks

If you've made it this far - well done! And I'm sure the mental energy you've just exerted will all pay off in the long run. Think about it: Next time you're talking to someone who brings up Brainfuck, you can tell them all about the wonders of P″ and the genius of Corrado Böhm.

As always, corrections and feedback are always more than welcome. :)

1. BASIC, FORTRAN and COBOL all added support for structured programming in later versions.
2. I guess this would make coroutines somewhat simple to implement.
3. It's not known exactly who coined this term, but according to Wikipedia, an early usage comes from the Guy L. Steele's 1977 paper: [Macaroni is better than spaghetti](http://dl.acm.org/citation.cfm?id=806933)
4. This paper is known to be cited more than it's read. OK, I admit, I *tried* to get through it all. I really did. But before you judge me - [give it a shot yourself](http://www.cs.unibo.it/~martini/PP/bohm-jac.pdf).
5. Had Backus-Naur Form been popular in '64, it would have looked something like this: `<program> ::= R|λ|<program><program>|(<program>)`.
6. I have searched high-and-low for a copy of this paper. The ICC was decommissioned in the late-60s and most of their publications are very difficult to find now. I've tracked a physical copy down to the British Library, but am yet to get my hands on it.
7. Courtesy of [esolangs](https://esolangs.org/wiki/brainfuck).
