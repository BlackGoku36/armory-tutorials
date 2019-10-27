# User Interface (UI)

<!-- TODO: More words -->
We will create better UI to remove need of pressing key.

This will be split in 2 parts:
- Main Menu UI: This will have placing of building from menu, settings, layout edit.
- 3D Menu UI: This will have moving, removing, rotating, info of buildings.

## Main Menu UI

We will be creating main menu UI, we should be able to select building that we desire to place and also able to access other things such as settings, layout edit, etc.

Let's get started!

1. Open `MainCanvas` in `Armory2D` that was created in previous tutorial and add some elements:

    - `Empty`:
        - `menu_empty`: We will use this as parent, so that we can make child element visible and invisible easily.
    - `Text`:
        - `menu_text`: We will use this as label for our menu button.
    - `Button`:
        - `settings_btn`: Toggle settings
        - `edit_btn`: Go in edit layout mode
        - `house_btn`: Toggle house building menu
        - `factory_btn`: Toggle factory building menu
        - `community_btn`: Toggle community building menu
    - `Rect`:
        - `menu_rect`: Just for style
    - `Fill Rect`:
        - `bluerect`: act as background for menu buttons

2. Now parent following elements to `menu_empty`:
    - `bluerect`
    - `settings_btn`
    - `edit_btn`
    - `house_btn`
    - `factory_btn`
    - `community_btn`

3. Now add events to buttons:
    - `settings_btn`: `settings_btn`
    - `edit_btn`: `edit_btn`
    - `house_btn`: `house_btn`
    - `factory_btn`: `factory_btn`
    - `community_btn`: `community_btn`


![](/../../../docassets/CBS_3_1.png ':size=800')

Now to add Main menu hiding and showing:

<!-- tabs:start -->
#### **MainMenuController.hx**
```haxe
package arm;

import armory.trait.internal.CanvasScript;
import armory.system.Event;

class MainCanvasController extends iron.Trait {

    static var maincanvas:CanvasScript;
    //State of menu, 0 -> closed, 1 -> opened
    var menuState = 0;

    public function new() {
        super();

        notifyOnInit(init);
        notifyOnUpdate(updateCanvas);
    }

    function init() {
        maincanvas = new CanvasScript("MainCanvas", "Big_shoulders_text.ttf");

        maincanvas.setCanvasVisibility(true);
        //Set menu_empty to invisible
        maincanvas.getElement("menu_empty").visible = false;
        //On 'menu_btn' event
        Event.add("menu_btn", function(){
            //If closed
            if (menuState == 0){
                // Set menu_empty to visible
                maincanvas.getElement("menu_empty").visible = true;
                // menu is open
                menuState = 1;
            //If opened
            }else if (menuState == 1){
                //Set menu_empty to invisible
                maincanvas.getElement("menu_empty").visible = false;
                // menu is close
                menuState = 0;
            }
        });
    }

    function updateCanvas() { ~ }
    function updatePB(){ ~ }
    function updateAmount(){ ~ }
}
```

---

<details>
    <summary>Code Explanation</summary>

1. We first set menuState to closed when declared
2. Every time we receive `menu_btn` event(i.e., `menu_btn` is pressed). We check if menu is closed, if it is then we show `menu_empty`(i.e, show all menu buttons) and set `menuState` to opened. Else if `menu_btn` is open than we do opposite.
</details>

---

<!-- tabs:end -->

---
