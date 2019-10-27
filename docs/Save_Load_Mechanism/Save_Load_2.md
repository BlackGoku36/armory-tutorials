## Save & Load game data

In this second part, we will write and read cube's Location and rotation to/from the `save_game.json` in bundled. At the end of this part you should be able to get saving/loading working.

---

Select our default cube and create another Haxe trait(`CubeController`). We will make it move on X-Y axis and then make it rotate randomly so, that we can demostrate saving/loading mechanism.

```haxe
// In CubeController.hx

package arm;

import iron.math.Vec4;
import iron.system.Input;

class CubeController extends iron.Trait {
	var kb = Input.getKeyboard();
	public function new() {
		super();

		notifyOnUpdate(function() {
			//Translates (x, y, z) object with according to the key press.
			if(kb.down("up")) object.transform.translate(-0.2, 0.0, 0.0);
			else if (kb.down("down")) object.transform.translate(0.2, 0.0, 0.0);
			else if (kb.down("left")) object.transform.translate(0.0, -0.2, 0.0);
			else if (kb.down("right")) object.transform.translate(0.0, 0.2, 0.0);
			//Rotate randomly with 0.1 speed.
			else if (kb.down("r")) object.transform.rotate(new Vec4(Math.random(), Math.random(), Math.random()), 0.1);
		});
	}
}
```

This should give you following result:

<iframe width="480" height="360" src="https://blackgoku36.github.io/armory-tutorials/docassets/save_load_8.mp4" frameborder="0"> </iframe>

Now to adding cube location and rotation to save_game.json for saving and loading.

Go back to `SaveLoadMechanism.hx`, we will get cube's location and rotation and add it to json structure.

```haxe
// In SaveLoadMechanism.hx

package arm;

import iron.Scene;
import iron.math.Vec4;
import iron.system.Input;
import iron.data.Data;

//Define Anonymous Structure (kind of like 'enums' for other languages) with Vec4 location and rotation.
typedef Cube = { loc : Vec4, rot : Vec4 }

class SaveLoadMechanism extends iron.Trait {

	var kb = Input.getKeyboard();

	var saveFile = "save_game.json";

	public function new() {
		super();

		notifyOnUpdate(function() {
			if(kb.started("f")){
				save();
			}else if(kb.started("g")){
				load();
			}
		});
	}
	public function save(){
		//Get 'Cube' from active Scene.
		var cube = Scene.active.getChild("Cube");
		//Get cube's location.
		var cubeLoc = cube.transform.loc;
		//Get cube's rotation.
		var cubeRot = new Vec4(cube.transform.rot.x, cube.transform.rot.y, cube.transform.rot.z);

		#if kha_krom
		//Set cube's loc and rot to anonymous structure defined above.
		var saveData: Cube = {loc: cubeLoc, rot: cubeRot};
		var saveDataJSON = haxe.Json.stringify(saveData);

		var path = Krom.getFilesLocation() + "/../../../" + "/Bundled/save_game.json";
		var bytes = haxe.io.Bytes.ofString(saveDataJSON);

		Krom.fileSaveBytes(path, bytes.getData());
		trace("Saved!");
		#end
	}

	public function load() {
		var cube = Scene.active.getChild("Cube");
		Data.getBlob(saveFile, function(b:kha.Blob) {

			var string = b.toString();
			var json = haxe.Json.parse(string);

			//Set cube's location and rotation.
			cube.transform.loc = json.loc;
			cube.transform.setRotation(json.rot.x, json.rot.y, json.rot.z);
			//builds matrix (kind of like applying changes).
			cube.transform.buildMatrix();
		});
	}
}
```

We can now save/load our cube location and rotation by parsing/writing to json with key press.

!> For now, we will have to reopen the game to let game parse newly overwritten game_save.json.

You should get this as result:

<iframe width="480" height="360" src="https://blackgoku36.github.io/armory-tutorials/docassets/save_load_9.mp4" frameborder="0"> </iframe>

If it work for you, then congrats! Part-II finishes!

In last part(aka Part-III) we will finalize it by adding UI
