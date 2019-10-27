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


![](/../../../docassets/CBS_3_1.png ':size=800')