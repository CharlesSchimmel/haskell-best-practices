Haskell Best Practices
======================

_(For Beginner-to-Intermediate Haskellians)_

# What is this?
Haskell is a very flexible language and as a result there are often many 
different flavors of tools that cater to different ideologies or opinions. This 
list is a collection of recommendations targeted to Haskell users who are in the 
slightly awkward position of knowing enough Haskell to use reasonably 
intermediate languages and libraries, but do not yet know enough to confidently 
investigate and weigh the minutia of different options.

# HTTP Request Library

## Wreq or Req
Req and Wreq are more or less the de-facto, easy-to-use request libraries. They 
handle all common HTTP-related tasks, and are very easy to get started with.

Wreq is very similar to Req, except that it also provides lens-based API for 
ease of use, and therefore also depends on the `lens` library. If you are 
unfamiliar with lenses, go with Req; it's a big library and there's no need to 
bring it in. If you are familiar with lenses or will be parsing a lot of raw 
JSON (in which case you will most likely also be using aeson and lens-aeson) go 
with Wreq.

# JSON {de,}serialization and parsing

## aeson
Parsing is one of Haskell's greatest strengths, and JSON is no exception.  
`aeson` is the de-facto standard library for parsing JSON and can automatically 
{de,}serialize most data structures.

## plus perhaps lens-aeson or microlens-aeson
If you are doing significant amounts of raw JSON traversal and parsing (in 
contrast with simple serialization and deserialization where you know the 
expected inpu/output) it is worth learning the basics about how to use lenses 
and lens-compatible libraries, and you will also likely want `lens-aeson' or 
`microlens-aeson`.

Again, `lens` is a big library, so many people avoid depending on it unless they 
will be using it heavily. If you have another libary that will also depend on 
`lens` or need the additional features of `lens`, go with `lens-aeson`. If not, 
go with `microlens-aeson`. They both meet the large majority of use-cases, 
though `lens-aeson` does have more depth.

# Effects System

## For Small Applications and Beginners

## None
Effects systems only really become useful once applications reach a moderate 
size and level of complexity. For smaller applications where most of the IO 
logic can sit at the top level, it's more prudent to focus on simply decomposing 
IO functions and refactoring to keep business logic out of IO.

## Whatever your framework uses
For example, if you're building a web API you may also be using a framework that 
bundles an effects system. Needless to say, do not give yourself the headache of 
trying to manage two effects systems.

## Tagless-Final or RIO

# Strings

## Why are there 3 String Types?
String is a simple list of ASCII characters and is built into the GHC as the 
standard text type. However, it's not particularly performantG

## When to use String
If you're doing a small amount of basic string reading and writing, `String` 
will be fine.

## When to use Text
In most situations where you're dealing with a modest amount of text that is 
intended to be read by a human, you want Data.Text. Data.Text has all of the 
utility functions that work on String (and more), but is more performant.

## When to use ByteString
ByteString is a collection of raw bytes and may or may not represent 
human-readable text. It is best used when you do not care whether it's 
human-readable, do not care about the abstraction around the representation of 
whatever data it represents, or need to parse raw bytes. It could be UTC-8, 
ASCII, or whatever.

# Web Servers

# Parsing

# Streaming Libraries
