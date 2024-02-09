---
title: Slow Entry Trampoline?
tags: WebAssembly JavaScript Browser Performance
---

# Slow Entry Trampoline?

<!-- page_excerpt -->
This week I've been using Firefox Profiler on a site I'm building. While looking at the stack chart of a
WebAssembly function, I noticed this<!-- endpage_excerpt -->:

[![Stack Chart showing a WebAssembly function called via "slow entry trampoline"][img-stack]][img-stack]

Between my JavaScript and WebAssembly functions there's a "slow entry trampoline". That raised two questions:


## Q1: What _is_ a "slow entry trampoline"?

This took a bit of digging to find out. A web search got no hits for that phrase, and it wasn't mentioned in the
[Firefox Profiler documentation][profiler-docs].

In the end I found the answer in the [Firefox source code][mozilla-src]:

[![Searchfox results page for the words "slow entry trampoline"][img-searchfox]][img-searchfox]

The second result lead to [WasmFrameIter.cpp][frameiter]:
```cpp
switch (codeRange_->kind()) {
  [...]
  case CodeRange::InterpEntry:
    return "slow entry trampoline (in wasm)";
  case CodeRange::JitEntry:
    return "fast entry trampoline (in wasm)";
```

CodeRange is defined in [WasmCodegenTypes.h][codegentypes]:
```cpp
class CodeRange {
 public:
  enum Kind {
    [...]
    InterpEntry,       // calls into wasm from C++
    JitEntry,          // calls into wasm from jit code
```

So a "slow entry trampoline" is just the code used by the JavaScript interpreter to call a WebAssembly function.
The interpreter is written in C++, which is why that's mentioned in the comment.

For more detail on how calls between JavaScript and WebAssembly are handled, see this
[Mozilla Hacks post][fast-calls].


## Q2: Can I make that a "_fast_ entry trampoline" somehow?

Probably not. The entry trampoline is selected based on whether the calling JavaScript code is interpreted or JIT
compiled. That's mostly in the hands of the JavaScript engine.


## Conclusion

In my case the calling code runs infrequently, and the WebAssembly function does a significant amount of work
before returning. That means the JavaScript engine is unlikely to JIT the calling code.

The same situation is likely to arise anywhere WebAssembly is used to optimise a hot path.

In a way, I think seeing the slow trampoline on the stack indicates that all's well. If the calling code runs
infrequently enough to remain interpreted, the total call cost is probably insignificant.



[img-stack]: /assets/images/blog/slow-entry-trampoline/profile.png "&quot;Slow Entry Trampoline&quot;"
[profiler-docs]: https://profiler.firefox.com/docs/#/
[mozilla-src]: https://searchfox.org/mozilla-central/source/
[img-searchfox]: /assets/images/blog/slow-entry-trampoline/searchfox.png "Searchfox Results"
[frameiter]: https://searchfox.org/mozilla-central/rev/54c9b4896fdc1e858cd4942f306d877f1f3d195e/js/src/wasm/WasmFrameIter.cpp#1857-1863
[codegentypes]: https://searchfox.org/mozilla-central/rev/54c9b4896fdc1e858cd4942f306d877f1f3d195e/js/src/wasm/WasmCodegenTypes.h#368-373
[fast-calls]: https://hacks.mozilla.org/2018/10/calls-between-javascript-and-webassembly-are-finally-fast-%F0%9F%8E%89/#optimizations
