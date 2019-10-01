# Basics

In `Basics` part we will do :
    * Create Arcball style rotating camera, to look around the buildings.
    * Create core of Selecting, moving, placing, removing, rotating and on-contact with other buildings.

---

Table of contents:
* [Arcball Camera](docs/Game_Demo/City_Sim/Part-1#Arcball-camera)
* [Placing Moving Rotating Removing](docs/Game_Demo/City_Sim/Part-1#Placing-Moving-Removing-Rotating-On-Contact)

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

# Placing, Moving, Removing, Rotating, On Contact

* For selecting and moving building we will use physics [Ray-casting](https://en.wikipedia.org/wiki/Ray_casting), we will ray cast from camera to mouse's position in world space and than get the `rigidbody` it hit and the location the ray intersected.

Now:

1. Create a cube `bld_hs`(stand for building_house), and set its physics as:
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

3. Create 2 new Haxe script `PlayerController` and `BuildingController` and assign it to scene.

<!-- tabs:start -->

#### **BuildingController.hx**

```haxe
package arm;

import armory.system.Event;
import armory.trait.physics.RigidBody;
import armory.trait.physics.PhysicsWorld;

import iron.Scene;
import iron.math.Vec4;
import iron.math.RayCaster;
import iron.system.Input;
import iron.object.Object;

// Define typedef Buildings(kind of like enum for other language)
typedef Building = {
	var name: String;
}

class BuildingsController extends iron.Trait {

	var physics = PhysicsWorld.active;

	var hitPointLoc = new Vec4();

    //Building's Id, eg: bld_hs1, bld_pw2, etc.
	public var buildingId = 0;
    //Declare selectedBuilding, i.e., name of building currently selected.
	public var selectedBuilding:String = "";
    //Selected Building Object
	public var selectedBuildingObj: Object;
    //Whether any building is selected or not
	public var isBuildingSelected = false;
    //Should building move
	public var buildingMove = false;
    //Is building in any contact
	public var buildingInContact = false;

	public function new() {
		super();
		notifyOnInit(function (){
            //Set selectedBuildingObj on init
			selectedBuildingObj = Scene.active.getChild(selectedBuilding);
		});
	}

	public function selectBuiliding() {
        //Get rigid body from raycast from group 2.
		var rigidbody = getRaycast(2).rigidbody;
        //Check if rigidbody isn't null and rigidbody's name start with "bld"
		if(rigidbody != null && StringTools.startsWith(rigidbody.object.name, "bld")){
            //Set selected building to hited rigidbody name
			selectedBuilding = rigidbody.object.name;
			isBuildingSelected = true;
		}else {
			selectedBuilding = null;
			isBuildingSelected = false;
		}
	}

	public function unselectBuilding() {
        //Nothing is selected and nothing should move
		selectedBuilding = null;
		isBuildingSelected = false;
		buildingMove = false;
	}

	public function moveBuilding() {
		var raycast = getRaycast(1);
		if(raycast.rigidbody != null && raycast.rigidbody.object.name == "Ground") {
            // Set location of house as floor of ray hit position's x, y and z as 0.4.
            // (We floor the hit.pos.x and hit.pos.y for grid snapping effect)
			hitPointLoc.set(Math.floor(raycast.hit.pos.x), Math.floor(raycast.hit.pos.y), 0.4);
            //Set loc of selected building
			selectedBuildingObj.transform.loc.setFrom(hitPointLoc);
		}
	}

	public function rotateBuilding() {
        //Get Eular of selected building
		var buildingEular = selectedBuildingObj.transform.rot.getEuler();
        //Add 90 deg to z-axis
		buildingEular.z += 90 * 3.14 / 180;
        //finally set Eular rotation of selected building
		selectedBuildingObj.transform.rot.fromEuler(buildingEular.x, buildingEular.y, buildingEular.z);
	}

	public function spawnBuilding(type: String) {
        //Spawn object with name = "bld_"+type
		Scene.active.spawnObject("bld_"+type, null, function(bld: Object){
            //Increment buildingID
			buildingId++;
            //set loc to center
			bld.transform.loc.set(0.0, 0.0, 0.0);
			bld.transform.buildMatrix();
            //Change name
			bld.name = "bld_"+type+buildingId;
		}, false);
	}

	public function removeBuilding() {
        //Remove Selected building
		selectedBuildingObj.remove();
	}

	public function buildingContact() {
        //Get contact of selected building
		var contact = physics.getContacts(selectedBuildingObj.getTrait(RigidBody));
		if (contact != null){
            //We don't want to loop through all contact, 
            //because only collideable object will be building
			buildingInContact = true;
			trace("In contact");
		}else{
			buildingInContact = false;
		}
	}

	function getRaycast(group:Int){
        var mouse = Input.getMouse();
		var start = new Vec4();
		var end = new Vec4();
		var camera = Scene.active.getCamera("Camera");
        // Get Ray-cast direction from start to end with mouse's x, y and camera
		RayCaster.getDirection(start, end, mouse.x, mouse.y, camera);
        // cast ray from camera's location in world space to end vec and get hit result.
		var hit = physics.rayCast(camera.transform.world.getLoc(), end, group);
		var rigidbody = (hit != null) ? hit.rb : null;
        //wrap rigidbody and hit result and return it.
		return{
			rigidbody: rigidbody,
			hit: hit
		};
	}
}

```

#### **PlayerController.hx**


```haxe
package arm;

import iron.system.Input;
//Import previously made BuildingController
import arm.BuildingsController;

class PlayerController extends iron.Trait {

	var mouse = Input.getMouse();
	var kb = Input.getKeyboard();

	var buildings = new BuildingsController();
	var buildingType: String = "hs";

	public function new() {
		super();
		notifyOnUpdate(update);
	}

	function update() {
        //If any building isn't selected then can select building or spawn one.
		if(!buildings.isBuildingSelected){
			if (mouse.started()){
				buildings.selectBuiliding();
			}
			if (kb.started("p")){
				buildings.spawnBuilding(buildingType);
			}
		}else{
            //If builidng is selected then get contact, unselect, move, remove, rotate building
			buildings.buildingContact();
			if (mouse.started("right")) {
				if (!buildings.buildingInContact){
					buildings.unselectBuilding();
				}
			}
			if (kb.started("m")){
				buildings.buildingMove = true;
			}else if (kb.started("f")){
				buildings.removeBuilding();
			}else if (kb.started("r")){
				buildings.rotateBuilding();
			}
		}
        //If buildingMove is true than move building
		if (buildings.buildingMove) {
			buildings.moveBuilding();
		}
		//If key 1 is pressed than set building type to house
        //Else if key 2 is pressed than set building type to park
        //...
		if (kb.started("1")) buildingType = "hs";
		else if (kb.started("2")) buildingType = "pk";
		else if (kb.started("3")) buildingType = "gd";
		else if (kb.started("4")) buildingType = "sc";
		else if (kb.started("5")) buildingType = "sm";
		else if (kb.started("6")) buildingType = "qa";
		else if (kb.started("7")) buildingType = "sw";
		else if (kb.started("8")) buildingType = "pw";
	}

}

```
<!-- tabs:end -->

Now try creating more building such as gardens, parks, sawmills, etc and apply same physics as `bld_hs`, and we are done!

Now, you should be ready to replace cube buildings, with your own assets!

!> W.I.P