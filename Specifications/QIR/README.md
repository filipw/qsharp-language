# Quantum Intermediate Representation (QIR)

This draft specification defines an intermediate representation for compiled quantum 
computations.

## Motivation

There are many languages and circuit-building packages that are used today to
program quantum computers.
These languages have some similarities, but vary significantly in their syntax
and semantics.

Similarly, there are many different types of quantum computers available already, 
and more types being developed.
Different computers have different instruction sets, different timings, and
different classical capabilities.

It is very useful to have a single intermediate representation that can express programs from 
all of the different quantum languages and can be converted into code for any
of the different quantum computers.
This allows language developers to write one compiler and quantum computer developers
to write one code generator, while still allowing all languages to run on all computers.
This is a well-established pattern in classical compilation.

## Model

We see compilation as having three high-level phases:

1. A language-specific phase that takes code written in some quantum language,
   performs language-specific transformations and optimizations, and compiles
   the result into this intermediate format.
2. A generic phase that performs transformations and analysis of the code
   in the intermediate format.
3. A target-specific phase that performs additional transformations and
   ultimately generates the instructions required by the execution platform
   in a target-specific format.

By defining our representation within the popular open-source [LLVM](http://llvm.org) framework,
we enable users to easily write code analyzers and code transformers that
operate at this level, before the final target-specific code generation.

It is neither required nor expected that any particular execution target actually
implement every runtime function specified here.
Rather, it is expected that the target-specific compiler will translate the
functions defined here into the appropriate representation for the target, whether
that be code, calls into target-specific libraries, metadata, or something else.

This applies to quantum functions as well as classical functions.
We do not intend to specify a gate set that all targets must support, nor even
that all targets use the gate model of computation.
Rather, the quantum functions in this document specify the interface that
language-specific compilers should meet.
It is the role of the target-specific compiler to translate the quantum functions
into an appropriate computation that meets the computing model and capabilities
of the target platform.

## Profiles

We know that many current targets will not support the full breadth of possible
quantum programs that can be expressed in this representation.
We define a sequence of specification _profiles_ that define
coherent subsets of functionality that a specific target can support.
Please take a look at [this document](Profiles.md) for more details.

In particular, the QIR specification permits the usage of subroutines as first class values, and includes the necessary expressiveness to e.g. provide runtime support for functor application. This introduces the need to define a common structure to represent callable values and their arguments. If the source language does not make use of such features and there is no need to represent callable values in the compilation, then the corresponding sections in [Callables.md](Callables.md) don't apply. 

## Current Projects / Users

The [list page](List.md) may be used to track current public projects that use QIR.
Please file a pull request to this page if you'd like your project listed.

## Index

1. [Data Types](Data-Types.md)
1. [Callables](Callables.md)
1. [Metadata](Metadata.md)
1. [Identifiers](Identifiers.md)
1. [Classical Runtime](Classical-Runtime.md)
1. [Quantum Runtime](Quantum-Runtime.md)
1. [Code Generation](Code-Generation.md)
1. [Profiles](Profiles.md)
2. [Appendix: Library Reference](Library-Reference.md)
