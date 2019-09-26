# Part I

In this part, we will implement the basics, such as moving, placing, rotating, removing, of buildings and arcball camera.

---

Table of contents:
* [Arcball Camera](docs/Game_Demo/City_Sim/Part-1#Arcball-camera)
* [Placing Moving Rotating Removing](docs/Game_Demo/City_Sim/Part-1#Placing-Moving-Rotating-Removing)

---

## Arcball camera

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

## Placing Moving Rotating Removing

For Placing Moving Rotating Removing of buildings, we will use physics ray-casting.

First we will ray-cast from mouse position on screen to ground and then transform it to world coords, then we will get hit result.

In `Physics` tab:
1. Set plane (ground) as `Rigid Body`, type as `Passive` and shape as `Box`.
2. Set cube (buildings) as `Rigid Body`, type as `Passive`, shape as `Box`, check `Animated` and in `Armory Collision Filter Mask`, set `Collision filter mask` to 2nd

?> Collision filter mask make raycast ignore the object.


1. Create new cube object `Cursor`, we will move it with mouse across the ground with ray-casting, i.e., we will use it as 3D cursor, and it will check if it is over-lapping with any building and then select it, set it's collision filter mask to 2nd too.
2. Create new Haxe script `PlayerController` and assign it to our scene.

```haxe
//In PlayerController.hx

import armory.trait.physics.PhysicsWorld;
import iron.math.RayCaster;
import iron.math.Vec4;
import iron.system.Input;
import iron.Scene;
import iron.object.Object;

class PlayerController extends iron.Trait {

	var cursor: Object;
	var mouse = Input.getMouse();
    //Get active Physics World
	var physics = PhysicsWorld.active;
    //Initiate new Vec4 for Ray-cast hit-point location
	var hitPointLoc: Vec4 = new Vec4();

	public function new() {
		super();

		notifyOnInit(function() {
			cursor = Scene.active.getChild("Cursor");
		});

		notifyOnUpdate(update);
	}

	function update() {
        //Update cursor's movement every frame
		cursorMovement();
	}

	function cursorMovement() {
		if(!mouse.down("right")){
			var start = new Vec4();
			var end = new Vec4();
			var camera = Scene.active.getCamera("Camera");
            // Get Ray-cast direction from start to end with mouse's x/y and camera.
			RayCaster.getDirection(start, end, mouse.x, mouse.y, camera);
            // cast ray from camera's loc in world space to end vec.
			var hit = physics.rayCast(camera.transform.world.getLoc(), end);
            // Check if hit isn't null, and get rigidbody of where the ray is hited.
			var rigidbody = (hit != null) ? hit.rb : null;
            // Check if rigidbody isn't null and rigidbody's name is `Ground`
			if(rigidbody != null){
				if(rigidbody.object.name == "Ground"){
                    //Make cursor visible.
					cursor.visible = true;
                    // Floor the hit position and set it as hitPointLoc.
                    // (Z-axis will remain 1.0 as we don't want to build buildings lower or higher the ground)
					hitPointLoc.set(Math.ffloor(hit.pos.x), Math.ffloor(hit.pos.y), 1.0);
                    // Set location of cursor from hitPointLoc
					cursor.transform.loc.setFrom(hitPointLoc);
					cursor.transform.buildMatrix();
				}
			}else{
                //If null, make cursor invisible
				cursor.visible = false;
			}
		}
	}
}

```

?> We floor the hit.pos.x and hit.pos.y for grid snapping effect

We get:

![](/../../../docassets/CBS_part1_2.gif ':size=400')

!> W.I.P