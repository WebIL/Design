The WebIL pipeline works roughly like the following:

- A compiler which converts, for example, .NET DLLs into a .webil file and resources.
- WebIL files are internally almost the same as [wasm](https://github.com/WebAssembly/design/blob/master/BinaryEncoding.md) ones, but may use a more GC specific set of opcodes. This project essentially encapsulates the higher level aspects such as GC and the conversion of higher level code (such as Java or .NET and their libraries). Think of it as an extension to WebAssembly, whereby .webil files always use GC.
- The webIL files get uploaded to a website, and are linked simply as:

&lt;script type='webIL' src='/myApp.webil'&gt;&lt;/script&gt;

- The WebIL runtime, also included on the website, loads the webIL file(s).
- The polyfill emits either normal Javascript, WebAssembly+GC or asm.js+GC (via the wasm polyfill).


<b>Runtime overview</b>

The runtime must remain as minimal as possible. The core of the runtime, "Runtime-Common" selects which output format is most suitable for a particular browser and then includes a second file (such as "Runtime-AsmJS") which deals with the output process. Core functionality is almost entirely provided by libraries which will often be specific to a particular higher level language.

<b>Benefits</b>

+ Previous plugin content can continue. 
Java, .NET (used by Unity3D and Silverlight), Flash etc all use virtual machines. WebIL essentially captures the essence of these and unifies them on the web, with the ideal goal of allowing existing plugin content to be simply converted for it to continue to operate when plugins are dropped from all browsers.

+ Compactness. 
Based off wasm and is inherently compact; far more-so than asm.js for example.

+ Platform optimisation. 
If a platform doesn't support asm.js or WebAssembly for example, it can instead emit vanilla JS. This means it doesn't end up with the enormously bloated overhead of asm.js and should essentially "just work", reaching maximum available performance on even the oldest of platforms.

+ Code optimisation. 
Similarly, code can be pre-optimised during the conversion to webIL, plus the actual code will have passed through e.g. the Java compiler and have been optimised there too. The runtime will also "feature sniff" as it's emitting the output, meaning there's no overhead of checking what a browser does or does not support during code execution as those checks are performed only once. In other words, if(window.localStorage){}else if(window.File){} etc as commonly seen in Javascript can be optimised out.

+ Runtime Emit. 
IL will be emittable at runtime, enabling for example .NET Reflection.Emit to be functional too, also allowing for modular code which can be live updated.

+ Automatic GC. 
As the runtime performs garbage collection, your code doesn't need to include it.

+ ..Any other ideas?
