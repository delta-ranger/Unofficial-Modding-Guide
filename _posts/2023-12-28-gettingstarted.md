---
title: Getting Started with Ready or Not Mapping
date: 
categories: [Map Modding]
tags: [maps, introduction]
description: An in-depth guide on getting started with Ready or Not Mapping
author: Delta|https://www.nexusmods.com/readyornot/mods/3072/
---

# Getting Started with Ready or Not Mapping [POST 1.0 RELEASE]

## Introduction
Ready or Not is currently *Unofficially* supported by dedicated modders. Due to the hard work of them we are currently able to get around 80-90% of the experience that Official Maps have; there is expected to be a little bit of Jank when creating maps. 

>Ready or Not mapping **REQUIRES** that you download and use **Unreal Engine 4.27.2**. Do not download or use any other version.
{: .prompt-danger }

If this is your first time creating a map, be aware that the level of experience to create something functioning is considered *Intemediate*. It is recommended that you spend some time watching and learning some beginner videos for UE4 on youtube before you start asking too many newbie questions.

If you need help or do have any questions, don't hesitate to join the [RoN Custom Maps Discord](https://discord.gg/NGAtrTmXBR), we're always happy to help out a fellow Level Designer!

### The Level Design Cycle for RoN

We currently provide a template project that contains all the gameplay features/blueprints needed to make RoN maps here: [Installation](##Installation).

For the Level Design process, you pretty much do it all within the Unreal Engine project provided, and can use whatever (reasonable) method you want to create your level. 

It is important to note that currently we cannot test gameplay within Unreal Engine and can only do it within the Game. To actually play the level you need to package your level into a deployable .PAK file that you place within the game files. Each change you need to test will need to repackaged into a new .PAK file. More info about Package can be ready here: [Packaging your Map](#packaging-your-map)

![Ready or Not Development Cycle](/assets/gettingstarted/DevCycle.png)

### Development Example
If you would like to see what its like to make a map for Ready or Not, I recorded my entire development for [Hell Comes to the Hills](https://www.nexusmods.com/readyornot/mods/3072/). 

[Youtube Playlist: Ready or Not: Custom Map Development by Delta](https://www.youtube.com/playlist?list=PLeMpdkJrX_CGU-kIZJz4nbZ8KMNNWhWz3)

## Installation

### Unreal Engine 
1. Download and install the [Epic Launcher](https://store.epicgames.com/en-US/download) and install Unreal Engine 4.27.2.
2. Open the Launcher, go `Unreal Engine` on the left side bar, then `Library` along the top toolbar
3. Under `Engine Versions` press the `+` icon and next to the Version Number, click the drop down and select `4.27.2` and Install

### The Template Project
>Currently we have a pre-release project that you **NEED** to use for maps to work for 1.0. Do not use the Covered Bones Template available on Nexus. THIS WILL LIKELY CHANGE IN THE NEAR FUTURE.
{: .prompt-warning }

1. Create a Folder anywhere and name it whatever you want. This will be the folder you need to extract the following files into. I will refer to this folder as `RoNTemplate`. 
2. Download and Extract the following into `RoNTemplate`
  * https://drive.google.com/file/d/17uyZ_hrklBIN1ci88GajcnT1pOeFQJGx/view?usp=sharing
  * https://drive.google.com/file/d/1BsavVwAYh6kq7hUs9wD3o2A5_nY6rWn6/view?usp=sharing
3. Download the following Hotfix. **DELETE** your `/plugins/` folder within `RoNTemplate` and then Extract the contents of the hotfix to `RoNTemplate`
a. https://drive.google.com/file/d/1IAD9qjTfN2JJ5yBUZ3R9Jhzi8R0YdgCS/view?usp=sharing 
4. Open `RoNTemplate` and open the `ReadyOrNot.uproject`. This will launch the Project and may take a couple of minutes to load if this is your first time using Unreal. Just be patient.

>**HELP! I Get 100s of errors when I load up the project!**
This is normal, it's just missing some game files that you can extract later on. We also don't have access to everything. Any errors you get on start-up are fine and will not give you any issues, provided you followed the steps so far exactly.
{: .prompt-warning }

### The Folder Structure
Before we continue, it is very important to understand the folder structure of an Unreal Project. 

Unreal uses this concept of a `Content` folder, this is where all the game content and maps for the game reside (This will be located at: `RoNTemplate\Content`).

When you install a .PAK file you are essentially adding or overriding assets here.

As modders we have a lot of power to change and edit any files within this folder, but it comes at with some pretty big responsibility. Any change you make here will likely affect other mods that people use. This a more common occurance when a lot of use are using the same Marketplace or Quixel/Megascans assets. 

To prevent us from stepping on each other's work, It is highly suggested that you create a directory for yourself within the `Mods` folder and place **ALL** your assets there. It should look like the following:
``` 
Content  
    Mods  
        Username
```
>You will see other folders in here such as `Blueprints` & `Reap`. **DO NOT** edit these files in here.
{: .prompt-danger }

## Your First Map
>If you have seen previous YouTube tutorials it may have said you need to set up the Render Settings, or modify the Packaging settings. With the latest version of the Template, you do not need to change or extract any settings. You should be good to go from the first time you launch.
{: .prompt-warning }

### Bare Essentials for Gameplay
1. Following the previous steps, go to your `Mod\Username` directory in the Content Browser and create a new map and open it
    * Whatever you name it will be what it shows up as in-game
2. Once opened, click on `Blueprints > Open Level Blueprint`
3. Under `Class Defaults`, check the details panel and click on `Item Data` and add the `ItemData` asset
4. Click Compile, and you can save and exit the window
5. Under the `Place Actors` tab, chuck down a Geometry floor and a couple of walls using boxes to act as a space to play in
    * If everything is black, press `Alt+3` to switch to `Unlit Mode`
4. Under the `Place Actors` tab, go to `All Classes` and search for and place down the following:
    - [ ] 5 Player Start Actors
        * This is where your Player and Team Spawn
    - [ ] Nav Mesh Bounds Volume
        * Required for your AI to function, whatever this covers will be where your AI can walk
    - [ ] World Data Generator Actor
        * This will build the navigation and gameplay elements when you load your map
    - [ ] Roster Scenario Spawner Actor
        * This creates your gameplay objectives and spawns
    - [ ] 2 BP_AISpawn_Reap_V1
        * Technically not needed, but without it the map will automatically complete when you play it and you wont be able to test it.
        * You can use the `AISpawn` actor, but Reap's one has some better control

### Additional Things to Add
#### Lighting
Currently your level doesn't have any light, so we should set up the basic stuff so we can actually see when we playtest it. Add the following:
- [ ] Lightmass Importance Volume and adjust it to cover the playable area
- [ ] 1 Directional Light to act as a Sun/Moon
    * Set it to `Stationary` for dynamic shadows to work properly
    * Under `Details > Atmosphere and Cloud` enable `Atmosphere Sun Light`
        * You can use `Ctrl+L` to change the direction of your sun now
- [ ] 1 Sky Light Actor
- [ ] 1 Sky Atmosphere Actor
- [ ] 1 Exponential Height Fog Actor
- [ ] 1 Reflection Probe that encompasses the map
    * Without build reflections, the models/guns in game can look flat and not lit properly.

#### Configuring the AISpawn
The 2 `BP_AISpawn_Reap_V1` that we placed down are not configured yet, so lets do that by adding a Civilian and Suspect:
1. Select an AISpawn and under `AISpawn > Spawn Array` press the `+` symbol
2. Expand the new array element and also expand `Spawned AI`
    * This is where you choose what AI to Spawn
3. To begin with change the `Data Table` to `AIDataTable_Gas` & the `Row Name` to `Civilian_Gas_Employee_01`
4. Select the other AISpawn and follow the same steps but change the `Row Name` to `Suspect_01_Shotgun`

Your AISpawn details should look something similar to this:
![AISpawn Detail Example](/assets/gettingstarted/AISpawnDetailExample.png)

You can select any Data Table and Row Name for your map and you are not restricted to 1 Data Table. You will need to experiment to see what you will want in your map as all the AI behaves differently.

#### Doors
Doors are pretty simple to place down into your map. 
1. Under `Place Actors` search for `BP_Door_Reap_V1` and place it into your level
2. Within the Details panel, change the `Door Type > Row Name` to the sort of Door you would like
    * For testing purposes I like to use `Wood_Door_Painted`

>The basic Template does not include the Static Meshes so you can preview the door. To get them you will need to follow the steps in [Using Ready or Not Game Assets and Content](#using-ready-or-not-game-assets-and-content)
{: .prompt-info }

>For Doors to actually work and AI to stack up and use them properly, it seems to require that the door be connected to an actual 'Room-like' structure (ie: enclosed by 3-4 walls). It's unclear on the requirements of them at the moment, but the way the AI handles doors, stacking and clearing is done via the world generation when you first load the map in-game.
{: .prompt-info }


### LevelData
LevelData will show you how your map will look in the Level/Mission Select screen. Currently LevelData does not work and we are waiting for a patch from VOID. 

## Packaging your Map

### Building / Baking
If you've been following along so far, then you probably have not built your map. Building a map fixes up changes in Geometry & Volumes, Lighting & Reflections and Pathing.

To build your map you simply press the `Build` button on the toolbar. Building times can vary, but the largest determining factor is usually Lighting. As map size increases and the quality of Lighting is changed, builds can take anywhere from 2mins all the way up to 6hrs and longer. At the moment the map should probably only take a couple of seconds. 

You can change the lighting quality if you press the drop-down on the Build button. However Lighting and all its nuances are left for you to discover and research for yourself. 

Once the build completes, You should `File > Save All`.

### Cooking
Cooking is the process of turning the project into content that can be deployed on other machines. We also only want cook content that is only important for the map to keep file size small. We need to configure this first:

1. Go to `Edit > Project Settings` and on the side tab select `Project > Packaging`
2. Under `Packaging > List of maps to include in a packaged build` you should see an element that shows the directory for Reap's `Example.umap`. We want to replace this entry with the directory of our Map.umap. 
3. Press the `+` and create another entry here and navigate to your map's BuildData, it should be in the exact same location as your map.
4. You do not need to change any settings and it should look something like this:
![Packaging Settings Example](/assets/gettingstarted/AISpawnDetailExample.png)
5. Make sure you Save All before proceeding
6. Select `File > Cook Content for Windows`
    * This could take some time for your first cook as it compiles all the shaders. Just be patient.

You should hear it succeed, but a message will not remain on screen when it completes. If it failed the notification will stay on the screen

### Packaging & Installing your Map
Packaging will turn our cooked content into a single and compressed file that we can actually use to play in game and distribute to other people. 

>There are a bunch of Unreal packers out in the wild, but only use the following method for maps.
{: .prompt-warning }

1. Create a folder somewhere to act as your staging location for packing, name doesn't matter but I use `Paking`. I recommened either your Desktop or Documents as the folder location.
2. Within this folder create a new folder using the following naming convention: `pakchunk99_YOURMAPNAME`
    * the 99 after `pakchunk` indicates the load order of paks. We keep maps at 99.
    * Replace `YOURMAPNAME` with whatever your map's name is. This will not show up in game but will indicate to people downloading your mod what it is.
    * Sometimes it is required to append `_P` at the end of the name if you are replacing files (i.e. P for Patching). But since you shouldn't be replacing any files this is unneeded. 
3. Go to `C:\Program Files\Epic Games\UE_4.27\Engine\Binaries\Win64` and create a .bat file here with the following arguments:
    ```
    @if "%~1"=="" goto skip
    @setlocal enableextensions
    @pushd %~dp0
    @echo "%~1\*.*" "../../../ReadyOrNot/" >filelist.txt
    .\UnrealPak.exe "%~1.pak" -create=filelist.txt -compress
    @popd
    @pause
    :skip
    ```
4. Create a Shortcut to the .bat file you created and move it to your staging folder from Step 1
5. Navigate to your Template/Project folder and go to: `\RoNTemplate\Saved\Cooked\WindowsNoEditor\ReadyOrNot` and copy the `Content` folder
6. Paste your `Content` folder into your `pakchunk99_YOURMAPNAME` folder
7. Drag your `pakchunk99_YOURMAPNAME` folder ontop of your .bat shortcut. A command window will pop up and tell you when it is complete. If successful you should have a new file in your staging directory named `pakchunk99_YOURMAPNAME.pak`
8. Copy your `pakchunk99_YOURMAPNAME.pak` to the corresponding folder where you installed Ready or Not: `C:\SteamLibrary\steamapps\common\Ready Or Not\ReadyOrNot\Content\Paks`
9. Your map should now be able to be played in-game

This is an example of what a Staging folder can look like:
![Staging Folder Example](/assets/gettingstarted/AssetsToCook.png)

>You will be jumping between these folders a lot, I really recommend putting your Staging folder, Project Cooked folder Ready or Not Pak directory to your Quick Access bar.
{: .prompt-tip }


## Testing your Map
> I highly recommend that you download the [Console Unlocker - Camera Mod](https://www.nexusmods.com/readyornot/mods/716) by QuantumNuke75. It will give you ability to use a Free-Look camera and also use basic Unreal Engine Console Commands. Both functions are extremely helpful for testing.
{: .prompt-tip }

The easiest way to test your map is to simply load up the game and select it from the Mission Select screen. If all works well then it should take a couple of seconds to build WorldGen and plops you into the map.

If you installed the Console Unlocker mod, you can also load into it faster by hitting ~ and typing `open YOURMAPNAME` and pressing execute. 
* `YOURMAPNAME` should be what the .umap name is called within your project. NOT the package name. 
* For Hell Comes to the Hills the command for opening it is `open Hell_Comes_to_the_Hills`.

### Common Issues with Map not Loading
Make sure you did the following:
- [ ] You are using the most up-to-date version of the template [linked above in this guide](#the-template-project).
- [ ] You are using the packing method [described above in this guide](#packaging--installing-your-map).
- [ ] Only include the `Content` folder from `\RoNTemplate\Saved\Cooked\WindowsNoEditor\ReadyOrNot`
- [ ] You have all the actors mentioned in [Bare Essentials for Gameplay](#bare-essentials-for-gameplay).

If your map is still not not launching, then you may want to reach out for help on the Mapping Discord linked above.

## Updating your Map after Playtesting
And Voilà! You've just gone through your first iteration of RoN Mapping! From here the world is your oyster in terms of where you want to take it. There are just some final important steps needed to know when you re-cook and package your map:

1. After you recook your map, it is recommended that you delete the `Content` folder within your staged `pakchunk99_YOURMAPNAME` folder instead of overriding it. 
    * This is because you want to have a clean copy of the content folder for each playtest, overriding your folders may leave residual files and settings that may mess with your playtest
2. BEFORE you launch the game. Navigate to `C:\Users\USERNAME\AppData\Local\ReadyOrNot\Saved\SaveGames` and delete the WorldGen.sav file that has the same map name as your map.
    * e.g. Hell Comes to the Hills filename looked like `Hell_Comes_to_the_Hills_WorldGen_40173.sav`
    * **This step is critical!** Each time the game launches your map for the first time, it generates World Data and stores it so the map launches faster the next time you play it. However the game does not know when your map has been changed. So you will likely be playing on a map with outdated build information resulting in quite a few bugs.
    * The WorldGen contains information such as how doors are positions and connected, room sizes and layouts, stacking points, navigation, hiding spots and cover and a probably a lot of other things we don't know about yet. So it is important to make sure you generate new World data with each iteration!

>Just like with the other folders, it is recommended you add the SaveGames directory to your quick access, as you will be visiting it quite a fair bit!
{: .prompt-tip }

## Using Ready or Not Game Assets and Content

If you want to use RoN game assets you will need to extract them yourself using UModel: https://www.gildor.org/en/projects/umodel

There are some restrictions here. It's generally not recommended to extract/save everything as it will take a very long time to do and will likely crash/fail to do so. Putting ALL the assets into your project will also significantly increase launching time when you open UE.

It is therefor recommended to extract mostly what you need, which mostly consists of Static Meshes, Skeletal Meshes and maybe Textures

To get the assets:
1. Open UModel and select your `/ReadyOrNot/Content/Paks` folder
2. Click `Tools > Scan Content` and select `Unreal Engine 4.27`
3. Tick `Flat View` in the top-left corner.
4. Sort by `Stat`
5. Select all with `Stat` = 1
6. Select `Tools > Save Selected Packages`

You can repeat this for `Skel` 
And you may want to repeat for `Tex` as well. But be warned that you will not have the materials to display onto the meshes. You will need rebuild the materials from scratch, and saving the Textures is not usually worth the time and hassle. 

Once you are done copy the saved files to your project. For UModel the `Game` folder is the `Content` folder for your Project. So you can just copy the contents of `Game` into your `Content` folder. 

Restarting the Editor may take some time as the project discovers and indexes all the cooked assets.

>If for some reason the assets are not loading double check the project settings so that it can use cooked content: https://docs.unrealengine.com/4.27/en-US/WorkingWithContent/CookedContent
{: .prompt-warning }

Video Guide by Reap: https://www.youtube.com/watch?v=-xQKyV7_6fE 


