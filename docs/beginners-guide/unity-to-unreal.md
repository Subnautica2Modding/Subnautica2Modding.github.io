# Switching from Unity to Unreal Engine

If you're coming from modding Subnautica or Subnautica: Below Zero, you'll quickly notice things are not quite the same as modding the previous games.

## Core Differences

As you likely already know, Subnautica 2 uses Unreal Engine, not Unity. This comes with plenty of minor quirks.

### Game Organization

Unreal Engine primarily uses Blueprints and C++ for logic. C++ is not a managed language like C# is, so a full decompilation of the game is not realistically attainable. However, Blueprint bytecode can be easily viewed with tools such as FModel, and can feel similar to reading IL logic.

If you keep hearing [Blueprints](https://dev.epicgames.com/documentation/unreal-engine/blueprints-visual-scripting-in-unreal-engine) but have no clear mental concept of what they are, that is understandable. They are a huge deal in Unreal Engine. You can mess around with Unreal Engine yourself to get a better understanding.

In Subnautica 2, game systems and content are often distributed across C++ code, Blueprints, and various Data Assets, rather than primarily living in managed assemblies and prefabs. Data is far more separated from logic here than in previous games, with many C++ and Blueprint systems relying on Data Assets to define specific game content.

### Coordinates

This one is more of a minor annoyance, but it took me a moment to realize exactly what was going on. The default unit is centimeters now, not meters. Don't be too concerned if teleporting yourself 5 units appears to do nothing at all. Furthermore, in Unreal Engine, the Z coordinate is up, NOT the Y coordinate, meaning the sea level plane is now at Z = 0.

### Asset Streaming

Unreal Engine often uses soft references instead of direct links to assets in memory. Similarly to asynchronous asset loading in SN1, you may find that some assets referenced by other objects will need to be loaded before they can be used.

Subnautica 2 uses built-in Unreal Engine systems for streaming the world (maybe the World Partition system?). This is a major shift from previous games, where most entities were streamed using a combination of their own custom systems.

Also, keep in mind that Subnautica 2 uses a combination of heightmaps and blended meshes for world geometry with no voxel terrain to be found.

## Analogies between Unity and Unreal Engine

These comparisons are not 1:1, but hopefully they can help you quickly understand some general concepts.

| Unity             | Unreal Engine   | Description                                                                                                                                  |
|:------------------|:----------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| GameObject        | Actor           | An object in the world that can contain components.                                                                                          |
| Component         | Actor Component | Reusable modules attached to actors with behaviour and/or data.                                                                              |
| Transform         | Scene Component | An Actor component with a physical position. This is commonly the "root" component of an Actor.                                              |
| Prefab            | Blueprint       | A reusable template for spawning pre-defined actors. Blueprints are generally more versatile than prefabs and often contain their own logic. |
| Scriptable Object | Data Asset      | An asset that stores data separately of logic.                                                                                               |
| Particle System   | Niagara System  | Niagara is a node-based visual effects system.                                                                                               |
| Scene             | Level           | A portion of the world containing many Actors. Levels can be combined to form larger worlds.                                                 |