Haskell Best Practices
======================

_For Beginner-to-Intermediate Haskellians_

Haskell is a very flexible language and as a result there is an abundance of 
tools often with subtley different tradeoffs, workflows, or opinions. This list 
is a collection of recommendations targeted to Haskell users who are in the 
awkward stage of knowing enough Haskell to use reasonably intermediate languages 
and libraries, but do not yet feel condfident to investigate and weigh the 
minutia of different options. This is a list of "safe bets" to avoid analysis 
paralysis.

* [HTTP Request Library](#http-request-library)
* [JSON Serialization and Parsing](#json-serialization-and-parsing)
* [Effects System](#effects-system)
* [Strings](#strings)
* [Web Servers](#web-servers)
* [General Parsing](#parsing)
* [Streaming Libraries](#streaming-libraries)
* [Command Line Arguments](#command-line-arguments)

# The Golden Rule

In general if you need a library for some functionality and a library you're 
already using depends on a certain library for that functionality, you should 
consider using that library unless you have specific requirements that it does 
not meet. Or, you should consider a library within an ecosystem of libraries 
that you are already using. For example, if you are already using the `Conduit` 
ecosystem of libraries, it will probably be easier to also use `xml-conduit` 
than another XML parsing library. However, if you do not particularly care about 
your dependencies or executable size, you might instead choose a tool listed 
here with a user experience more suited to you.

# HTTP Request Library

### Req
Req and Wreq are batteries-included, easy-to-use request libraries that handle 
the majority of use cases.

Wreq is a derivative of `Req`, and also provides lens-based API for ease of use, 
and therefore also depends on the `lens` library. If you are unfamiliar with 
lenses, go with `Req`; it's a big library and there's no need to bring it in. If 
you are familiar with lenses or will be parsing a lot of raw JSON (in which case 
you will most likely also be using `aeson` and `lens-aeson`) go with `Wreq`.

# JSON

### aeson
Parsing is one of Haskell's greatest strengths, and JSON is no exception.  
`aeson` is the de-facto standard library for parsing JSON and can generically 
serialize most data structures.

### plus perhaps lens-aeson or microlens-aeson
If you are doing significant amounts of raw JSON traversal and parsing (in 
contrast with simple serialization and deserialization where you know the 
expected input/output) it is worth learning the basics about how to use lenses 
and lens-compatible libraries, and you will also likely want `lens-aeson` or 
`microlens-aeson`.

Again, `lens` is a big library, so many people avoid depending on it unless they 
will be using it heavily. If you have another libary that will also depend on 
`lens` or need the additional features of `lens`, go with `lens-aeson`. If not, 
go with `microlens-aeson`. They both meet the large majority of use-cases, 
though `lens-aeson` does have more depth.

# Effects System

### None
Effects systems only really become useful once applications reach a moderate 
size and level of complexity. For smaller applications where most of the IO 
logic can sit at the top level, it's more prudent to focus on simply decomposing 
IO functions and refactoring to keep business logic out of IO.

### Whatever your framework uses
For example, if you're building a web API you may also be using a framework that 
bundles an effects system. Needless to say, do not give yourself the headache of 
trying to unify two effects systems.

### Transformers

### Tagless-Final or RIO

# Strings
An oddity in Haskell is the three primary string types. `String` is the built-in 
string type and is a simple list of ASCII characters. However, it's not 
particularly performant and does not handle Unicode well.

### Text
In most situations where you're dealing with a modest amount of text that is 
intended to be read by a human, you want `Text`. `Text` has all of the utility 
functions that work on `String` (and more), but is more performant and handles 
Unicode.

For small applications, or applications that do minimal String processing 
`String` will be fine.

### ByteString
`ByteString` is an implementation of raw byte arrays, and therefore may be 
arbitrary binary data. It is best used when you care about performance more than 
whether it's human-readable, do not care about the abstraction around the 
representation of whatever data it represents, or need to work with raw bytes.  
As an array of Word8 bytes, `ByteString` does not care about the encoding of its 
contents. `ByteString` also enables more control over the memory use of data and 
is useful when dealing with large 

# Web Servers

# Parsing
The major libraries in this space are `parsec` and its derivatives `attoparsec` 
and `megaparsec`. Similar to `lens` variants, the API and user experience of 
these three libraries are very similar, so understanding the basics of one will 
translate well to another.

### megaparsec
`megaparsec` has all of the functionality of `parsec` and `attoparsec` but with 
better error messages, and generally better user experience. As a tradeoff, 
`attoparsec` is faster.

# Streaming
This is another area where the general patterns and user experience of one tool 
will generally apply to the others.

### Streaming
The aptly-named `Streaming` package is aims to be the most simple streaming 
library and builds off of existing base knowledge of List-like operations.

### Conduit
The primary use case for `Conduit` is streams of raw data, though it can also be 
used for reactive, event-oriented programming as well. It is part of a large 
ecosystem of `Conduit`-oriented packages and is the most established, widely 
used option. `Conduit` also has packages for handling concurrent streams.

# Command Line Arguments

### optparse-applicative

`optparse-applicative` is by far the de-facto standard for parsing command line 
arguments with an easy to use API and automatically generated `--help` 
documentation.
