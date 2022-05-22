---
title: Huge fungi
description: Mechanics of huge fungi generation and farms
---

# Huge fungi

Report conducted by ncolyer, with information gathered through code reading, multiple experiments, consultations, discussions, trial and error, and more

## Overview
This page will split into the 3 main ways to harvest huge fungi (more commonly referred to as 'Nether Trees', or 'Wart Trees')
- Manual Harvesting
- Semi-Automatic
- Self-Sustaining

Some theorised methods will also be discussed, such as RNG manipulation, and self-sustaining N-Core designs.

## Glossary:
- Rates:
 The farm's collectable item output, normally measured in items per hour
- Efficiency:
 The output compared to the maximum possible output, in this case, the number of blocks of a huge fungi that are harvested, compared to the number that can possibly generate.
- gt:
 Game ticks, 1/20th of a second
- RNG: 
Random Number Generator, can be manipulated using the seed.
- s: 
Used in all cases instead of alternative 'z' spelling.

## Growth Mechanics
1. Natural Generation
Huge fungi generate naturally in only 2 biomes situated in the Nether (DIM-1); the warped forest, where you can find warped huge fungi, and the crimson forest, where you can find crimson huge fungi. Huge fungi only generate naturally in their respective biome.   
To determine the size of huge fungi, a random integer is chosen between 4 and 13 [inclusive]. There is then a 1/12 chance of that integer doubling. The resulting value is the height of the trunk. A layer of wart blocks and shroomlights, or leaves, is then spread around the trunk, being able to generate up to 3 blocks out from the trunk. There is a 3x3x2 hollow ring that surrounds the lowest block of a fungus tree where no blocks generate.  
![fungi_gen](https://i.imgur.com/8dHv9g5.png)  
Layers 1-3 the leaf diameter (measured as a square) is 3, then at layers 4-17 it's 5 blocks, at layer 18-25 it reverts back to 3 blocks wide, and at layers 26 and 27, it gets reduced again to just 1 block wide.  
2. Player Grown
If a player bonemeals a fungus, and if that fungus is directly above its respective nylium block (e.g. warped fungus on warped nylium), there will be a 40% chance that it will grow. So if a player bonemeals a fungus 4 times, there will be an 87.04% chance it will grow, (99.998% chance if 20 attempts are made).  
Note: A fungus plant (the 'sapling' of huge fungi), will not grow by itself through random ticks, unlike traditional saplings.  
When a fungus grows, it doesn't check for blocks above or around it, this allows for pistons to be situated adjacent to the trunk, allowing for extremely fast cycle speeds.  
Note: That a fungus will not grow if it is obscured by the height limit or if it is placed outside of the world.  
3. Dispenser Grown
Similarly, if bonemeal is placed inside of a dispenser, and then that dispenser is pointing into a fungus plant on its respective nylium block, then there will also be a 40% chance of it growing per attempt. Dispensers can be used to automatically grow the fungus plant, and up to 5 can surround a fungus plant, allowing for fungi to grow at 5hz on average.  
Note: There is no diminish in return with bonemeal efficiency, and dispensers used.  

## Growth Mechanics [Advanced]
**Phase 1: Defining the shape/bounding box of the Huge Fungus**

The below steps are going to create the outlines of the huge fungus and the bounding box as to where blocks can generate



1. An attempt to generate a nether tree is made if the fungus is placed on top of the correct variant of nylium



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.png "image_tooltip")




2. The height of the trunk is found, between 4 and 26 blocks tall (non-uniform)



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.png "image_tooltip")




3. Step 3 is creating the stem if it’s a thick, 3x3 stem, but I’m excluding this as it’s not practical to farm thick huge fungus
4. The height from bottom to top of the hat ‘i’ (the hat is like the leafy area of the huge fungus) is found, note ‘hatHeight’ is the ‘i’ value from the trunk height calculations. This also means that the tallest hat that can generate is 15 blocks!



<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image3.png "image_tooltip")




5. The height from the bottom of the trunk to the bottom of the hat ‘j’ is found. Note that if ‘hatHeight’ is the smallest out of the 2 options in step 4, then ‘j’ will =0 and the hat will be touching the floor



