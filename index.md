## Armory3D

wip
{: .label .label-red}

### What is it?
Armory3D is data-driven 3D engine written in Haxe, C and WebAssembly, comes with blender embeding but is designed to be able to be integrated into almost any 3d modelling and animation tool like blender. It's main focus is on performance, portability and minimal footprint.

### Rendering
Armory has it own renderer(see Iron). Armory uses Cycles/EEVEE material nodes to build shaders.

### Scripting
Scripting in armory is done with Haxe, Logic Nodes and C/C++/Rust/WebAssembly.

---

## Technologies

### Haxe

[Haxe](https://haxe.org/) is open source cross-platform toolkit based on modern, high-level, strictly typed, multi-paradigm programming language and cross-compiler that can compile code to target platform native source code or binary. Haxe is easy-to-learn if you know java/AS3 and other OOP languages.


### Kha

[Kha](http://kha.tech/) is ultra-portable, high performance, open-source multimedia framework. It is low level SDK for building cross-platform games and media applications. With the help of Haxe Programming language and the [Krafix shader-compiler](https://github.com/Kode/krafix) it can cross-compile your code and optimize your assets. Kha gives you a common API to graphics, audio, input, and networking, for platforms such as Web, Mobile, Desktop, Consoles, [etc](https://github.com/Kode/Kha/wiki/Features#supported-platforms) and Graphics APIs such as Metal, Vulkan, DirectX, WebGL and OpenGL. 


### Iron

[Iron](https://github.com/armory3d/iron) is data-based, asynchronous engine used for building 3D tools, It is built with Kha, Haxe and WebAssembly. It is core engine of Armory but is designed to run stand-alone. Iron handles render & content pipelines and lets you develop a custom visual engine on top of it.


wip
{: .label .label-red}
