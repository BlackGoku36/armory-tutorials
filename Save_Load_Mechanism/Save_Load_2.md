---
layout: default
title: Part-II
parent: Save Load Mechanism
nav_order: 2
---

## Part II

In this second part, we will write and read cube's Location and rotation to/from the `save_game.json` in bundled. At the end of this part you should be able to get saving/loading working.

---

I created simple scene, consisting of cube, plane with Hosek-Wilkie hdr(background hdr is not necessary, it is for eye-candy only).

![scene](/../Assets/save_load_7.png)

Now, select our default cube and head over to `Object - Armory Traits` to create a new Haxe trait and name it `CubeController`(or whatever you want). We will make it move on X-Y axis and then make it rotate randomly so, that we can demostrate saving/loading mechanism.

```
// In CubeController.hx

package arm;

import iron.math.Vec4;
import iron.system.Input;

class CubeController extends iron.Trait {
	var kb = Input.getKeyboard();
	public function new() {
		super();

		notifyOnUpdate(function() {
			if(kb.down("up")) object.transform.translate(-0.2, 0.0, 0.0);// 1
			else if (kb.down("down")) object.transform.translate(0.2, 0.0, 0.0);// 1
			else if (kb.down("left")) object.transform.translate(0.0, -0.2, 0.0);// 1
			else if (kb.down("right")) object.transform.translate(0.0, 0.2, 0.0);// 1
			else if (kb.down("r")) object.transform.rotate(new Vec4(Math.random(), Math.random(), Math.random()), 0.1);// 2
		});
	}
}
```
1. We check if player if pressing `up arrow` than it will translate the object(self/default cube) -0.2 on X-axis than if pressing `down arrow` than it will translate 0.2 on x-axis, same with rest but on y-axis.
2. In rotate(-vec4-, -speed-) function we set all axis in Vec4 to random(`Math.random()` give random float) to make it rotate randomly, and set speed to 0.1.

This should give you following result:

![](/../Assets/save_load_8.mov)

Now to adding cube location and rotation to save_game.json for saving and loading.

Now, go back to `SaveLoadMechanism.hx`, we will get cube's location and rotation and add it to json structure.

```
// In SaveLoadMechanism.hx

package arm;

import iron.Scene;
import iron.math.Vec4;
import iron.system.Input;
import iron.data.Data;

typedef Cube = { loc : Vec4, rot : Vec4 } // 1

class SaveLoadMechanism extends iron.Trait {
                    ~
		notifyOnUpdate(function() {
			var cube = Scene.active.getChild("Cube"); //2
			var cubeLoc = cube.transform.loc;// 2
			var cubeRot = new Vec4(cube.transform.rot.x, cube.transform.rot.y, cube.transform.rot.z); //2
			#if kha_krom
			var saveData: Cube = {loc: cubeLoc, rot: cubeRot}; //3
			var saveDataJSON = haxe.Json.stringify(saveData);
                            ~
					// Get json from string
					var json = haxe.Json.parse(string);// 4
					cube.transform.loc = json.loc; //4
					cube.transform.setRotation(json.rot.x, json.rot.y, json.rot.z);//4
					cube.transform.buildMatrix();//4
				});
			}
		});
	}
}

```

1. We define Cube's [Anonymous Structure](https://haxe.org/manual/types-anonymous-structure.html)(it is kind of like other languages's struct)
2. Here we get Cube from active scene and than we get it Vec4 location and rotation
3. We change json structure to our cube's Anonymous Structure
4. When the json is parsed, it will get cube's location and rotation value and set the cube's location and rotation to this obtained value, hence Loading.

for now, we will have to reopen the game to let game parse newly overwritten game_save.json. We can fix this why hot asset reload.
{: .label .label-yellow}

You should get this as result:

![](/../Assets/save_load_9.mov)

If it work for you, then congrats! Part-II finishes!

---
In last part(aka Part-III) we will finalize it by adding UI

[<-- Part-I](Save_Load_1.md)
[Part-III -->](Save_Load_3.md){: .ml-xl-9}