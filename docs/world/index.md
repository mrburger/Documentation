World Generation in 1.13.2
==========================
The process of world and dimension generation has changed significantly since 1.12.2, especially in regards to world decoration (formerly "population"). These changes have improved the process, though, especially in regards to structures larger than a chunk.
Understanding the Process
---------------
In order to get started with generation in 1.13.2, one must first understand how Minecraft now generates terrain.
The high-level steps are, in order:
1. Generate the chunk's base by filling in only the terrain shape and assigning `Biomes` to each block/
2. Add caves and ravines, both ground and sea type
3. Decorate the world with all applicable `Feature` types
4. Calculate the light levels in the chunk
5. Spawn applicable mobs

A more detailed explanation is provided below, with method types

1. `public void makeBase(IChunk chunkIn)`
  Generates the basic terrain. This is accomplished in Vanilla by using the ouputs of a set of Noise Generators to determine if a `BlockPos` is going to be a terrain block (usually Stone) if it exceeds a certain threshold, and if the generator is _below_ sea level, the generator will replace that `BlockPos` with Water, provided a terrain block does not generate there. Then, the surface is overlayed (The grass, dirt, or sand blocks that make up the first few layers of the world below the ground) based on the previously assigned `Biome`, which has an associated `CompositeSurfaceBuilder<>()` that dictates how and which blocks will be overlayed. Bedrock is then generated, and the chunk has its status set to `ChunkStatus.BASE`, meaning it is ready for the next step.
2. `public void carve(WorldGenRegion region, GenerationStage.Carving carvingStage)`
  "Carves" The land and sea by adding caves and ravines of the respective type. This step develops the underground spaces Minecraft is known for.
3. `public void decorate(WorldGenRegion region)`
  Decorates the world with `Feature`s specific to the `Biome`. `Feature`s are not added here, rather they are attached to the `Biome`, discussed later.
4. Lighting is now handled off-thread automatically, and therefore is of little interest in the scope of this page.
5. `public void spawnMobs(WorldGenRegion region)`
  Spawn passive and hostile mobs besides Phantoms in the world.
6. Finalization of the chunk, which converts it from a `ChunkStatus.Type.PROTOCHUNK` to a `ChunkStatus.Type.LEVELCHUNK`, ready to be used by the server.
Creating a World Type
-----------------
The simplest form of custom generators is the custom `WorldType`. Both a `BiomeProvider` and a `ChunkGenerator` are needed to create a selectable world type. Each of these objects will be discussed in detail on their respective pages. To create this, a class extending `WorldType` is declared, with the following method:
* `public IChunkGenerator<?> createChunkGenerator(World world)`
  This creates both the `ChunkGenerator` and `BiomeProvider` objects, and passes them to the world generator using the given settings. **NOTE:** `ChunkGenSettings` objects _DO NOT_ persist across world loads.
Remember how we need a `ChunkGenerator` and `BiomeProvider`? Their functions are explained below.
TODO: WRITE HOW THEY WORK
