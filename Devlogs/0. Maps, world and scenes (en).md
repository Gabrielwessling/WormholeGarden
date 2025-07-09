https://gabrielwessling.itch.io/the-wormhole/devlog/981684/maps-world-and-scenes

Just started building the structural base of the game, like how we have a "world" class that defines basically the save game and each map existent in it, the world asset that saves base world classes to be played, maps that serve as metadata for the scenes and also hold the grids, entities and buildings, and now I'm doing the dirty editor making work...  

_![](https://img.itch.zone/aW1nLzIyMDM0Nzk4LnBuZw==/original/ID6iuP.png)_  

I need to load the map then and show the editor tools, I want a way to build, a way to make entities and their routines and put them in their initial places, make events like it is in rpg maker, etc.

I am mainly doing it to make it easier for me to just play around and have fun making the maps, but someday I may have some comrade to share the editor tools with.

Also, in the cover for this post I will have the drawing I did to decide the slot system, it will not be that each tile has slots, but that the Vector2 indicates what type of "slot" it would go on. So if you say 2.0,2.0 you are talking about the center of the 2,2 tile in the grid, but 2.5,2.0 is talking about the right border of that tile, and 2.5,2.5 is talking about the lower right intersection. That was decided after the drawing was made, but it's how it's working now.