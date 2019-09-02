# Traits

In this tutorial, we will learn about what is Traits, what are different types, how to create them and how to print `Hello World` from different traits.

---

`Trait` is conecpt in `Iron` means a piece of logic attached to object or scene, just like traits of humans that is it characteristic.

There are 4 different types of `traits`:
1. **Haxe Trait**: [Haxe](https://haxe.org/) scripting trait that can be used to write game logic in.

2. **Logic Nodes Trait**: It is visual scripting trait that can also be used to write game logic in, it is similar to [UE4's Blueprints](https://docs.unrealengine.com/en-US/Engine/Blueprints/index.html).

3. **Canvas Trait**: This is trait for UI, canvas can be made using [Armory2D](https://github.com/armory3d/armory2d), but you can create it yourself with Armory2D as canvas use `.json`.

4. **[WebAssembly(WASM)](https://webassembly.org/)**: This is also scripting trait, you can write your game logic in [`Rust`](https://www.rust-lang.org/)/[`C`](https://en.wikipedia.org/wiki/C_%28programming_language%29)/[`C++`](https://en.wikipedia.org/wiki/C%2B%2B) and than compiling the code to `WASM` to use it in Armory.

---

Let's get started, fire up armory3D project and head over to `Scene - Armory Scene Trait`.
We will create Trait in following order:

1. [Haxe](#haxe)
2. [Logic Node](#logic-node)
3. [Wasm](#wasm)
4. [Canvas](#canvas)

## Haxe
1. Click `+`, select `Haxe` and click `OK`, `Haxe` trait placeholder should appear.
2. Click `New Script` and name the script of your choice, just make sure the first letter is capital(`HaxeScript` for me).
3. Finally hit `Edit Script`, you system default IDE should now open up with the project.

Edit the script:
```haxe
// In HaxeScript.hx
package arm;

class HaxeScript extends iron.Trait {
	public function new() {
		super();

		notifyOnInit(function() {// 1
            trace("Hello World from Haxe!");// 2
		});

		notifyOnUpdate(function() {// 3
		});

		notifyOnRemove(function() {// 4
		});
	}
}
```
1. `notifyOnInit(*function*)`, will call `*function*` when the trait it is attached to initialised, in our instance, it will call the `*function*` when the scene initialises(remember? the haxe trait it attached to scene?).
2. Print "Hello World from Haxe!" when initialised.
3. `notifyOnUpdate(*function*)`, will call `*function*` every tick.
4. `notifyOnRemove(*function*)`, will call `*function*` when the trait it is attached to get removed/destroyed in game.

Now, if you were to play it, you should see `Hello World from Haxe!` in console.

## Logic Node

## Wasm

## Canvas