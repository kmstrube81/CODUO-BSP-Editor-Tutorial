# How to add Search and Destroy with CoD BSP Editor

Certain maps seem like they would be a great fit for Search and Destroy but the original creator didn't add it into the map. Has this situation ever happened to you? It used to be a major bummer but with the power of the [CoD BSP Editor](https://github.com/kartjom/CoD-BSP-Editor), we can add S&D (or CTF, a tutorial for that to come later) to any map we desire.

## Getting Started
To begin editing a map, please make sure you have the following tools
- [CoD BSP Editor](https://github.com/kartjom/CoD-BSP-Editor)
   - needed for editing the map .bsp of course. It's just a stand alone exe, no installer. I saved mine in my Call of Duty directory, under the Tools folder.
- A Pak File editor (pakscape, 7zip, WinRar etc.)
    - needed for retrieving your desired .bsp file from its pk3 file. I use 7zip.
- Last Demon's [Position Debugger Mod](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/raw/refs/heads/main/assets/uo/z_demon_position.pk3) for UO. 
    - needed for getting coordinates to place new models/brushes. Either create a new folder in your Call of Duty directory to save it in (no spaces), or throw it into your UO folder. Save this [config file](https://raw.githubusercontent.com/kmstrube81/CODUO-BSP-Editor-Tutorial/refs/heads/main/assets/uo/edit.cfg) next to it as well.
 - VE_AG_RA's [Xmodel Hitbox Calculator](https://drive.google.com/file/d/1olftpo5OZqe6JGAYirQt7wSNUaktXm2I/view?usp=sharing) Spreadsheet
     - a tool that I use for adding hitboxes for models that don't have collision. Right now it's pretty barren but as I created custom hitboxes, I will keep adding to it.

## Step 1. Gathering Coordinates
Start by editing your edit.cfg file. Make sure that demon_position is set to 1. Set the game type to whatever is compatible with the source map you are editing (some maps may only work with dm or ctf for example). Then set the devmap to your source map. (In our example: ctf_chateau). Save your edit.cfg and make sure it is saved in the same folder as your z_demon_position.pk3.
``` cfg
//DEMON POSITION made by lastdemon
set demon_position 1

g_gametype ctf
devmap ctf_chateau
```
Launch Call of Duty: United Offensive Multiplayer. If you saved z_demon_position.pk3 and your edit.cfg file in your UO folder you are good to go. If you created a new folder, first click mods and then your folder name (I did DSR_Edit), and then click Launch and let the game reload.
Press ~ to bring up the developer console. Type the following and press enter to start the position debugger mod.
```
exec edit.cfg
```
Your source map will start to load and we can gather coordinates. Once loaded, join up and head to your desired bomb site A location. (team and weapon don't matter) Go to where you want the bomb site A model to be. Take a screenshot, write them down, or somehow otherwise note the first 3 values on the left of the screen. These are your bomb site A model coordinates. The first value is your X coordinate, the second your Y coordinate, and third your Z coordinate (the fourth value is your horizontal angle value)
![The bomb site A coordinates on the left](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/34f008d2d5e6f1e05c0f42cd873071785986b051/assets/img/bombA-marked-00.jpg?raw=true)
Next go to each of the four corners of area of where you would like attackers to be able to plant the bomb. Take note of the 3 coordinates at each location.
![The bomb site A trigger coordinates](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/34f008d2d5e6f1e05c0f42cd873071785986b051/assets/img/bombA-marked-01.jpg?raw=true)
Now go to the desired bomb site B location and take the same steps for the bomb site B model
![The bomb site B coordinates](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/34f008d2d5e6f1e05c0f42cd873071785986b051/assets/img/bombB-00.jpg?raw=true)
...And for the plant area as well
![The bomb site B trigger coordinates](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/34f008d2d5e6f1e05c0f42cd873071785986b051/assets/img/bombB-marked-01.jpg?raw=true)

## Step 2. Editing the BSP
Now that we have our coordinates, we need to get the source bsp file to start editing it. For stock CoD 1.0 maps, open up pak4.pk3 or pak5.pk3 with your pak editor. For maps introduced, in later patches, open up pak9.pk3, paka.pk3, or pakb.pk3. For stock UO maps, open up pakuo01.pk3 or uomappack00.pk3. For custom maps, open up whatever pak file corresponds to the custom map you want. Your .bsp file will be in the maps\mp folder of whatever pak file it belongs too.
![ctf_chateau.bsp in the maps\mp folder](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/a04a724c4ca3a34913e972e964e3e8540236bbc3/assets/img/windows00.png?raw=true)
Save that .bsp file somewhere on your PC. Open up CoD BSP Editor and open up the .bsp wherever you saved it. It will take a moment to populate all the entities and brushes in the map. Once it is done, we can start adding our required components for Search and Destroy. 
#### 1. Make sure we have the textures/common/trigger material.
 Click Editors, Materials editor in the toolbar to check for the textures/common/trigger. If there is no textures/common/trigger material, we will need to create it. In the Materials editor window, click Tools, Create material. Create a material with the following properties.
```
Material name:
   textures/common/trigger
Surface type:
   128
Surface properties
   671088641
```

![Create a new textures/common/trigger material if necessary](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/34f008d2d5e6f1e05c0f42cd873071785986b051/assets/img/bsp-editor00.png?raw=true)

#### 2. Create script_model for bombzone A 
Choose an xmodel for your objective you want the attackers to destroy. For chateau we will use HQ radios placed near where the retrieval objectives are placed. Find the coordinates for bomb site A that you gathered in step 1. Click List, Add entities by string in the toolbar and paste in the following and click confirm to add it. Replace [xmodel] with the path to the model you wish to use. Replace [x] [y] and [z] with the x, y, and z coordinates of the objective from your notes.  Optionally replace [yaw] with your rotation value from before (round to the nearest 0, 90, 180, or -90 for best results with adding hitboxes in the next optional step), otherwise use 0 for the model to face due north.
>Objective Script Model
```
{
    "classname" "script_model"
    "model" "[xmodel]"
    "origin" "[x] [y] [z]"
    "angles" "0 [yaw] 0"
    "script_exploder" "1"
    "targetname" "sd_objective_a"
}
```
![Create the bombzone A model](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/34f008d2d5e6f1e05c0f42cd873071785986b051/assets/img/bsp-editor02.png?raw=true)
#### 2b. (Optional) Add hitbox for bombzone A script_model
To add a hitbox, we need to create a brush with a clip material. For metal surfaces, use textures/common/clip_metal. For all other types of surfaces, use textures/common/clipfull. Just as before we need to check the Editors, Materials editor option in the toolbar for our desired clip type. If not found, create a new texture with the following properties based on your preferred clip type. 

| Material name: | Surface type: | Surface properties: |
| --- | ---: | ---: |
| textures/common/clipfull | 16544 | 671298112 |
| textures/common/clip_metal | 13631648 | 671286976 |
| textures/common/clip_nosight_metal | 13648032 | 671290944 |
| textures/common/clip_nosight_dirt | 6308000 | 671290944 |
| textures/common/clip_nosight | 22036640 | 671290944 |

![Create a clip material (clip_nosight_metal for HQ radio)](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/34f008d2d5e6f1e05c0f42cd873071785986b051/assets/img/bsp-editor03.png?raw=true)
Now that we have our material, to add the hitbox we need to create a brush for it. Click Utility, Create brush in the toolbar. Enter your bounding box coordinates for your hitbox and your clip texture material name for the clip type.
![Create a hitbox brush](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/34f008d2d5e6f1e05c0f42cd873071785986b051/assets/img/bsp-editor04.png)
Your bounding box can be easily calculated by using the  [Xmodel Hitbox Calculator](https://drive.google.com/file/d/1olftpo5OZqe6JGAYirQt7wSNUaktXm2I/view?usp=sharing) Spreadsheet for the models that have already had hitboxes made. Figuring out the bounding box values is beyond the scope of this tutorial. If you don't know how to figure it out, use one of the models with the precomputed hitboxes in the calculator. Some models, for instance the kubelwagon, have multiple hitboxes and will require multiple brushes. To use the calculator, just insert the objective X Y and Z coordinates into the X Y and Z cells in the calculator and past the bounding box start/end values into the Create brush window.
![Using the Model Hitbox Calculator](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/9acc865cc1ea45e6d82aa0d1b884190f9087ca9f/assets/img/bsp-editor11.png?raw=true)
#### 2c. Create the destroyed bombzone A script_model
Choose a model for the destroyed version of the bomb site A objective. Typically the destroyed version of a model has the same name as the normal version with _d happened on it. For the most part the values are the same as before, however the target name must be exploder. Replace the same values as before but with the destroy xmodel instead of normal xmodel.
>Destroyed Script Model
```
{
    "classname" "script_model"
    "model" "[xmodel]"
    "origin" "[x] [y] [z]"
    "angles" "0 [yaw] 0"
    "script_exploder" "1"
    "targetname" "exploder"
}
```
If you just want the model to disappear after the bomb explodes, delete the model "key" "value" pair entirely. If you made a hitbox for the normal objective and want to delete the model or changing to a destroyed version with a different hitbox once destroyed make sure you add script_exploder 1 to the hitboxes. If needed, create additional hitboxes after creation 
#### 3. Add trigger_use for bomb site A plant area
Create a new brush for the bomb site A plant area. You can easily calculate by the trigger bounding box by inputing the X Y and Z coordinates of the objective as well as the X Y and Z coordinates of the 4 points into the Trigger Bound Box sheet on [Xmodel Hitbox Calculator](https://drive.google.com/file/d/1olftpo5OZqe6JGAYirQt7wSNUaktXm2I/view?usp=sharing). 
![Using the Trigger Bounding Box sheet on the Xmodel Hitbox Calculator](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/34f008d2d5e6f1e05c0f42cd873071785986b051/assets/img/bsp-editor05.png)
Copy the MAX X, MAX Y, and MAX Z values into the bounding box start field. Add at least 20 units to the MAX Z value. (add more if there are raised surfaces that the bomb could be planted on within the plant area). Copy the MIN X, MIN Y, and MIN Z values into the bounding box end field. The material name should be textures/common/trigger. Click Confirm.
![Create a brush for the trigger_use](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/d99bd57b259af868599ea3238facb25ffcbda0bf/assets/img/bsp-editor12.png?raw=true)
Find the script_brushmodel that was just created on the right side bar. Right click it and choose Rename and rename entity to trigger_use.
![Right Click and rename the script_brushmodel to trigger_use](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/34f008d2d5e6f1e05c0f42cd873071785986b051/assets/img/bsp-editor07.png?raw=true)
Make note of the model number of the trigger_use brush for the next step.
#### 4. Add trigger_multiple for bombzone A
Lastly we need to add a trigger_multiple for the bomb site. Copy the following and paste it into the List, Add entities by string window. Replace [x] [y] and [z] with the X Y and Z coordinates of the objective. Replace [trigger_use number} with the value of "model" from the trigger_use brush from the prior step and click Confirm.
>Trigger Multiple
```
{
    "classname" "trigger_multiple"
    "target" "sd_objective_a"
    "script_gameobjectname" "bombzone"
    "script_noteworthy" "1"
    "targetname" "bombzone_A"
    "origin" "[x] [y] [z]"
    "model" "*[trigger_use number]"
}
```
![Adding the bomb site A trigger multiple](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/a04a724c4ca3a34913e972e964e3e8540236bbc3/assets/img/bsp-editor08.png?raw=true)
#### 6. Repeat 3 - 5 again for bombzone B
Repeat the same steps, this time replacing your X Y and Z coordinates with the coordinates for your desired bomb site B location. Replace your script_noteworthy and script_exploder values with 2 instead of 1. And set your targets and targetnames for objective_b/bombzone_B. Use the following code blocks to help you copy-paste.

>Objective Script Model
```
{
    "classname" "script_model"
    "model" "[xmodel]"
    "origin" "[x] [y] [z]"
    "angles" "0 [yaw] 0"
    "script_exploder" "2"
    "targetname" "sd_objective_b"
}
```
>Destroyed Script Model
```
{
    "classname" "script_model"
    "model" "[xmodel]"
    "origin" "[x] [y] [z]"
    "angles" "0 [yaw] 0"
    "script_exploder" "2"
    "targetname" "exploder"
}
```
>Trigger Multiple
```
{
    "classname" "trigger_multiple"
    "target" "sd_objective_b"
    "script_gameobjectname" "bombzone"
    "script_noteworthy" "2"
    "targetname" "bombzone_B"
    "origin" "[x] [y] [z]"
    "model" "*[trigger_use number]"
}
```
#### 7. Add the trigger_lookat for bomb defusals.
The game uses this entity to determine if players are facing the bomb while defusing. It is required. Click List, Add entities by string in the toolbar and paste in the following and click confirm to add it. It doesn't matter which trigger_use model number you use. I typically use the one for bomb site A.
```
{
    "classname" "trigger_lookat"
    "targetname" "bombtrigger"
    "script_gameobjectname" "bombzone"
    "model" "*[trigger_use number]"
}
```
![Create the trigger_lookat using Add entities by string](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/a04a724c4ca3a34913e972e964e3e8540236bbc3/assets/img/bsp-editor01.png?raw=true)
#### A few more things before we move on
If you only want these added entities to show up in the Search and Destroy game mode, you need to add `"script_gameobjectname" "bombzone"` to **EVERY** entity added, otherwise they will show up in TDM/CTF etc.

#### 8. Adding spawns 
For the map to properly load, there needs to be spawn points for the player to load into. You need at least one mp_searchanddestroy_intermission and several mp_searchanddestroy_spawn_axis and mp_searchanddestroy_spawn_allied.

My preferred method is to duplicate existing mp_x_intermission from other game types name them mp_searchanddestroy_intermission. 
![Duplicating an existing intermission spawn](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/34f008d2d5e6f1e05c0f42cd873071785986b051/assets/img/bsp-editor09.png?raw=true)
For the axis and allied spawn, duplicating existing retrieval or uo spawns works just as well.
![Duplicating an existing UO spawn](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/34f008d2d5e6f1e05c0f42cd873071785986b051/assets/img/bsp-editor10.png?raw=true)
Spawns can added manually by finding where you would like spawn in and making note of the X Y and Z coordinates and yaw value using Position Debugger mod.

Add them by clicking List, Add entities by string and pasting however many you desire into the window, replacing [x] [y] and [z] and [yaw] with the X Y and Z coordinates and yaw value for the spawn.

>Allied Spawn
```
{
    "classname" "mp_searchanddestroy_spawn_allied"
    "origin" "[x] [y] [z]"
    "angles" "0 [yaw] 0"
}
```
>Axis Spawn
```
{
    "classname" "mp_searchanddestroy_spawn_axis"
    "origin" "[x] [y] [z]"
    "angles" "0 [yaw] 0"
}
```

Click File, Export and give your new bsp a name. Avoid using the same name as the source bsp or you will run into collision issues with servers who already have the map under the same name. 
## Step 3. Building your new Pak file
Find the corresponding .gsc file for your source map and the corresponding _fx.gsc if it exists. Rename these files so they have the same name as your edited bsp (eg. dsr_chateau.gsc and dsr_chateau_fx.gsc)

In your edited.gsc, add these lines if they aren’t already present. 
```
game[“attackers”] = “allies”;
game[“defenders”] = “axis”;
```
(If axis are the attackers on your map, swap the values) If there are any references to the _fx.gsc make sure you update the name there as well.

Create an arena file for your edited .bsp, name it the same name as your map (eg. dsr_chateau.arena). Arena files look like this.
```
{
   map “dsr_chateau”
   longname “DSR Chateau”
   gametype “sd”
}
```
Map is the exact name of the bsp. 
longname is the formatted name as it appears on the Call Vote and Loading screens. gametype is the game modes that the map is allowed to be called for a vote for.

With your .bsp, .gsc, and .arena files staged, it’s time to add them to your .pk3 file with your pak editor. The .bsp and .gsc files go in maps/mp/ directory, the .arena file goes in the mp/ directory under the .pk3 root. If you are editing a map with added models or textures make sure they are added in the appropriate directories as well. When editing custom maps, I find it easiest to make a copy of the .pk3 file and delete the old .bsp, .gsc, and .arena files and drag the new ones into the duplicate.
![example pk3 image](https://github.com/kmstrube81/CODUO-BSP-Editor-Tutorial/blob/9acc865cc1ea45e6d82aa0d1b884190f9087ca9f/assets/img/windows01.png?raw=true)
Once you have your .pk3 built, move it to your UO folder in your Call of Duty installation and test it. You should be able to plant and defuse as well as plant and have the bomb explode. If everything checks out then congrats! You have a new SD map to play!
