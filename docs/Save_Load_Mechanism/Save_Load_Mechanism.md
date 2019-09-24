# Save/Load Mechanism

Saving and loading mechanism can be done by simply writing data into file and reading from it.

[Json](https://en.wikipedia.org/wiki/JSON) and [Xml](https://en.wikipedia.org/wiki/XML) file formats are popularly used for this purpose. To prevent cheating by just re-writing the file you can encrypt it, if you are making small games and don't want your friend to cheat, just use `serialization`.

We will be creating save/load game mechanism in `Armory3D`. At the end of this tutorial you should be at-least be able to save data into `json` and parse from it with UI.
We will be using `json` here and for simplicity sake, we will not do any sort of encryption.

This tutorial is divided into 3 parts:
1. Setup the mechanism, we will only look into writing and reading file in-game.
2. Finally add saving and loading cube's Location and Rotation.
3. Close this tutorial by adding UI for saving and loading for more gamey experience.

at the end of this tutorial you should get this:
<iframe width="640" height="480" src="https://blackgoku36.github.io/armory-tutorials/docassets/save_load_final.mp4" frameborder="0" allowfullscreen> </iframe>

If anything goes wrong, then you can check the [source code](https://github.com/BlackGoku36/armory-tutorial-download/tree/master/SaveLoadMechanism)