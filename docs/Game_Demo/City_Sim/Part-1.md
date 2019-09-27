# Part I

In this part, we will implement the basics, such as moving, placing, rotating, removing, of buildings and arcball camera.

---

Table of contents:
* [Arcball Camera](docs/Game_Demo/City_Sim/Part-1#Arcball-camera)
* [Placing Moving Rotating Removing](docs/Game_Demo/City_Sim/Part-1#Placing-Moving-Removing-Rotating)
* [Contact collision with building]()

---

# Arcball camera

Arcball rotation is *rotation of an object around a point*.

Here, when the right-mouse-button is pressed-hold, we will `Arcball rotate` the camera with an `empty` as center point.

1. Create an empty `CameraEmpty` and position it to center of world and then set parent of our camera to this empty.
2. Create new haxe trait `CameraController` and assign it to our camera, we will use this to control behavior of our camera.

```haxe
// In CameraController.hx

import iron.Scene;
import iron.system.Input;
import iron.math.Vec4;

class CameraController extends iron.Trait {
    //Get our CameraEmpty
	var cameraEmpty = Scene.active.getEmpty("CameraEmpty");
    //Get mouse
	var mouse = Input.getMouse();

	public function new() {
		super();
        
        notifyOnUpdate(update);
	}

	function update() {
		if(mouse.down("right")){
            // Rotate our empty on z-axis in opposite direction of our mouse-x movement.
            // Mouse movement is divided by 200 to slow the rotation.
			cameraEmpty.transform.rotate(new Vec4(0, 0, 1), -mouse.movementX / 200);
            // calling buildMatrix() updates transform such as loc, rot, scale.
			cameraEmpty.transform.buildMatrix();
			cameraEmpty.transform.rotate(object.transform.world.right(), -mouse.movementY / 200);
			cameraEmpty.transform.buildMatrix();
		}
	}
}
```

You should get this:

![](/../../../docassets/CBS_part1_1.gif ':size=400')

---

# Placing, Moving, Removing, Rotating

For Placing, moving, removing and rotating of buildings, we will use physics `ray-casting`.

First we will `ray-cast` from mouse position on screen to ground and then transform it to world coords, then we will get `hit` result.

!> We are going to hard code many thing, because right now, we just want features to 'work'.

Now:

1. Create a cube `hs`(stand for house), and set its physics as:
	* Physics type -> RigidBody
	* RigidBody Type -> Passive
	* Setting -> Animated
	* Collision shape -> Box
	* Collision Collection -> 2nd Group
	* Collision Filter mask -> 2nd Group

1. Set plane (ground) physics as:
	* Physics type -> RigidBody
	* RigidBody Type -> Passive
	* Collision shape -> Box

?> Collision filter mask will make ray-cast ignore the object.

3. Create new Haxe script `PlayerController` and assign it to scene.

```haxe
//In PlayerController.hx

import armory.trait.physics.PhysicsWorld;
import iron.math.RayCaster;
import iron.math.Vec4;
import iron.system.Input;
import iron.Scene;
import iron.object.Object;

class PlayerController extends iron.Trait {

	var mouse = Input.getMouse();

	var kb = Input.getKeyboard();
	//Get active physics world
	var physics = PhysicsWorld.active;
	//Store hit point location of ray-cast
	var hitPointLoc = new Vec4();

	// Will be used to check if house is select and should it move house
	var houseSelected = false;
	var moveHouse = false;

	public function new() {
		super();

		notifyOnUpdate(update);

	}
	function update() {
		if (mouse.started("right")) {
			selectBuiliding();
		}else if (mouse.started()) {
			unselectBuilding();
		}
		if (houseSelected){
			if (kb.down("m")){
				trace("'M' key pressed");
				moveHouse = true;
			}else if (kb.started("f")){
				removeBuilding();
			}else if (kb.started("r")){
				rotateBuilding();
			}else if (kb.started("p")){
				spawnBuilding();
			}
		}
		if (moveHouse) {
			moveBuilding();
		}
	}
	function selectBuiliding() {
		var start = new Vec4();
		var end = new Vec4();
		var camera = Scene.active.getCamera("Camera");
		// Get Ray-cast direction from start to end with mouse's x, y and camera
		RayCaster.getDirection(start, end, mouse.x, mouse.y, camera);
		// cast ray from camera's location in world space to end vec.
		var hit = physics.rayCast(camera.transform.world.getLoc(), end, 2);
		// Check if hit isn't null, and get rigidbody of where the ray is hitted.
		var rigidbody = (hit != null) ? hit.rb : null;
		//Check if rigidbody isn't null and rigidbody's name is 'hs'
		if(rigidbody != null && rigidbody.object.name == "hs"){
			houseSelected = true;
		}else {
			houseSelected = false;
		}
	}

	function unselectBuilding() {
		houseSelected = false;
		moveHouse = false;
	}

	function moveBuilding() {
		var start = new Vec4();
		var end = new Vec4();
		var camera = Scene.active.getCamera("Camera");
		var house = Scene.active.getChild("hs");
		RayCaster.getDirection(start, end, mouse.x, mouse.y, camera);
		var hit = physics.rayCast(camera.transform.world.getLoc(), end, 1, 1);
		var rigidbody = (hit != null) ? hit.rb : null;
		if(rigidbody != null && rigidbody.object.name == "Ground") {
			// Set location of house as floor of ray hit position's x, y and z as 0.4.
			hitPointLoc.set(Math.floor(hit.pos.x), Math.floor(hit.pos.y), 0.4);
			house.transform.loc.setFrom(hitPointLoc);
		}
	}
	function removeBuilding() {
		//Get hs from active scene and remove it
		Scene.active.getChild("hs").remove();
	}
	function rotateBuilding() {
		//Get hs from active scene and rotate it on z-axis by 90 degrees
		Scene.active.getChild("hs").transform.rot.fromEuler(0.0, 0.0, 90*3.14/180);
	}
	function spawnBuilding() {
		// Spawn object hs and set location to hitPointLoc
		Scene.active.spawnObject("hs", null, function(bld: Object){
			bld.transform.loc.setFrom(hitPointLoc);
			bld.transform.buildMatrix();
		}, false);
	}

```

?> We floor the hit.pos.x and hit.pos.y for grid snapping effect

# Building on-contact

Now, here we will get contact between building, and if in contact than it will not us spawn anymore buildings.

```haxe
//In PlayerController.hx

import armory.trait.physics.RigidBody;
	~
class PlayerController extends iron.Trait {
	~
	var inContact = false;

	public function new() { ~ }

	function update() {
		if (mouse.started("right")){ ~ }
		else if (mouse.started()) { ~ }

		if (houseSelected){
			if (kb.down("m")){
				trace("'M' key pressed");
				moveHouse = true;
			}else if (kb.started("f")){
				removeBuilding();
			}else if (kb.started("r")){
				rotateBuilding();
			}
			//If isn't inContact then let it spawn by pressing `p`
			if (!inContact){
				if (kb.started("p")){
					spawnBuilding();
				}
			}
		}
		if (moveHouse){ ~ }
		// Check buildingContact each frame
		buildingContact();
	}
	function selectBuiliding() { ~ }
	function unselectBuilding() { ~ }
	function moveBuilding() { ~ }
	function removeBuilding() { ~ }
	function rotateBuilding() { ~ }
	function spawnBuilding() { ~ }

	function buildingContact() {
		//Get array of contacts of hs object
		var contact = physics.getContacts(Scene.active.getChild("hs").getTrait(RigidBody));
		// If isn't null, than set inContact to true, else set it to false.
		if (contact != null){
			inContact = true;
		}else{
			inContact = false;
		}
	}
}
```

Playing it now should get you:


# Everything for all buildings

We will remove hard coded `hs` building and add for all other.
1. Rename cube `hs` to `bld_hs`.
2. Create more cube `bld_pk`, `bld_gd`, `bld_sc`, `bld_sm`, `bld_qa`, `bld_sw`, `bld_pw` and set their physics as:
	* Physics type -> RigidBody.
	* RigidBody Type -> Passive.
	* Setting -> Animated.
	* Collision shape -> Box.
	* Collision Collection -> 2nd Group.
	* Collision Filter mask -> 2nd Group.

3. Create haxe script `BuildingsController`, we will use this to control buildings

```haxe
//In BuildingsController
// Define typedef Buildings(kind of like enum for other language)
typedef Building = {
	var name: String;
}

class BuildingsController extends iron.Trait {
	//Declare arrays of buildings
	public var buildings: Array<Building> = [];
	//Declare selectedBuilding, i.e., name of building currently selected.
	public var selectedBuilding:String = "";
	//Is building selected?
	public var isBuildingSelected = false;
	//Building's Id, eg: bld_hs1, bld_pw2, etc.
	public var buildingId = 0;

	public function new() {
		super();
	}
	//Get name of all building in buildings array
	public function getBuildingName():String {
		var name:String = "";
		for (building in buildings){
			name = building.name;
		}
		return name;
	}
	//Add building to buildings array
	public function addBuilding(name: String) {
		var building: Building = {
			name: name
		}
		buildings.push(building);
	}
}

```
Now we will use this in `PlayerController` and adjust few things:

```haxe
//In PlayerController
	~
import arm.BuildingsController;

class PlayerController extends iron.Trait {
	~
	var buildings = new BuildingsController();

	var buildingType: String = "hs";

	public function new() { ~ }

	function update() {
		if (mouse.started("right")){
			selectBuiliding();
		}else if (mouse.started()) {
			if (!inContact){
				unselectBuilding();
			}
		}
		if (buildings.isBuildingSelected){
			buildingContact();
			if (kb.down("m")){
				moveHouse = true;
			}else if (kb.started("f")){
				removeBuilding();
			}else if (kb.started("r")){
				rotateBuilding();
			}
		}
		if (kb.started("p")){
			if (!inContact){
				spawnBuilding(buildingType);
			}
		}
		if (moveHouse){ ~ }

		//Add building selecting for spawning, this will be change later to use UI instead 
		if (kb.started("1")) buildingType = "hs";
		else if (kb.started("2")) buildingType = "pk";
		else if (kb.started("3")) buildingType = "gd";
		else if (kb.started("4")) buildingType = "sc";
		else if (kb.started("5")) buildingType = "sm";
		else if (kb.started("6")) buildingType = "qa";
		else if (kb.started("7")) buildingType = "sw";
		else if (kb.started("8")) buildingType = "pw";
	}

	function selectBuiliding() {
		~
		//Check if rigidbody isn't null and rigidbody's name start with "bld"
		if(rigidbody != null && StringTools.startsWith(rigidbody.object.name, "bld")){
			//Set selectedbuilding to rigidbody's name and set isBuildingSelected to true.
			buildings.selectedBuilding = rigidbody.object.name;
			buildings.isBuildingSelected = true;
		}else {
			buildings.selectedBuilding = null;
			buildings.isBuildingSelected = false;
		}
	}

	function unselectBuilding() {
		buildings.selectedBuilding = null;
		buildings.isBuildingSelected = false;
		moveHouse = false;
	}

	function moveBuilding() {
		~
		if(rigidbody != null && rigidbody.object.name == "Ground") {
			hitPointLoc.set(Math.floor(hit.pos.x), Math.floor(hit.pos.y), 0.4);
			//Set transform of selected building
			Scene.active.getChild(buildings.selectedBuilding).transform.loc.setFrom(hitPointLoc);
		}
	}

	function removeBuilding() {
		//Removes selected building
		Scene.active.getChild(buildings.selectedBuilding).remove();
	}

	function rotateBuilding() {
		//Rotates selected building
		Scene.active.getChild(buildings.selectedBuilding).transform.rot.fromEuler(0.0, 0.0, 90*3.14/180);
	}

	//Spawn building with type
	function spawnBuilding(type: String) {
		Scene.active.spawnObject("bld_"+type, null, function(bld: Object){
			//Increment buildingId, everytime a new building is spawned
			buildings.buildingId++;
			bld.transform.loc.set(0.0, 0.0, 0.0);
			bld.transform.buildMatrix();
			//Change name of building and add id to it
			bld.name = "bld_"+type+buildings.buildingId;
			//Add building to buildings array
			buildings.addBuilding(bld.name);
		}, false);
	}

	function buildingContact() { ~ }
}
```


!> W.I.P