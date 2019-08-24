## Part-III

In this third and the last part we will add UI to our game for gamey effect.

---

Move over to `Scene - Armory Scene Trait` and create a new Trait but this time select `UI`, click `New Canvas` and as always name it whatever you want(`SaveLoad` for me)

![saveloadcanvas](/../../docassets/save_load_10.png)

And then finally click `Edit Canvas`, a new window should pop-up named `Armory2D`:

![armory2d](../../docassets/armory2D.png)

And do the following:

![canvasbtn](/../../docassets/save_load_12.png)

1. This create a button, create 2 button and place them where you want them to be(mine in the center).
2. This is will name the button, name them.
3. This will set the text of the button, label them.
4. This will anchor the button, selecting `Center` will make the button to center, irrespective of the window size, anchor them.
5. This is for button's event, we will use this for calling the button's event from our haxe script, we will name it `save_btn` and `load_btn` for respective button.
6. Finally save it.

and if you play it, you should get something like this:

![menu](/../../docassets/save_load_13.png)

Now to button functionality, we will implement it in `SaveLoadMechanism.hx`

```haxe
// In SaveLoadMechanism.hx
package arm;

import armory.system.Event;//<===
import iron.Scene;
                ~
class SaveLoadMechanism extends iron.Trait {
                ~
	public function new() {
		super();

		notifyOnInit(function(){// 1
			Event.add("save_btn", save);// 2
			Event.add("load_btn", load);// 2
		});

	}
                ~
```
1. We change notifyOnUpdate to notifyOnInit
2. Adding Event listener. In Event.add(`*name*`, `*function*`), `*name*` is name the of event, like we added to buttons to canvas in Armory2D(__Flashback__), and `*function*` is what it should do after it getting notified of event. So, here it will call function `save/load` after getting notified about event `save_btn/load_btn`.

For our last and final feature, we are going to hide buttons on start and then we will able to show and hide them with `m`.

```haxe
// In SaveLoadMechanism.hx

			~
import armory.trait.internal.CanvasScript;// 1

typedef Cube = { loc : Vec4, rot : Vec4 }

class SaveLoadMechanism extends iron.Trait {
				~

	var canvas:CanvasScript;// 1

	var isButtonsHidden:Bool;// 2

	public function new() {
		super();

		notifyOnInit(function(){
			canvas = Scene.active.getTrait(CanvasScript);// 1
			hideButtons(); // 3
			isButtonsHidden = true;// 3
						~
		});
		// 4
		notifyOnUpdate(function (){
			if (kb.started("m")){
				if (isButtonsHidden){
					showButtons();
				}else{
					hideButtons();
				}
			}
		});
	}

	public function save(){
		~
	}

	public function load() {
		~
	}
	// 5
	public function hideButtons() {
		canvas.getElement("save_btn").visible = false;
		canvas.getElement("load_btn").visible = false;
		isButtonsHidden = true;
	}
	// 5
	public function showButtons() {
		canvas.getElement("save_btn").visible = true;
		canvas.getElement("load_btn").visible = true;
		isButtonsHidden = false;
	}
}

```
1. Import, initialize `CanvasScript` trait.
2. Create `isHiddenButton` Bool.
3. We hide buttons and set `isButtonsHidden` to true on init.
4. We create `notifyOnUpdate()` function and add key check for `m` and on key `m` started it will check if the button is hidden, if so than it will call `showButtons()`, if not then it will call `hideButtons()`.
5. We create `hideButtons()`/`showButtons()` and then get canvas's element by name(name that we gave to butttons in Canavs __Flashback__) and then get it visible property and set it to `true`/`false` respective to the function, and then finally set `isButtonsHidden` to `true`/`false` again respective to the function.

You should get the following result:

<iframe width="480" height="360" src="https://blackgoku36.github.io/armory-tutorials/docassets/save_load_final.mp4" frameborder="0"> </iframe>


🎉There we go! Save and Load mechanism tutorial is over!🎉

---

For further improving, you can:
1. Make the file secure, so that people don't cheat.
2. Add more value for it to save.
3. Make GTA XII :trollface: