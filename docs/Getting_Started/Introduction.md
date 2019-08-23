# Introduction

## ARMORY3D

Armory3D is data-driven 3D engine written in [`Haxe`](https://en.wikipedia.org/wiki/Haxe), [`C`](https://en.wikipedia.org/wiki/C_(programming_language)) and [`WebAssembly`](https://en.wikipedia.org/wiki/WebAssembly), comes with Blender embeding but is designed to be able to be integrated into almost any 3d modelling and animation tool like [Blender](https://en.wikipedia.org/wiki/Blender_(software)). It's main focus is on performance, portability and minimal footprint.

### RENDERING
Armory has it own renderer(see Iron). Armory uses Cycles/EEVEE material nodes to build [shaders](https://en.wikipedia.org/wiki/Shader). The [renderpath](https://en.wikipedia.org/wiki/Graphics_pipeline) is completely scriptable, with support of forward and deferred renderpath out-of-box. It is completely possible to write your own shaders(in [glsl](https://en.wikipedia.org/wiki/OpenGL_Shading_Language)).

### SCRIPTING
Scripting is done with `traits`(a conecpt developed in `Iron`: *a piece of logic attached to an object*).

There are 4 different `Traits`:
1. **LOGIC NODES TRAIT**: Traits can be written with `Logic Node`(similar to UE4's Blueprints)
2. **HAXE TRAIT**: Traits can be written in `Haxe`.
3. **CANVAS TRAIT**: Trait for `Canvas`
4. **WEBASSEMBLY(WASM) TRAIT**: Trait written in WebAssembly, `Rust`/`C`/`C++` can be used to write logic in after compiling it to wasm.

### TARGETS
Armory support all targets that Kha support. Platform such as `Desktop(MacOS, Linux, Windows)`, `HTML5`, `Mobile(Android, iOS)` and `consoles(PS4, Xbox One, Switch)` is supported out-of-box.

---

## Technologies

### HAXE

[**HAXE**](https://haxe.org/) is open source cross-platform toolkit based on modern, high-level, strictly typed, multi-paradigm programming language and cross-compiler that can compile code to target platform native source code or binary. Haxe is easy-to-learn if you know java/AS3 and other OOP languages.


### KHA

[**KHA**](http://kha.tech/) is ultra-portable, high performance, open-source multimedia framework. It is low level SDK for building cross-platform games and media applications. With the help of Haxe Programming language and the [Krafix shader-compiler](https://github.com/Kode/krafix) it can cross-compile your code and optimize your assets. Kha gives you a common API to graphics, audio, input, and networking, for platforms such as Web, Mobile, Desktop, Consoles, [etc](https://github.com/Kode/Kha/wiki/Features#supported-platforms) and Graphics APIs such as Metal, Vulkan, DirectX, WebGL and OpenGL. 


### IRON

[**IRON**](https://github.com/armory3d/iron) is data-based, asynchronous engine used for building 3D tools, It is built with Kha, Haxe and WebAssembly. It is core engine of Armory but is designed to run stand-alone. Iron handles render & content pipelines and lets you develop a custom visual engine on top of it.