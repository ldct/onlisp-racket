# onlisp-racket
On Lisp in Racket (scheme)

#### Preprepreface

This section and the next are not part of the book proper, but written by me. If there is a need to insert such original content anywhere else in the book, I will mark it with [XJ].

### Prepreface

This is a port of Paul Graham's On Lisp to Racket, because I don't really want to learn CL just to read the book. Although I specify Racket here, I mainly do so in order to specify unambiguously some implementation of Scheme, rather than in order to use the non-scheme language features of Racket. Hence, I will try to make sure all the code works in any R5RS Scheme.

This presents a number of difficulties. First, PG starts off by creating unhygenic macrso (if you don't know what these are, they are defined later in the book), whereas Scheme encourages you to write hygenic macros. In fact, as far as I am aware, it is not possible to create unhygenic macros in standard Racket at all! The traditional name of the special form used to define unhygenic macros is `defmacro` or `define-macro`, and Racket will complain that they are deprecated if you try to use them. Instead, it wants you to use the special forms for defining hygenic macros, which are `define-syntax` and `syntax-rules`.

However, I think PG's approach is easier to teach in that you can reuse alot of what you already know about the language to write macros. Hence, we will ask the Racket compiler to please let us use `defmacro` by placing

```
#lang racket
(require compatibility/defmacro)
```

at the top of the file (the `#lang` directive is Racket-specific and always required).

For more about scheme-style hygenic macros, see http://www.willdonnelly.net/blog/scheme-syntax-rules/

Why do I care about macros? In his essay "Beating the Averages", PG writes

"But I think I can give a kind of argument that might be convincing. The source code of the Viaweb editor was probably about 20-25% macros. Macros are harder to write than ordinary Lisp functions, and it's considered to be bad style to use them when they're not necessary. So every macro in that code is there because it has to be. What that means is that at least 20-25% of the code in this program is doing things that you can't easily do in any other language. However skeptical the Blub programmer might be about my claims for the mysterious powers of Lisp, this ought to make him curious. We weren't writing this code for our own amusement. We were a tiny startup, programming as hard as we could in order to put technical barriers between us and our competitors."

20-25%? I can't imagine that. But maybe this is exactly like the C programmer who can't imagine why you would ever use first-class functions. And I love first-class functions and all the other language features that have slowly been absorbed into other languages. Will I feel differently about macros once I understand them?

## Preface

This book is intended for anyone who wants to become a better Lisp programmer. It assumes some familiarity with Lisp, but not necessarily extensive programming experience. The first few chapters contain a fair amount of review. I hope that these sections will be interesting to more experienced Lisp programmers as well, because they present familiar subjects in a new light.

It's difficult to convey the essence of a programminglanguage in one sentence, but John Foderaro has come close:

Lisp is a programmable programming language. 

There is more to Lisp than this, but the ability to bend Lisp to one’s will is a large part of what distinguishes a Lisp expert from a novice. As well as writing their programs down toward the language, experienced Lisp programmers build the language up toward their programs. This book teaches how to program in the bottom-up style for which Lisp is inherently well-suited.

## Bottom-up design

Bottom-up design is becoming more important as software grows in complexity. Programs today may have to meet specifications which are extremely complex, or even open-ended. Under such circumstances, the traditional top-down method sometimes breaks down. In its place there has evolved a style of programming quite different from what is currently taught in most computer science courses: a bottom-up style in which a program is written as a series of layers, each one acting as a sort of programming language for the one above. X Windows and TEX are examples of programs written in this style.

The theme of this book is twofold: that Lisp is a natural language for programs written in the bottom-up style, and that the bottom-up style is a natural way to write Lisp programs. On Lisp will thus be of interest to two classes of readers. For people interested in writing extensible programs, this book will show what you can do if you have the right language. For Lisp programmers, this book offers
a practical explanation of how to use Lisp to its best advantage.

The title is intended to stress the importance of bottom-up programming in Lisp. Instead of just writing your program in Lisp, you can write your own language on Lisp, and write your program in that.

It is possible to write programs bottom-up in any language, but Lisp is the most natural vehicle for this style of programming. In Lisp, bottom-up design is not a special technique reserved for unusually large or difficult programs. Any substantial program will be written partly in this style. Lisp was meant from the start to be an extensible language. The language itself is mostly a collection of Lisp functions, no different from the ones you define yourself. What’s more, Lisp functions can be expressed as lists, which are Lisp data structures. This means you can write Lisp functions which generate Lisp code.

A good Lisp programmer must know how to take advantage of this possibility. The usual way to do so is by defining a kind of operator called a macro. Mastering macros is one of the most important steps in moving from writing correct Lisp programs to writing beautiful ones. Introductory Lisp books have room for no more than a quick overview of macros: an explanation of what macros are, together with a few examples which hint at the strange and wonderful things you can do with them. Those strange and wonderful things will receive special attention here. One of the aims of this book is to collect in one place all that people have till now had to learn from experience about macros. 

Understandably, introductory Lisp books do not emphasize the differences between Lisp and other languages. They have to get their message across to students who have, for the most part, been schooled to think of programs in Pascal terms. It would only confuse matters to explain that, while defun looks like a procedure definition, it is actually a program-writing program that generates code which builds a functional object and indexes it under the symbol given as the first argument.

One of the purposes of this book is to explain what makes Lisp different from other languages. When I began, I knew that, all other things being equal, I would much rather write programs in Lisp than in C or Pascal or Fortran. I knew also that this was not merely a question of taste. But I realized that if I was actually going to claim that Lisp was in some ways a better language, I had better be prepared to explain why.

When someone asked Louis Armstrong what jazz was, he replied “If you have to ask what jazz is, you’ll never know.” But he did answer the question in a way: he showed people what jazz was. That’s one way to explain the power of Lisp—to demonstrate techniques that would be difficult or impossible in other languages. Most books on programming—even books on Lisp programming—deal with the kinds of programs you could write in any language. On Lisp deals mostly with the kinds of programs you could only write in Lisp. Extensibility, bottom-up
programming, interactive development, source code transformation, embedded languages—this is where Lisp shows to advantage.

In principle, of course, any Turing-equivalent programming language can do the same things as any other. But that kind of power is not what programming languages are about. In principle, anything you can do with a programming language you can do with a Turing machine; in practice, programming a Turing machine is not worth the trouble.

So when I say that this book is about how to do things that are impossible in other languages, I don’t mean “impossible” in the mathematical sense, but in the sense that matters for programming languages. That is, if you had to write some of the programs in this book in C, you might as well do it by writing a Lisp compiler in C first. Embedding Prolog in C, for example—can you imagine the amount of work that would take? Chapter 24 shows how to do it in 180 lines of Lisp.

I hoped to do more than simply demonstrate the power of Lisp, though. I also wanted to explain why Lisp is different. This turns out to be a subtle question—too subtle to be answered with phrases like “symbolic computation.” What I have learned so far, I have tried to explain as clearly as I can.

[XJ] (stuff elided here)