<p id="gdcalert4" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image4.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert5">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image4.png "image_tooltip")



**Phase 2: Block Type Distribution**

This phase now moves on to calculating the positions and type of block on a per-block basis, meaning that as we run the loop over and over again and slowly increment our XYZ coordinates of ‘m’, ‘k’, and ‘n’ we will build the huge fungus block by block.



1. A loop that increments through each Y layer of the hat starts at the lowest layer of the hat ‘j’ and finishes 1 block above the trunk as the Y coordinate ‘k’ is incremented one more time after reaching the limit of hatHeight



<p id="gdcalert5" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image5.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert6">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image5.png "image_tooltip")




2. The distance from the trunk ‘L’ is found. The 3rd line is what only lets huge fungus that are at least 4 blocks off the ground and have hats 8 blocks high have a radius of 3



<p id="gdcalert6" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image6.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert7">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image6.png "image_tooltip")




3. Initial conditions for the loops for the X coord ‘m’ and the Z coord ‘n’ are set. Essentially the algorithm sweeps through the roughly rectangular prism-like shape of the huge fungus layer by layer starting at -X, lowest Y, -Z and finishing at +X, highest Y, +Z



<p id="gdcalert7" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image7.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert8">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image7.png "image_tooltip")




4. For aesthetic reasons, the hat of a huge fungus is divided up into 4 regions, each of which has its own chances of generating specific types of blocks (different weightings for wart blocks and shroomlights). These different regions essentially boils down to:
    1. The inside of the hat where no block is placed on the outside of the bounding box ‘bl4’
    2. The corners of the hat where the blocks are placed on the outermost parts of the bounding box ‘bl5’
    3. A 3 high ‘brim’ of the hat that makes up the lowest 3 layers of the bounding box, i.e. the area where vines generate ‘bl6’
    4. The edges of the hat where the blocks are placed on the outermost edge of the bounding box, neither ‘bl4’, ‘bl5’ or ‘bl6’

    	To help calculate whether a specific value of ‘m’ or ‘n’ meets the criteria of ‘bl4’, ‘bl5’ or ‘bl6’, variables ‘bl2’ and ‘bl3’ are found. ‘bl2’ is true when ‘m’ is furthest away AND ‘L’ blocks, from the trunk. It’s the same thing for ‘bl3’ except instead of ‘m’ it’s ‘n’




<p id="gdcalert8" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image8.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert9">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image8.png "image_tooltip")




5. Next up we need to determine if we meet the criteria of either ‘bl4’, ‘bl5’ or ‘bl6’. As mentioned before ‘bl4’ is the region in which no blocks touch the bounding box, i.e. no blocks are the furthest from the trunk. This means that both ‘bl2’ and ‘bl3’ need to be false so the outermost blocks on the side aren’t included, as well as the topmost blocks on the top of the hat



<p id="gdcalert9" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image9.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert10">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image9.png "image_tooltip")




6. ‘bl5’ is even simpler as it’s the corners of the hat meaning that both the X and Z coordinates need to be the furthest from the trunk, i.e. ‘bl2’ and ‘bl3’ are true



<p id="gdcalert10" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image10.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert11">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image10.png "image_tooltip")




7. ‘bl6; applies to Y layers of the hat starting from the bottom, ‘j’, and moving 3 blocks up to ‘j’ + 2. Have you ever grown a bunch of huge fungi on top of each other and noticed how the first 3 layers of the huge fungus never generate shroomlights and are also hollow? Well this is the vines region and for warped trees, it only consists of wart blocks



<p id="gdcalert11" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image11.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert12">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image11.png "image_tooltip")




8. Now, this just leaves the outermost edges region. When you think about it, the edges of the tree are basically everything but the corners and the inside. Think of it like a 3x3 Rubiks cube where the inside mechanism is ‘bl4’, the corner pieces are ‘bl5’ and the edges are well, the edge pieces (and the centre pieces are the trunk). So the way to find the edges is to first find where the other regions are and then negate that. This isn’t strictly written anywhere in the code but it can be derived
9. Now as there is some overlap between the vines region ‘bl6’ and the inside region ‘bl4’, some more work needs to be done to further separate these two. This turns ‘bl6’ into a hollow shell as it removes all blocks in ‘bl6’ that aren’t touching the bounding box



