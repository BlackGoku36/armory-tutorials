# PART-2

In `Part-2`, we will involve resources producing and collecting from buildings.

Rechecking our concept, when we place buildings, we want it to produce some resources at cost of some other resources, here the small table from the concept:

| Buildings    | Costs                      | Produce                |
| ------------ | -------------------------- | ---------------------- |
| House        | Woods, Stones, Electricity | Money, Happiness       |
| Parks        | Woods, Stones, Electricity | Money, Happiness       |
| Sawmills     | Money, Electricity         | Woods, Pollution       |
| Quarry       | Money, Electricity         | Stones, Pollution      |
| Powerplants  | Money                      | Electricity, Pollution |

---
Table of contents:
- [Building properties structure](docs/Game_Demo/City_Sim/part-2#building-properties-structure)

---

# RESOURCES

Buildings will produce given amount of resources in given interval of time. We want to make:
- House produce 5 money every 5 sec at cost of 10 woods and 10 stones.
- Sawmill produce 5 woods every 5 sec at cost of 10 money.
- Quarry produce 5 stones every 5 sec at cost of 10 money.
- Powerplant produce 10 electricity every 5 sec at cost of 20 money.
- Electricity to be used by house, sawmill, quarry to produce their resources, if they don't get enough electricity, they stop production.

## BUILDING PROPERTIES STRUCTURE

Let create proper structure of our building property, where we will store buildings properties such as maximum buildings allowed, cost, production, etc.

1. Create new scene Haxe trait `WorldController`, this will act as our main world controller.

<!-- tabs:start -->

#### **WorldController.hx**

```haxe
package arm;
//Building properties structure
typedef BuildingProp = {
    //at: how many building are there currently?, max: how many maximum buildings can be spawned?
	at:Int, max:Int,
    //Cost amount of money, wood, stones, electricity
	costM:Int, costW:Int, costS:Int, costE:Int,
    //Produce amount of money, woods, stones, electricity
	prodM: Int, prodW:Int, prodS:Int, prodE:Int,
    //Produce amount of happiness, pollution
	prodH:Int, prodP:Int
}

class WorldController extends iron.Trait {
    //Set houses prop
	public static var houseProp: BuildingProp = { 
		at:0, max:2, costM: 0,costW:10, costS:10, costE:5, prodM: 5, prodW:0, prodS:0, prodE: 0, prodH:3, prodP:0
	};
    //Set parks prop
	public static var parkProp: BuildingProp = {
		at:0, max:2, costM: 0,costW:10, costS:10, costE:5, prodM: 5, prodW:0, prodS:0, prodE: 0, prodH:5, prodP:0
	};
    //Set sawmills prop
	public static var sawmillProp: BuildingProp = {
		at:0, max:2, costM: 10,costW:0, costS:0, costE:5, prodM: 0, prodW:5, prodS:0, prodE: 0, prodH:0, prodP:3
	};
    //Set quarrys prop
	public static var quarryProp: BuildingProp = {
		at:0, max:2, costM: 10, costW:0, costS:0, costE:5, prodM: 0, prodW:0, prodS:5, prodE: 0, prodH:0, prodP:3
	};
    //Set powerplants prop
	public static var powerplantProp: BuildingProp = {
		at:0, max:2, costM: 20, costW:0, costS:0, costE:0, prodM: 0, prodW:0, prodS:0, prodE: 10, prodH:0, prodP:5
	};

	public function new() {
		super();
	}
}

```

---

<details>
	<summary>Code Explanation</summary>

1. We create buildings properties structure to store information and fields which are not needed are set to '0' as we don't want to create structure for each building.

2. We set properties of buildings `houseProp`, `parkProp`, etc.

</details>

---

<!-- tabs:end -->


## RECALCULATE BUILDINGS

We will recalculate buildings from buildings structure and set building properties structure and limit amount of particular buildings that can be spawned.

<!-- tabs:start -->

#### **BuildingController.hx**

```haxe
~
import arm.WorldController;

typedef Building = {~}
class BuildingController extends iron.Trait{
	~
	public static var enoughBuildings = true;
	public function new(){~}
	~
	public static function spawnBuilding(type: Int){
		var world = WorldController;
		//Check if this type of buildings reached max amount
		checkMaxBuilding(type);
		//If there is not enough building
		if(!enoughBuildings){
			Scene.active.spawnObject("bld_"+type, null, function(bld: Object){
				~
				buildings.push({~});
				//Recalculate amount of buildings
				recalculateBuildings();
				unselectBuilding();
				selectBuilding(bld.name);
			});
		}

	}
	public static function removeBuilding(){
		Scene.active.getChild(selectedBuilding).remove();
		removefromArray(selectedBuilding, buildings);
		//Recalculate amount of buildings
		recalculateBuildings();
		unselectBuilding();
	}
	~
	static function recalculateBuildings(){
		var world = WorldController;
		//Create buildings type list
		//[House, parks,......, powerplant]
		var buildingList = [0, 0, 0, 0, 0, 0, 0, 0];
		for(building in buildings){
			switch (building.type){
				//Set increase building list by one of certain type
				case 1: buildingList[0] += 1;//House
				case 2: buildingList[1] += 1;//Park
				case 5: buildingList[4] += 1;//Sawmill
				case 6: buildingList[5] += 1;//Quarry
				case 8: buildingList[7] += 1;//Powerplant
			}
		}
		//Set 'at' of building property
		world.houseProp.at = buildingList[0];
		world.parkProp.at = buildingList[1];
		world.sawmillProp.at = buildingList[4];
		world.quarryProp.at = buildingList[5];
		world.powerplantProp.at = buildingList[7];
	}
	static function checkMaxBuilding(type:Int){
		var world = WorldController;
		switch(type){
			//Check if building of type reached max amount, then set enough buildings to true else false
			case 1: world.houseProp.at == world.houseProp.max ? enoughBuildings = true : enoughBuildings = false;
			case 5: world.sawmillProp.at == world.sawmillProp.max ? enoughBuildings = true : enoughBuildings = false;
			case 6: world.quarryProp.at == world.quarryProp.max ? enoughBuildings = true : enoughBuildings = false;
			case 8: world.powerplantProp.at == world.powerplantProp.max ? enoughBuildings = true : enoughBuildings = false;
		}
	}
}
```

---

<details>
	<summary>Code Explanation</summary>

1. In `recalculateBuildings()`, we create array of building list, each index represent building type. Then we loop through our array of buildings and switch through building types and increase the building list by one if type matches. And finally set 'at' of our buildings property from building list.

2. In `checkMaxBuilding(*type*)`, we switch through types, if type match we check building of type's property and check if no. of buildings is same as maximum limit, if true than set enoughBuildings to true, else false.

3. In `spawnBuilding(*type*)`, check if maximum amount of buildings of particular type reached, if not true than allow to spawn, and then on spawning recalculate buildings again.

4. In `removeBuilding()`, recalculate buildings again after selected building is removed.

</details>

---

<!-- tabs:end -->

## RECALCULATE RESOURCES

We will recalculate resources and limit buildings from being spawned if there aren't enough resources.

<!-- tabs:start -->
#### **WorldController.hx**
```haxe
~
typedef BuildingProp = {~}

class WorldController extends iron.Trait {
	//Set resources with array -> [at, max]
	public static var happiness:Array<Int> = [50, 100];
	public static var money:Array<Int> = [50, 100];
	public static var woods:Array<Int> = [50, 100];
	public static var stones:Array<Int> = [50, 100];
	public static var electricity:Array<Int> = [0, 100];

	public static var houseProp: BuildingProp = {~};
	public static var parkProp: BuildingProp = {~};
	public static var sawmillProp: BuildingProp = {~};
	public static var quarryProp: BuildingProp = {~};
	public static var powerplantProp: BuildingProp = {~};

	public function new(){~}

}
```

---

<details>
	<summary>Code Explanation</summary>

1. We set resources with array as [amount of this resource, maximum resource storeable].

</details>

---

#### **BuildingController.hx**
dfsf
```haxe
~

typedef Building = {~}
class BuildingController extends iron.Trait{
	~
	public static var enoughBuildings = true;
	public static var enoughResources = true;
	public function new(){~}
	~
	public static function spawnBuilding(type: Int){
		var world = WorldController;
		checkMaxBuilding(type);
		//Check if resources reached max amount
		checkResources(type);
		//If there is not enough building and there is enough resource
		if(!enoughBuildings && enoughResources){
			Scene.active.spawnObject("bld_"+type, null, function(bld: Object){
				~
				buildings.push({~});
				recalculateBuildings();
				//Recalculate amount of resources of this type
				recalculateResources(type);
				unselectBuilding();
				selectBuilding(bld.name);
			});
		}

	}
	public static function removeBuilding(){~}
	~
	static function recalculateBuildings(){~}
	static function checkMaxBuilding(type:Int){~}
}
```
<!-- tabs:end -->

---

# CANVAS

!> W.I.P