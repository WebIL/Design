The WebIL pipeline works roughly like the following:

- A compiler which converts, for example, .NET DLLs into a .webil file and resources.
- The webIL files get uploaded to a website, and are linked simply as:

<script type='webIL' src='/myApp.webil'></script>

- The WebIL runtime, also included on the website, loads the webIL file(s).
- The polyfill emits either normal Javascript, asm.js or WebAssembly.


<b>Runtime overview</b>

The runtime must remain as minimal as possible. The core of the runtime, "Runtime-Common" selects which output format is most suitable for a particular browser and then includes a second file (such as "Runtime-AsmJS") which deals with the opcode->output process. Core functionality is almost entirely provided by libraries also written in WebIL with special opcodes for JS communications.

<b>Benefits</b>

+ Previous plugin content can continue
Java, .NET (used by Unity3D and Silverlight), Flash etc all use virtual machines. WebIL essentially captures the essence of these and unifies them on the web, with the ideal goal of allowing existing plugin content to be simply converted for it to continue to operate when plugins are dropped from all browsers.

+ Compactness
IL is compact; far more-so than asm.js for example.

+ Platform optimisation
If a platform doesn't support asm.js or WebAssembly for example, it can instead emit vanilla JS. This means it doesn't end up with the enormously bloated overhead of asm.js (or non-support of WebAssembly) and should essentially "just work", reaching maximum available performance on even the oldest of platforms.

+ Code optimisation
Similarly, the IL can be pre-optimised during the (technically simple) conversion to webIL, plus the actual code will have passed through e.g. the Java compiler and have been optimised there too. The runtime will also "feature sniff" as it's emitting the output, meaning there's no overhead of checking what a browser does or does not support during code execution as those checks are performed only once. In other words, if(window.localStorage){}else if(window.File){} etc can be optimised out.

+ Runtime Emit
IL will be emittable at runtime, enabling for example .NET Reflection.Emit to be functional too, also allowing for modular code which can be live updated.

+ Automatic GC
As the runtime performs garbage collection, your code doesn't need to include it.

+ Any other ideas?