<p id="gdcalert12" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image12.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert13">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image12.png "image_tooltip")




10. Finally, the block type distributions are calculated using a multitude of hard-coded values, all for aesthetic reasons



<p id="gdcalert13" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image13.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert14">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image13.png "image_tooltip")


I purposefully included the additional vine region code to show that there is no chance for shroomlights to generate in that region. Some important takeaways here is that there are regions of the tree where wart blocks have an exceptionally high chance of generating, 98%, meaning that a setup could be made to harvest blocks only from that region as opposed to say harvesting from the region ‘bl4’ where they only have a 20% chance of generating. 

Note that decoration chance here refers to shroomlights and the ternary is used for determining the chance of a weeping vine generating there if the block is a netherwart block.

**Additional Stuff**

There are some minor probability errors in this (and the vines layer being 1 too high) but this should give u a more intuitive idea of what’s going on here



<p id="gdcalert14" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image14.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert15">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image14.png "image_tooltip")


~~Why look at the probability table when u have the code right above doe~~

That’s pretty much it, there’s a phase 3 where the weeping vines/vine region is generated but this is a bit more complicated and not too applicable for huge fungus farming so I also left it out.

If you wanna take a look at the code yourself in more detail, join the fabric project discord where’s there’s heaps of info on how to decompile the code and find whatever you’re after:

