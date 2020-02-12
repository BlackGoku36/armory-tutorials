# Canvas

!> THIS IS DATED! ARMORY2D HAS EVOLVED A LOT SINCE WRITING THIS!

In this tutorial, we will create canvas using Armory2D, learn about it and then add and control canvas's element using `haxe`.

Our Goal:
<iframe width="640" height="480" src="https://blackgoku36.github.io/armory-tutorials/docassets/finalcanvas.mp4" frameborder="0" allowfullscreen></iframe>

---
Fire up Armory3D project, once you have it up, go to `Scene - Armory Scene Trait`, create new `Canvas Trait`, name it `UI` and hit `Edit Canvas`. A window named Armory2D should pop-up:

![Armory2d Image](../../docassets/armory2D.png ':size=700')

---

Let study its user interface:

![Canvas's Layout Image](../../docassets/CanvasLayout.png ':size=700')

1. This is where you can add elements from.
2. This is `Canvas`'s area, where you arrange for you window.
3. This is `Project` tab, where you configure you elements.
   1. `Save`: Saves the canvas.
   2. `Canvas`: Create new canvas with given name, width and height.
   3. `Outliner`: This is layer. You can press `Up`/`Down` to move it up/down the layer, `Remove` to remove the element, `Duplicate`, to duplicate. (Pretty obvious)
   4. `Properties`: Adjust visibility, name, text, transform, color of an selected element.
   5. `Align`: *(WIP)*.
   6. `Anchor`: Anchor element to selected side.
   7. `Script`: This is used to handle event.
   8. `Timeline`: *(WIP)*.


4. This is `Assets` tab, where you import assets in:
   1. `Import Assets`: Import assets using zui's file dialogue.
   2. `X`: Remove assets.

5. This is `Preferences` tab, where you configure Armory2D:
   1. `UI Scale`: Scales the UI.
   2. `Grid Size`: Scale the grid.
   3. `Grid Snap Position:` Snap element to grid position.
   4. `Grid Snap Bounds:` Snap element to grid bounds.
   5. `VSync`: Enable/disable [VSync](https://en.wikipedia.org/wiki/Screen_tearing).
   6. `Console`: *(WIP)*.

6. This is Timeline, used to animate elements, but currently it is WIP, so we will ignore it for now.

Let add elements:

![](../../docassets/canvas_2.png ':size=700')

1. `Text`: Add Text `txt`, We will control this by using below UI (sliders, input, etc).
2. `Sliders`: Add 3 sliders `slider_r`, `slider_g`, `slider_b` to control rgb value of the `txt`.
3. `Input`: Add input `input_txt`, we will use this to controll the text of `txt`.
4. `Combo`: Add combo(aka dropdown), set text as `Default Case;Upper Case;Lower Case`,we will use it make our `txt` case-sensitive or not.
5. `Radio`: Add radio, `rotate`, set text as `Rotate 90;Rotate 180;Rotate 0`,we will use this set rotation of our text.
6. `Check`: Add checkbox `visibility`, we will use this to make `txt` visible/invisible.
7. `Button`: Add button `apply_btn`, and set `apply_btn` in `Script - Event`,we will use this to apply the change made using UI.
8. `Image`: Add `logo`, we use this to for image assets.
9. `Shape`: Add `h_1`, we use this add shape.

?> We set text of our combo and radio as `option1;option2;option3` because it seperate options by `;`.

When you run the game, you should see something like this:

![](../../docassets/canvas_3.png ':size=700')
   
---

Now, we will create `Haxe Trait` to control our UI created in `UI` Canvas. Head over to `Scene - Armory Scene Trait` in Blender and create new `Haxe Trait` and name it `CanvasController` and hit `Edit Script`.

```haxe
// In CanvasController.hx
package arm;

import iron.Scene;
import armory.trait.internal.CanvasScript;
import armory.system.Event;

class CanvasController extends iron.Trait {

	// Initialize canvas as CanvasScript.
	var canvas:CanvasScript;

	public function new() {
		super();

		notifyOnInit(function() {
			// Set canvas as CanvasScript trait from active scene.
			canvas = Scene.active.getTrait(CanvasScript);
			// Add event listener with string 'apply_btn' and call 'applyCanvas' function.
			Event.add("apply_btn", applyCanvas);
		});

	}

	public function applyCanvas() {
		trace("Applied!");
	}
}
```

If you were to run this and press `Apply` button, it should print `Applied!`. If it do, then our basics are working and we are ready to do more work.

Now to change color of text using `slider_r`/`slider_g`/`slider_b` silder and enable/disable visibility using `visibility` checkbox:
```haxe
//In CanvasController.hx
~

import kha.Color;
import zui.Canvas.TElement;
~
class CanvasController extends iron.Trait {

	var canvas:CanvasScript;
	// Initialize text as TElement.
	var text:TElement;

	public function new() {
		super();

		notifyOnInit(function() {
			canvas = Scene.active.getTrait(CanvasScript);
			//get element from canvas with string 'txt'.
			text = canvas.getElement("txt");
			Event.add("apply_btn", applyCanvas);
		});
	}

	public function applyCanvas() {
		//Only call set_'s funtions when button is pressed.
		set_rgb();
		set_visibility();
	}

	public function set_rgb() {
		//Get value from UI using getHandle().
		var r = canvas.getHandle("slider_r").value;
		var g = canvas.getHandle("slider_g").value;
		var b = canvas.getHandle("slider_b").value;
		//Set text color from float r,g,b.
		text.color = Color.fromFloats(r, g, b);
	}

	public function set_visibility() {
		//Get checkbox's selected bool value.
		var is_selected = canvas.getHandle("visibility").selected;
		// if isn't selected than keep invisible else visible.
		if (!is_selected){
			text.visible = false;
		}else{
			text.visible = true;
		}
	}
}
```
   
Now we will get input from `input_txt` and case sensitivity from `dd_case` and set Text `txt` according to it.

```haxe
//In CanvasContoller.hx
~
class CanvasController extends iron.Trait {
	~
	public function new() { ~ }

	public function applyCanvas() {
		set_rgb();
		set_visibility();
		set_input_txt();// 4
	}

	public function set_rgb() { ~ }

	public function set_visibility() { ~ }

	public function set_input_txt() {
		//Get input value.
		var input_txt = canvas.getHandle("input_txt").text;
		//Get selected value from drop down with position.
		var caseSensitivity = canvas.getHandle("dd_case").position;
		// 3
		if (caseSensitivity == 1){
			text.text = input_txt.toUpperCase();
		}else if (caseSensitivity == 2){
			text.text = input_txt.toLowerCase();
		}else{
			text.text = input_txt;
		}
	}
}
```

And lastly rotation of Text `txt` using Radio `visibility`:

```haxe
~
class CanvasController extends iron.Trait {
	~
	public function new() { ~ }

	public function applyCanvas() {
		~
		set_rotation();
	}

	public function set_rgb() { ~ }

	public function set_visibility() { ~ }

	public function set_input_txt() { ~ }

	public function set_rotation(){
		//Just like dropdown's case use position to get selected value.
		var rotate = canvas.getHandle("rotate").position;
		//Set text's rotation according to selected radio's value.
		if (rotate == 0){
			text.rotation = 0;
		}else if (rotate == 1){
			text.rotation = 90 * 3.14 / 180;
		}else if (rotate == 2){
			text.rotation = 180 * 3.14 / 180;
		}
	}
}
```
?> Armory uses radians almost everywhere so be sure to convert it to degrees.

Now on playing it, you should get our goal.

**🎉There we go, our tutorial is finished 🎉**

---
If anything goes wrong, then you can check the [source code](https://github.com/BlackGoku36/armory-tutorial-download/tree/master/Canvas)