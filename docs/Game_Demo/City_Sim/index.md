# Introduction
!> W.I.P

This tutorial is on creating 3d [city building sim](https://en.wikipedia.org/wiki/City-building_game) game demo in Armory3D game engine.
We will try to reach features of engine as much as we can and utilize it.

## Concept

* **Art type**: [Voxels](https://duckduckgo.com/?q=voxel+art&t=ffab&atb=v164-1&iax=images&ia=images)

* **Goal**: Your goal is to keep your citizens happy, go below the line of happiness, and you loses.

* **How it work?**:
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

## Tutorial
This tutorial will be divided into many parts:
1. In this part we, will implement the basics, such as buildings moving, placing, rotating, removing, arcball camera.

Now before we go, make sure to start up armory and run it, to make sure everything is working correctly.
!> W.I.P