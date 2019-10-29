# Setup

Setting up Armory in Blender.

- [Setup](#setup)
  - [Downloading the add-on](#downloading-the-add-on)
    - [Git](#git)
    - [Itch.io](#itchio)
  - [Installing the add-on](#installing-the-add-on)
  - [Updating the add-on](#updating-the-add-on)

---

## Downloading the add-on

### Git

Installing Armory using Git is easy task.

1. Get [Blender](https://www.blender.org/download/) if you don't already have.
2. Open Terminal/Command Prompt, locate directory where the Blender is.
3. Enter following command:
```bash
git clone --recursive https://github.com/armory3d/armsdk
cd armsdk
git submodule foreach --recursive git pull origin master
```
4. Follow `Installing the add-on` steps.

### Itch.io

1. Go to [itch.io](https://armory.itch.io/armory3d), and then download `ArmorySDK-20XX-XX.zip`
2. Unpack it and place it where ever you want.
3. Follow `Installing the add-on` steps.

---

## Installing the add-on

![prefs](../../docassets/setup_1.png ':size=800')

1. Open Blender, go to `Edit - Preference`, `Blender Preference` should open, click `install`, locate the armsdk and select `armory.py` and click `install Add-on from File..` and save preference(Three dash icon from bottom left corner)
2. Restart Blender, and head back to preference and go to `Render: Armory` add-on, and to `Preferences - SDK Path` and enter the your path to armsdk, and then finally save it again.
3. Restart the Blender again and Armory's new UIs should be there.

---

## Updating the add-on

1. Go to Armory add-on tab and then click `Update SDK`.
2. Open terminal/command prompt and check the progress, when it is done, it should say `Armory is sucessfully updated. Restart Blender`.
3. Restart Blender
