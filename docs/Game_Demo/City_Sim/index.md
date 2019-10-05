# INTRODUCTION
!> W.I.P

**Quick introduction on Armory3D**: `Armory3D` is game engine integrated in `Blender` to provide excellent workflow from start to finish, it uses ultimate cross-platform tool-kit and framework that is `Haxe` and `Kha`.

**Quick introduction on City Building Simulator game genre**: `CBS` game is `strategic-management` type game where you act as `Mayor` of the city you are building. One of the main concept of `CBS` game is simple, you build houses and get money from it, get resources such as wood, stones, etc from buildings such sawmill, quarry, etc. your main goal is to increase your city level *(keep in mind, that we are talking about small CBS game her, not the one made by AAA studios)*.

So, let make a small `CBS` game demo in `Armory3D`, but with little twist in concept.

## CONCEPT

* **Art type**: Voxels *(those blocky things)*

* **Goal**: Your goal is to keep your citizens happy, go below the line of happiness, and you lose.

* **How this game work**:
    * You start playing the game with happiness at half.
    * Building such as Housing, Amusement park, small gardens, sports court increases happiness.
    * Building such as Sawmills, quarry, steelworks, powerplants increases pollution bar.
    * Pollution degrades parks, gardens, etc, and this make people unhappy.
    * You have to pay to maintain this parks, gardens, etc.

* **Building stuffs**:

| Buildings    | Costs                             | Produce                |
| ------------ | :-------------------------------: | ---------------------: |
| House        | Woods, Stones, Steel, Electricity | Happiness              |
| Parks        | Woods, Stones, Steel, Electricity | Happiness              |
| Garden       | Woods, Stones, Steel              | Happiness              |
| Sports court | Woods, Stones, Steel              | Happiness              |
| Sawmills     | Money, Electricity                | Woods, Pollution       |
| Quarry       | Money, Electricity                | Stones, Pollution      |
| Steelworks   | Money, Electricity                | Steel, Pollution       |
| Powerplants  | Money                             | Electricity, Pollution |

* **Features**:

| In-game Feature                                  | Tutorial's feature                             |
| ------------------------------------------------ | ---------------------------------------------- |
| Game can be saved/loaded locally                 | Extend upon [save/load tutorial](docs/Save_Load_Mechanism/Save_Load_Mechanism.md) and add little encryption feature like, [Caesar chiper](https://en.wikipedia.org/wiki/Caesar_cipher)|
| Spawning, removing, selecting moving of building | Will uses physics and raycasting stuffs        |
| Layout Editing                                   | Handling of different scenes                   |
| Settings                                         | Handling of screen size, graphics, etc. with UI|

A quick overview of this tutorial:
1. `Basic`: we will implement basics of our city building game such as placing, moving, etc with player interaction.

Now before we go, make sure to start up armory and run it, to make sure everything is working correctly.

!> Keep in mind that this tutorial is not fully done, this tutorial will change from time to time.