# Chapter 0: What is this?

This is a journal about building a tiny typed programming language from scratch.

The language is called Helium. The goal is to explore what happens when you take a minimal embeddable scripting language — something in the spirit of Lua or Wren — and add a real type system. Not a bolted-on gradual typing layer, but Haskell-family type inference: the compiler figures out the types for you and catches mistakes before your code runs. The language should have first-class tooling to show you these issues in your editor.

Languages like Lua and Wren are great for embedding. They're small, they're fast to start up, and they make it easy for a host application to expose functionality to scripts. But they're dynamically typed, which means the boundary between host and script is untyped too. You pass values across that boundary and hope they're the right shape. When they're not, you find out at runtime.

What if that boundary were typed? What if the host could declare "I provide an HTTP client that takes a URL string and returns a record with `status : Int` and `body : String`" and the embedded language could check, at compile time, that scripts use it correctly?

The closest existing language to this idea is [Roc](https://www.roc-lang.org/), whose platform model provides exactly this kind of typed boundary between host and guest code. But Roc is a full-featured language with a native-code compiler — you don't embed it the way you embed Lua, spinning up a tiny VM inside your application. It compiles to machine code and links against your codebase. It's a great language solving a related problem, but it's not small and it's not a scripting engine.

The space I'm curious about — small, embeddable, bytecode-interpreted, _and_ typed — seems mostly empty. Maybe there's a good reason for that. Maybe the type system machinery is inherently too heavy for a scripting language. That's what I'm hoping to find out.

## What this is not

This is not a polished textbook. I'm figuring things out as I go — the type system design, the implementation strategy, the right trade-offs for an embedded context. Some chapters will go down paths that later chapters abandon. I'll try to flag those with forward-references ("we'll replace this approach in Chapter N"), but the journal reflects the real development process, dead ends included.

If you're looking for something polished, I highly recommend [Crafting Interpreters](https://craftinginterpreters.com/) by Robert Nystrom. That doesn't cover building type systems though. For that, I'll add recommendations as I read throught he literature myself.

This is also not an attempt to write another Haskell. Stephen Diehl's [Write You a Haskell](http://dev.stephendiehl.com/fun/) already exists and it's excellent, although sadly imcomplete. Helium is exploring a different question: not "how do you build a full-featured functional language?" but "how much type safety can you fit into something small enough to embed?"

## The plan (loosely)

The implementation is in Haskell, using megaparsec for parsing. Each chapter produces a runnable language, building incrementally:

1. An arithmetic expression evaluator (parse and evaluate `1 + 2 * 3`)
2. Variables and let bindings
3. Functions (lambda calculus, still dynamically typed)
4. A simple type checker (explicit type annotations, no inference)
5. Type inference (Hindley-Milner)
6. Algebraic data types and pattern matching
7. Row-polymorphic records
8. Typed platform boundaries (the host/script interface)

That's the rough trajectory. I'm fairly confident about the first few steps. The later ones — especially row polymorphism and the platform boundary — are where the real exploration happens, and where the plan might change.

## Who this is for

Anyone curious about how type systems work, how parsers work, or how programming languages are built. I'll explain concepts as they come up rather than assuming background knowledge. If you know some Haskell (or are willing to learn it alongside), you should be able to follow along.

Let's start building.

## Chapters

1. [Arithmetic Expressions](chapter/001.md) — Parse and evaluate `1 + 2 * 3`. Introduces megaparsec, the AST, and tree-walk evaluation.
2. [Variables and Let Bindings](chapter/002.md) — `let x = 5 in x + 1`. Error handling with `Either`, environments, and parser error messages.
