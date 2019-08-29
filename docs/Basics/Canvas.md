# Canvas
In this tutorial, we will create canvas using Armory2D, learn about it and then add and control canvas's element using `haxe`.

Our Goal:
<iframe width="640" height="480" src="../../docassets/finalcanvas.mp4" frameborder="0"></iframe>

---
Fire Armory3D project, once you have it up, go to `Scene - Armory Scene Trait`, create new `Canvas Trait`, name it `UI` and hit `Edit Canvas`. A window named Armory2D should pop-up:

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

!> We set text of our combo and radio as `option1;option2;option3` because it seperate options by `;`.

When you run the game, you should see something like this:

![](../../docassets/canvas_3.png ':size=700')
   
---

Now, we will create `Haxe Trait` to control our UI created in `UI` Canvas. Head over to `Scene - Armory Scene Trait` in Blender and create new `Haxe Trait` and name it `CanvasController` and hit `Edit Script`.

```haxe
// In CanvasController.hx
package arm;

//Imports 1
import iron.Scene;
import armory.trait.internal.CanvasScript;
import armory.system.Event;

class CanvasController extends iron.Trait {

	var canvas:CanvasScript;// 2

	public function new() {
		super();

		notifyOnInit(function() {
			canvas = Scene.active.getTrait(CanvasScript);// 2
			Event.add("apply_btn", applyCanvas);// 3
		});

	}

	public function applyCanvas() {// 3
		trace("Applied!");
	}
}
```
1. We import `Scene`, `CanvasScript` and `Event`.
2. Initialize canvas variable, assign and get `CanvasScript` Trait attached to Scene, i.e., our `UI` Canvas Trait.
3. We add `apply_btn` event on init and call `applyCanvas()` when done. And then finally trace `Applied!` when called.

If you were to run this and press `Apply` button, it should print `Applied!`. If it do, then our basics are working and we are ready to do more work.

Now to change color of text using `slider_r`/`slider_g`/`slider_b` silder and enable/disable visibility using `visibility` checkbox:
```haxe
//In CanvasController.hx
~
// 1
import kha.Color;
import zui.Canvas.TElement;
~
class CanvasController extends iron.Trait {

	var canvas:CanvasScript;
	var text:TElement;// 2

	public function new() {
		super();

		notifyOnInit(function() {
			canvas = Scene.active.getTrait(CanvasScript);
			text = canvas.getElement("txt");// 2
			Event.add("apply_btn", applyCanvas);
		});
	}

	public function applyCanvas() {
		set_rgb();// 7
		set_visibility();// 7
	}

	public function set_rgb() {
		var r = canvas.getHandle("slider_r").value;// 3
		var g = canvas.getHandle("slider_g").value;
		var b = canvas.getHandle("slider_b").value;
		text.color = Color.fromFloats(r, g, b);// 4
	}

	public function set_visibility() {
		var is_selected = canvas.getHandle("visibility").selected;// 5
		// 6
		if (!is_selected){
			text.visible = false;
		}else{
			text.visible = true;
		}
	}
}
```
1. We import `kha.Color`*(color handling by Kha)* and `zui.Canvas.TElement`*(Element handling)*.
2. We declare text as `TElement` and initialize it as canvas element `txt`.*(canvas.getElement("txt")* get element of canvas by name).
3. We get input value of slider using `canvas.getHandle("slider_r/g/b").value` and assign it to var r/g/b.
4. We set color of `text` using `Color.fromFloats(*r*,*g*,*b*,*a*)`*(alpha value is defaultly set to 1.0, you can change it by adding value)*.
5. Inside, `set_visibility()`, we get selected bool and assign it to `is_selected`
6. We check if `is_selected` is true or false and then set text visbility according to it.
7. Finally call `set_rgb()`/`set_visibility` in `applyCanvas()`.
   
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
		var input_txt = canvas.getHandle("input_txt").text;// 1
		var caseSensitivity = canvas.getHandle("dd_case").position;// 2
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
1. Get text value from `input_txt` and assign to `input_txt`.
2. Get position value from `dd_case` and assign it to `caseSensitivity` *(position get value of select value in order, i.e., `Default Case` = 0, `Upper Case` = 1, `Lower Case` = 2)*
3. check the value of `caseSensitivity`, then change case sensitivity according to it using `toUpperCase()`/`toLowerCase()` or else keep the case.

And lastly rotation of Text `txt` using Radio `visibility`:

```haxe
~
class CanvasController extends iron.Trait {
	~
	public function new() { ~ }

	public function applyCanvas() {
		~
		set_rotation();// 3
	}

	public function set_rgb() { ~ }

	public function set_visibility() { ~ }

	public function set_input_txt() { ~ }

	public function set_rotation(){
		var rotate = canvas.getHandle("rotate").position;// 1
		if (rotate == 0){// 2
			text.rotation = 0;
		}else if (rotate == 1){
			text.rotation = 90*3.14/180;
		}else if (rotate == 2){
			text.rotation = 180*3.14/180;
		}
	}
}
```
1. Get `rotate` position value and assign it to rotate.
2. Set text rotation according to the value.
3. Finally call `set_rotation()` in `apply_canvas()`.
4. *`degrees x 3.14/180`, will convert degrees to radians that armory uses*.

Now on playing it, you should get our goal.

**🎉There we go, our tutorial is finished 🎉**

---
If anything goes wrong, then you can check the [source code](https://github.com/BlackGoku36/armory-tutorial-download/tree/master/Canvas)