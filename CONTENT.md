# P′′ - The Original Brainf*ck

When we think of esoteric programming languages, most of us quickly gravitate towards one in particular: the infamous [Brainfuck](aaa) - a super minimal programming language that achieves [Turing Completeness](aaa) in just six simple instructions (not including the two instructions for I/O).

The incessantly lovely Brainfuck was created in 1993 by Urban Müller in an attempt to construct a usable - albeit barely - programming language with a compiler under 1024 bytes. Why 1024 bytes? Well, he had been inspired by Wouter van Oortmerssen's [FALSE](aaa), a stack-based language with a compiler of exactly 1024 bytes. So I guess you could say it was a competition of sorts.

In this article, purely for the fun of it, we will see that Brainfuck is actually an informal dialect of a programming language invented in Italy 30 years prior: Corrado Böhm's **P′′** (or "P Prime Prime"). I do expect that the reader has basic familiarity with Brainfuck - although, if you've never heard of it, I definitely recommend you take a look. Oh, and don't worry; despite the scary name, it's really very easy to learn (note, I didn't say "easy to use"!). 

Let's talk about a bit about Corrado Böhm's place in the history of Computer Science before we get into the nuts and bolts of his decidedly strangely-named creation.

## Corrado Böhm and the Structured Program Theorem

**You can skip this section if you're only interested in the details and not the motivation**

When we talk about "programming" today, we are almost universally referring to what is known as "structured programming". That is, writing programs that employ the use of block structures, functions and loops. Pretty simple, huh? In fact, this all seems so obvious nowadays that we don't feel the need to quality so specifically what we mean - so we use more general terms like "imperative" and "declarative" to describe our favourite languages. The whole "structured" part is kind of a given.

But it wasn't always this way...

Many of the earliest programming languages, including heavy-hitters like BASIC, FORTRAN, COBOL <sup>1</sup> and several assembly languages, were *unstructured*: Statements were sequentially ordered, generally one per-line. And those lines were given labels or numbered in such a way that a program could perform an *unconditional jump* from one part of the program to the other.

And when I say "jump", I really mean jump! Execution was not returned back to the calling context as is the case with a function call (unless, of course, it was physically asked with another jump). One could, at least theoretically, jump straight into the middle of a `SUBROUTINE` or an `IF` <sup>2</sup>. Following the execution of a program meant tracing all of the jumps. Such a messy form of execution eventully gave rise to a phrase we still hear a lot of today (but never about our own code, of course): "Spaghetti code" <sup>3</sup>!

In case you haven't worked it out already; yes, I am talking about the notorious `GO TO`. 

The early '60s were dominated by the debate over structured vs. non-structured programming. This was further ignited when a landmark paper titled "Flow diagrams, Turing Machines and Languages with only Two Formation Rules" was published by Corrado Böhm and Giuseppe Jacopini in 1966. This paper <sup>4</sup> provided the theoretical foundation for structured programming (and therefore the abolishment of `GOTO`) by proving that one can compute any computable function with only three simple control structures:

1. Execute one subprogram, and then another subprogram (sequence)
2. Execute one of two subprograms according to the value of a boolean expression (selection/branching)
3. Execute a subprogram until a boolean expression is true (iteration)

To demonstrate this in relation to Turing machines, they, of course, used a programming language: P′′

## P′′, the Grandmother of the Turing Tarpits

P′′ is a very simple. In fact, it's so simple that the whole thing can get a bit confusing. Kinda' like those movies that are so bad they loop back around to being good again. The language consists of an alphabet of only four characters: `R`, `λ`, `(` and `)` that act upon a [Turing machine](https://en.wikipedia.org/wiki/Turing_machine) with infinite tape and a finite alphabet (the set of things that can be written in a memory cell - such as the digits `0..9`).

Böhm defined the syntax rules <sup>5</sup> as follows:

1. `λ, R ∈ p″`
2. `q₁, q₂ ∈ p″` implies `q₁q₂ ∈ p″`
3. `q ∈ p″` implies `(q) ∈ p″`
4. Only the expressions that can be derived from rules 1, 2 and 3 belong to P″.

Right. Let's try that again in English:

1. The characters `λ` and `R` are syntactically valid programs in p″.
2. Programs can be composed. So, if `q₁` and `q₂` are programs in p″, then so is their composition: `q₁q₂`.
3. Any valid program can be iterated over. So, if `q` is a program in p″, then so is `(q)`. This will make sense when we get to the semantics.
4. That's it!

Böhm then goes on to explain the language semantics:

- The machine has a finite alphabet of length `N`, where `N > 1`. `0` is considered a special character. So each memory cell can contain any value from `0` to `N`.
- Execution starts at the leftmost symbol and proceeds to the right until there is nothing left to compute.
- `R` is the operation of shifting the tapehead forward to the right one cell (if there is one).
- `λ` is the operation of replacing the symbol, `c`, at the tapehead with `(c+1) mod (N+1)` and then shifting the tapehead to the left one cell. Note the modulus operation - so if `N = 5` then trying to increment `5` will result in `0` (the blank symbol) because `6 mod 6 = 0`.
- `(q)`, where `q` is any valid program, is interpreted as a *while loop* that iterates while the current cell does not contain the blank symbol (`0`).

Let's run through a simple program:

## Brainfuck

- Doesn't sound much like BF yet?
- Abstractions on p''
- Examples

## Conclusion

- Why?
- Thanks

1. BASIC, FORTRAN and COBOL all added support for structured programming in later versions.
2. I guess this would make coroutines somewhat simple to implement.
3. It's not known exactly who coined this term, but an early usage comes from the Guy L. Steeles 1977 paper: [Macaroni is better than spaghetti](http://dl.acm.org/citation.cfm?id=806933)
4. This paper is known to be cited more than it's read. OK, I admit, I tried to read it. I really did. But before you judge me - [give it a shot yourself](http://www.cs.unibo.it/~martini/PP/bohm-jac.pdf).
5. Had Backus-Naur form been popular in '64, it would have looked something like this: `TODO`.
