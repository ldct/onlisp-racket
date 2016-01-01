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
