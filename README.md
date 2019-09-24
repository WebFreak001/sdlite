SDLite - a lightweight SDLang parser/generator
----------------------------------------------

This library implements a small and efficient parser/generator for SDLang
documents, providing a range based API. While the parser still uses the GC to
allocate identifiers, strings etc., it uses a very efficient pool based
allocation scheme that has very low computation and memory overhead.

The motivation for writing another SDLang implementation for D came from the
high overhead that the original [sdlang-d][1] implementation has. Parsing a
particular 200 MB file took well over 30 seconds and used up almost 10GB of
memory if parsed into a DOM. The following changes to the parsing approach
brought the parsing time down to around 2.5 seconds:

- Using a more efficient allocation scheme
- Using only "native" range implementations instead of the more comfortable
  fiber-based approach taken by sdlang-d
- Using `TaggedUnion` ([taggedalgebraic][2]) instead of `Variant`

Further substantial improvements at this point are more difficult and likely
require the use of bit-level tricks and SIMD for speeding up the lexer, as well
as exploiting the properties of pure array inputs.

[1]: https://github.com/Abscissa/SDLang-D
[2]: https://github.com/s-ludwig/taggedalgebraic