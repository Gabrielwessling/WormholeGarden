https://gabrielwessling.itch.io/the-wormhole/devlog/972397/build-module

So, first devlog, I'm pretty optimistic about this project, I am doing it in a way that will postpone my burnouts, but even then I think I can handle what is coming. The first thing I wanted to make was the building system so I can make a basic demo scene (where you can play whatever is already created) with some kind of tutorial for what there is and then a sandbox for just playing with the game. After this demo is ready it will be freely available here and will be updated every time a big update happens with the project.

I've finished the basic building system. It will be used by both players and the development team for level creation, so it covers both base building and level design.

Main features:

- Support for straight and diagonal walls. Both require at least half a floor to be under it.

- Half a floor works like a triangle. It can also be used to form a complete floor with two different materials. This can help create less square edges in structures, this and the diag walls of course.

- Each material has its own texture for each type of material (like metal, wood and stone). The final color of the texture depends on the final item (e.g.: cast iron, acacia, slate), not on the generic type. I love this so much, materials are not only for gameplay reasons but also look cute and have variations.

- Cutout system: Inside buildings, walls are shown at 25% height so as not to obstruct the view. I tried circle of transparency like this, but it was ugly, I still want a variation where only walls that are in front of the character (in the middle of it and the camera) to be shown like that.

- There is no inventory yet, but the costs are already defined and balanced based on the occupied area. Diagonal walls have an adjusted cost so as not to follow the exact proportion of the area (avoiding the 1.4x increase in relation to straight walls) since I think this would be worse than it would be realistic.

I think what I'm going to do now is a little cleanup on the codes already there (I need to separate some functions of my 900 lines "build mode" general script), but I'm not going to invest much time in it, I already am doing everything in a more modular way than I did before, so in terms of hardcoded shit there's not much yet. After that I want an inventory system with some kind of component style mechanic, like, imagine you have a sword and an apple, both will be items, and they wont be their own classes, instead they will have different components, like "Equippable, Melee, Slashing, Recyclable" for the sword and "Consumable, Healing(50), Nutrition(35), Biodegradable, etc" for the apple.



Thank you for reading, and please, any feedback is a good feedback, even if not completely right. 