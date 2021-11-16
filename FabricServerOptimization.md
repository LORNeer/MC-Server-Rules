# Fabric Server Optimization
Small collection of things that we might or might not use to optimize the fabric server.

#Mods
|Mod|Release for 1.17|Current Version|Description|
|---|---|---|---|
|[Lithium](https://www.curseforge.com/minecraft/mc-mods/lithium/files/all)|08.06.2021 (beta), 16.06.2021 (stable)|G: 1.17.1, M: 0.7.5|A mod designed to drastically improve the general performance of Minecraft without breaking things.|
|[Starlight](https://github.com/PaperMC/Starlight/releases)|03.07.2021|G: 1.17.x, M: 1.0.0-RC3|A mod that rewrites the light engine to fix lighting performance and lighting errors.|
|[DimensionalThreading](https://github.com/WearBlackAllDay/DimensionalThreading/releases)|09.06.2021 (1.2.4)|G: 1.17, M: 1.2.5|A mod which optimizes the processing of multiple dimensions by assigning them independent threads.|
|[Hydrogen](https://github.com/CaffeineMC/hydrogen-fabric/releases)|19.06.2021 (0.3)|G: 1.17.1, M: 0.3.1|A mod designed to reduce the game's memory requirements by implementing more memory-efficient data structures and logic.|
|[FerriteCore](https://www.curseforge.com/minecraft/mc-mods/ferritecore-fabric)|21.06.2021 (beta), 22.08.2021 (stable)|G: 1.17.1, M: 3.0.3 (stable) / 3.1.0 (beta)|A mod that reduces the memory usage of Minecraft in a few different ways.|
|[RecipeCache](https://www.curseforge.com/minecraft/mc-mods/recipe-cache)|11.06.2021 (0.2.0)|G: 1.17.1, M: 0.2.1|A mod that caches recipe lookup for crafting and furnaces to lessen server lag caused by crafting stacks of items and large amounts of furnaces ticking.|
|[Krypton](https://github.com/astei/krypton/releases)|08.06.2021 (0.1.3)|G: 1.17.1, M: 0.1.4|A mod that optimizes the Minecraft networking stack and entity tracker.|
|[LazyDFU](https://modrinth.com/mod/lazydfu)|19.02.2021 (0.1.2)|G: 1.17.1, M: 0.1.2|A mod that makes Minecraft DataFixerUpper initializatiion lazy. (Faster server start-up time)|
|[Carpet Mod](https://www.curseforge.com/minecraft/mc-mods/carpet/files/all)|29.06.2021 (1.4.41)|G: 1.18, M: 1.4.53|A mod for vanilla Minecraft that allows you to take full control of what matters from a technical perspective of the game.|
|[Carpet Extra](https://www.curseforge.com/minecraft/mc-mods/carpet-extra/files/all)|17.02.2021 (1.4.30)|G: 1.18, M: 1.4.51|A mod that adds extra features to the Carpet Mod.|
|[Carpet TIS](https://github.com/TISUnion/Carpet-TIS-Addition/releases)|28.03.2021 (1.20)|G: 1.18, M: 1.25.0|A Carpet mod extension, a collection of carpet mod style useful tools and interesting features.|
|[Chunky Pregenerator](https://www.curseforge.com/minecraft/mc-mods/chunky-pregenerator)|08.06.2021 (1.2.86)|G: 1.17.1, M: 1.2.124|A Fabric mod while pre-generates chunks, quickly and efficiently.|
|[Spark](https://ci.lucko.me/job/spark/)|   |   |A performance profiling server-side mod.|
|[C2ME (EXPERIMENTAL)](https://github.com/ishlandbukkit/C2ME-fabric/releases)|20.06.2021 (0.2.0+alpha.1)|G:1.18, M: 0.2.0+alpha.5)|A Fabric mod designed to improve the performance of chunk generation, I/O, and loading. This is done by taking advantage of multiple CPU cores in parallel. (This mod is experimental and you should only use it at your own risk)|

#Configurations
## [server.properties](https://minecraft.fandom.com/wiki/Server.properties)
|Config|Value|Description|
|---|---|---|
|network-compression-threshold|512|By default it allows packets that are n-1 bytes big to go normally, but a packet of n bytes or more gets compressed down. A lower number means more compression but compressing small amounts of bytes might actually end up with a larger result than what went in.|
|view-distance|6 (or less)|Sets the amount of world data the server sends the client, measured in chunks in each direction of the player.|
|max-tick-time|-1|The maximum number of milliseconds a single tick may take before the server watchdog stops the server. -1 disables this watchdog alltogether.|
|snooper-enabled|false|Sets whether the server sends snoop data regularly to http://snoop.minecraft.net.|

## [world/carpet.conf](https://gist.github.com/skyrising/cea2495437afea0cc3af2bb11d6a1856)
This is for the Carpet Mod, if it is used.

|Config|Vakue|Description|
|---|---|---|
|combineXPOrbs|true|XP orbs combine with other into bigger orbs|
|commandLog|?|Enables /log command to monitor events in the game via chat and overlays|
|commandPerimeterInfo|?|Enables /perimeterinfo command that scans the area around the block for potential spawnable spots|
|commandDistance|?|Enables /distance command to measure in game distance between points. Also enables brown carpet placement action if 'carpets' rule is turned on as well|
|tileTickLimit|1024|Customizable tile tick limit|
|fastMovingEntityOptimization|true|Optimized movement calculation or very fast moving entities|
|updateSuppressionCrashFix|true|Fixes updates suppression causing server crashes|
|reloadSuffocationFix|true|Won't let mobs glitch into blocks when reloaded. Can cause slight differences in mobs behaviour|
|blockCollisionsOptimization|?|Optimized entity-block collision calculations. By masa|
|maxEntityCollisions|Customizable maximal entity collision limits, 0 for no limits|1|
|optimizedTNT|true|TNT causes less lag when exploding in the same spot and in liquids|

#Java Optimization
## Java version & JRE
Preferably we use [GraalVM](https://www.oracle.com/downloads/graalvm-downloads.html?selected_tab=1) as our Java
runtime environment (with the latest java version available). It's the fastest JRE available right now.  
Another good alternative is
[Adoptium](https://adoptium.net/installation.html?variant=openjdk16&jvmVariant=hotspot#installers)
(formerly AdoptOpenJDK).

## Java start-up flags
### Stable flags
-Xms8G -Xmx12G -XX:+IgnoreUnrecognizedVMOptions -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions
-XX:-OmitStackTraceInFastThrow -XX:+ShowCodeDetailsInExceptionMessages -XX:+DisableExplicitGC -XX:-UseParallelGC
-XX:-UseParallelOldGC -XX:+PerfDisableSharedMem -XX:+UseZGC -XX:-ZUncommit -XX:ZUncommitDelay=300
-XX:ZCollectionInterval=5 -XX:ZAllocationSpikeTolerance=2.0 -XX:+AlwaysPreTouch -XX:+UseTransparentHugePages
-XX:LargePageSizeInBytes=2M -XX:+UseLargePages -XX:+ParallelRefProcEnabled

### Experimental flags
-Xms8G -Xmx12G -XX:+IgnoreUnrecognizedVMOptions -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions
-XX:-OmitStackTraceInFastThrow -XX:+ShowCodeDetailsInExceptionMessages
-XX:+DisableExplicitGC -XX:-UseParallelGC -XX:-UseParallelOldGC -XX:+PerfDisableSharedMem
-XX:+UseZGC -XX:-ZUncommit -XX:ZUncommitDelay=300 -XX:ZCollectionInterval=5 -XX:ZAllocationSpikeTolerance=2.0
-XX:+ExitOnOutOfMemoryError -XX:+AlwaysPreTouch -XX:-DontCompileHugeMethods -XX:+TrustFinalNonStaticFields
-XX:+UseFastUnorderedTimeStamps -XX:+UseTransparentHugePages -XX:LargePageSizeInBytes=2M -XX:+UseLargePages
-XX:+UseCMoveUnconditionally -XX:+UseNewLongLShift -XX:+UseVectorCmov -XX:+UseXmmI2D -XX:+UseXmmI2F
-XX:+ParallelRefProcEnabled

### Explaining some flags
|Flag|Explanation|
|---|---|
|-Xms matching -Xmx|You should never run your server with the case that -Xmx can run the system completely out of memory. Your server should always be expected to use the entire -Xmx|
|+UnlockExperimentalVMOptions|Unlocks experimental flags/options|
|+DisableExplicitGC|Disables System.gc() calls from code, you really don’t want people playing around with your GC|
|-UseParallelGC|Disables Parallel GC, this should already be disabled, but we set this just to be sure|
|-UseParallelOldGC|^ but disables ParalledOld GC|
|-UseG1GC|^ but disables G1GC|
|+UseZGC|Enables ZGC|
|+AlwaysPreTouch|AlwaysPreTouch gets the memory setup and reserved at process start ensuring it is contiguous, improving the efficiency of it more. This improves the operating systems memory access speed. Mandatory to use Transparent Huge Pages|
|+ParallelRefProcEnabled|Optimizes the GC process to use multiple threads for weak reference checking|
|+PerfDisableSharedMem|Causes GC to write to file system which can cause major latency if disk IO is high.|