[https://discord.gg/T2GJp3aX8q](https://discord.gg/T2GJp3aX8q)

And of course, if you want even more accurate graphics and charts about nether trees as well as all the different farms for them, consider joining the nether tree discord server:

[https://discord.gg/EKKkyfcPPV](https://discord.gg/EKKkyfcPPV)

Oh and also if you want to mess around with generating huge fungus on a graphing calculator here’s a little project I’m working on

[https://www.desmos.com/calculator/tsrdkppcbk](https://www.desmos.com/calculator/tsrdkppcbk)

Anyways, thanks for taking the time out of your day to learn more about huge fungus and I hope you have a lovely day :)



## Block Properties
A table showing the various blocks that are heavily involved in a Huge Fungi Farm:
![fungi_table](https://i.imgur.com/rQrmRej.png)

## Harvesting Methods
There are a multitude of ways in which huge fungi can be farmed, here are some of the more common and technical methods.

### Manual Harvesting
During the early game where redstone resources are limited, certain manual harvesting methods are utilised. While it is recommended that a player gathers resources to make a farm, rather than spending that time manually farming, certain manual harvesting methods have arisen.

### Nether Exploration
Collecting huge fungi blocks in their own biomes is a great way to collect a sufficient amount of wood and decorative blocks early game. 
The fastest way to use this method is to make an efficiency 5 netherite axe and hoe, then travel to the nether and fly around using an elytra until you come across a warped or crimson forest. If it's a crimson forest, be sure to bring warped fungus to repel hoglins (must be planted, with an area of effect of 15 blocks). 
After planting a fungus, build a 3x3 platform of gold blocks with a beacon on top for the haste 1 effect. Fly up from the beacon base and destroy the netherrack obstructing the beacon beam. The haste 1 effect, will reduce the mining time per stem block by 2gt [check]. This will also help when mining wart blocks and shroomlights, as when combined with an efficiency 3 netherite or diamond hoe, you will be able to mine each block in 1gt [check]. Setting up and removing a 1 layer beacon in the nether takes on average 600gt, so this beacon method will only be effective if you are mining more than 300 stem blocks, or if it is necessary for instant mining more than 38 blocks. 
Once the beacon is set up, you can begin harvesting trees. Huge Fungi normally generate together in clumps, so it's best to climb to the top using twisting vines, and then mine from the top down. Be sure to use a hoe to mine the leaf blocks (wart blocks and shroomlights), and your axe to harvest the stems.
![fungi_harvesting](https://i.imgur.com/Vk8g6bi.png)
To marginally increase speed, carry with you a few shulker boxes of unbreaking 3, efficiency 5, gold axes to reduce the mining speed by 1gt per stem block. 

### Greenhouse
Place 2 nylium in a 3x3 grid pattern with 6 blocks in between each one. Then construct a water stream platform beneath it. Have the water stream funnel the items into a chest storage, connected by a hopper. To use the manual farm, place down a fungus on each nylium block, then grow the fungi using bonemeal. Now climb to the top using ladders, a bubble column, scaffolding or vines, and mine out the huge fungi from the top down. After harvesting the huge fungi, collect any items that didn't drop into the water stream below. 
![fungi_greenhouse](https://i.imgur.com/wUlpwwM.png)

### Block Chamber/Storage
Build a traditional semi-automatic huge fungi farm, and connect the block stream to a block chamber. When you would like to harvest the blocks, climb to the top of the storage and mine them out using haste 2 and an efficiency 5 netherite axe and hoe.
[https://www.youtube.com/watch?v=jytLCfRu_54](https://www.youtube.com/watch?v=jytLCfRu_54)

### Semi-Automatic
Semi-Automatic harvesting is when everything in a farm is self-sufficient except for the input. 
In the case of semi-automatic huge fungi farms, this would be the placement of fungi plants and often, but not always, supplying it with bonemeal.
- Positives:
 Very fast rates, inexpensive, lag friendly on small scales, produces every block from the tree (wart blocks, shroomlights and weeping vines included)
- Negatives: 
Usually quite block inefficient, consumes high amounts of bonemeal, high-frequency designs can be locational and directional
1. Single Core
   1 nylium, slow rates, average efficiency.  
   [https://www.youtube.com/watch?v=yY64bj2DjhA](https://www.youtube.com/watch?v=yY64bj2DjhA)
   [https://www.youtube.com/watch?v=nlfAwEV-cCk](https://www.youtube.com/watch?v=nlfAwEV-cCk)
2. Dual Core
   2 nylium, average rates, low efficiency.  
   [https://www.youtube.com/watch?v=XAIsjTTGpPA](https://www.youtube.com/watch?v=XAIsjTTGpPA) (outdated)
   [https://www.youtube.com/watch?v=yY64bj2DjhA](https://www.youtube.com/watch?v=yY64bj2DjhA)
3. Quad-Core
   4 nylium, fast rates, low efficiency.  
   [https://www.youtube.com/watch?v=rhncko7vKLs](https://www.youtube.com/watch?v=rhncko7vKLs)
4. N-Core
   A farm consisting of N-modules connected by a sufficient transportation system. Extremely fast rates, maxing out at 5,400,000/h with an auto-clicker (20hz) and around 1,400,000/h at vanilla placement speed (5hz), high efficiency.  
   [https://www.youtube.com/watch?v=rhncko7vKLs](https://www.youtube.com/watch?v=rhncko7vKLs)
   [https://youtu.be/nMkffHVLHvw](https://youtu.be/nMkffHVLHvw) (ongoing)

### Self-Sustaining
Once built, a Self-Sustaining Huge Fungi Farm can run without player interaction as long as it is kept loaded.
- Positives: 
Can run in loaded chunks without the need for a player to plant any fungus, can generate a surplus of bonemeal
- Negatives:
 Slow, more expensive to build, laggy, and limited (if any) wart blocks and shroomlights are produced, often unreliable.
 
1. Growing Area
   Max 5x5 area of nylium to grow fungus plants using bonemeal.  
   Flying Machine  
   Flying Machine pushing into a TNT dropping zone.  
   [https://www.youtube.com/watch?v=dEToZ4wLuqY](https://www.youtube.com/watch?v=dEToZ4wLuqY)

2. Blast Chamber
   Directly Above, Piston wall pushing to external
   [https://www.youtube.com/watch?v=M51sTIwDo5w](https://www.youtube.com/watch?v=M51sTIwDo5w)

3. N-Core
   Theoretically, the fastest way to harvest huge fungi 

### RNG Huge Fungus Farming
Manipulate RNG to generate a 5x5 platform of fungus, which all grow on the first bonemeal attempt to produce the largest huge fungi possible (7x7x27)

### Fungus Farming
The collection of fungus plants to grow in a huge fungi farm

Dissection of nether foliage distribution:

[https://www.youtube.com/watch?v=SOcY9En4l_E](https://www.youtube.com/watch?v=SOcY9En4l_E)
