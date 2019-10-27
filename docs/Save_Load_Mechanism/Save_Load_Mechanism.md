# Save/Load Mechanism

Save/load is one of the most important feature in game, whether it is single-player or multi-player game. In case of single-player, the game data can be saved to local drive, with encryption so that cheating is not made easy. On other hand, in multi-players game, game data is saved to server hosted by the game development group.

Ever wondered how this is generally done? Well, Saving and loading mechanism can be done by simply writing data into file and reading from it. [Json](https://en.wikipedia.org/wiki/JSON) and [Xml](https://en.wikipedia.org/wiki/XML) file formats are popularly used for this purpose for small game, but you can write your own format *(structure, parser, encryption, etc)* if you are into writing your own stuffs. *(we are talking about small game that you share with friend, for fun and nothing serious, writing save/load mechanism for AAA games involves lot of other things)*.

We will be creating save/load game mechanism in `Armory3D`. For simplicity sake, we will be using `Json` to store in our game data and not use any sort of encrytion *(this kind of stuffs are left for you as 'DIY')*.

This tutorial is divided into 3 parts:
1. Setup the mechanism, we will only look into writing and reading file in-game.
2. Finally add saving and loading cube's Location and Rotation.
3. Close this tutorial by adding UI for saving and loading for more gamey experience.

At the end of this tutorial you should be at-least be able to save data into `json`, read from it and control it with UI. And you should get something like this:
<iframe width="640" height="480" src="https://blackgoku36.github.io/armory-tutorials/docassets/save_load_final.mp4" frameborder="0" allowfullscreen> </iframe>

If anything goes wrong, then you can check the [source code](https://github.com/BlackGoku36/armory-tutorial-download/tree/master/SaveLoadMechanism)
