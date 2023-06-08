# Noriel_Sylvire's Minerals Core
Version: 1.2

Copyright (c) 2020 Noriel_Sylvire (Flaviu E. Hongu)

Licenses:
Code: LGPL 2.1
Textures: CC-BY-SA, CC-BY

Read license.txt for more information.

---

This mod has a hard dependency on ns_minerals_core and nslib, and optional dependencies on all the other mods from this modpack to ensure a correct load order.
If this mod wasn't set up in this way, your mods may load before the mods in this modpack, which would cause them not to work.

Before this mod was added, nsmc probably only worked with mods that came after it, alphabetically. I'm really sorry for not noticing before!

Technically, the name of this mod is ns_minerals, simply to make it easier to use. Because of this name, one might think this is the main mod in the modpack but actually it isn't. This mod is the last one to load
because it depends on all the other mods from the modpack. Then you add this mod and only this mod as a dependency and that causes the entire modpack to load before your mods, so that you can actually use the
modpack!

This mod is also referred to as the afterloader, because it loads after the rest of the modpack, and fixes the load order.

---
## Changelog

* 1.3 - Created the afterloader to fix load order problems

---
## API Documentation

In order to use the API, you must create a mod that depends on `ns_minerals_core`. Then you must write the definition of an array of `mineral`, and call the `nsmc.register_minerals()` method.

---
### Methods

All of the methods implemented by this API are contained within a table named `nsmc` so that you don't have to type in the full name of the mod, which is `ns_minerals_core` every time you want to call a method.

Example of how to call a method:
```lua
nsmc.register_minerals("mymod", {{}})
```
The above code is functional and will result in one mineral named `"mymod1mineral"` added to the game, complete with tools, nodes, ore generation and textures. The textures will all be white.
All the methods contained in this mod, when given a blank mineral with no values, will default everything to the same values or `default` mod iron. So, the same scarcity, depths, uses, damage, etcetera, as iron.

---
### nsmc.register_crafts(modname, mineral)
Parameters:
* modname: a `string` value. This must be the name of your mod.
* mineral: a `mineral` table. This must be a `mineral` table containing everything needed to register your mineral. See MINERAL TABLE for more information.

This method registers all the crafting recipes that other minerals from the `default` mod have.
Examples: the recipes for the axe, sword, pickaxe, block...

---
### nsmc.register_craftitems(modname, mineral)
Parameters:
* modname: a `string` value. This must be the name of your mod.
* mineral: a `mineral` table. This must be a `mineral` table containing everything needed to register your mineral. See MINERAL TABLE for more information.

This method registers all the items that other `default` mod minerals have.
Currently only registers the lump and ingot if it is a metal, or the gem if it is a crystal.

---
### nsmc.register_tools(modname, mineral)
Parameters:
* modname: a `string` value. This must be the name of your mod.
* mineral: a `mineral` table. This must be a `mineral` table containing everything needed to register your mineral. See MINERAL TABLE for more information.

This method registers all the tools other minerals from the `default` mod have.
Examples: the actual axe, pickaxe, sword, shovel...

---
### nsmc.register_oregen(modname, mineral)
Parameters:
* modname: a `string` value. This must be the name of your mod.
* mineral: a `mineral` table. This must be a `mineral` table containing everything needed to register your mineral. See MINERAL TABLE for more information.

This method registers all the ores of a mineral, with their depths, scarcities, etc.

---
### nsmc.register_nodes(modname, mineral)
Parameters:
* modname: a `string` value. This must be the name of your mod.
* mineral: a `mineral` table. This must be a `mineral` table containing everything needed to register your mineral. See MINERAL TABLE for more information.

This method registers all the nodes a mineral from `default` mod has.
For now, this means the ore node and the block resulting from combining 9 of the item in a crafting grid.


---
### Tables and Data Structures

---
### mineral Table

This table can contain all of the data needed to register everything pertaining to a `default` mineral, such as tools, nodes, ores, crafting recipes, etcetera.

All the members can be given a value or left out of the definition. If left out, the default value will be assigned.
Note: leaving a member out of the definition and giving it an empty value are not the same thing.
Example: `name = ""` is not leaving the `name` member out of the definition but giving it the `""` value. Leaving it out means not writing it at all.

The table can be left empty (as in `{}`), in which case it will be filled with the default values of all members.
If this is done, the game will notify you that no name was given, and give it a very ugly name instead, so, please, at least give the minerals a name! If you also give them a color you'll already get a very convincing result.
All the default values are the same as iron.

---
### mineral Table Members

All the different pairs of keys and values in this table are called "members" in this documentation.
Each member's section title contains the name of the member and the value type separated by a coma.
Example: `type`, `string`
The above example means the member is called `type`, and it takes on `string` type values.
Then, after the section title, there will be the default value.
After that, a detailed explanation of the member.

You will often find examples too.

And lastly, if any, there will be the special values the member can take, also explained in detail.

---
### mineral_type, string
Default: `"metal"`

This can be either `"metal"` or `"gem"`.
If it is a metal, the mineral will be registered with a lump and an ingot. If it is a gem, it won't have an ingot or a lump, instead it will have a gem type `craftitem`.
If the type is neither `"metal"` nor `"gem"`, or there is a typo, it will be treated as if it was a metal, which is the default value.

---
### flammable, bool
Default: `false`

Enable this if you want your mineral to be useable as a fuel in furnaces.
Note: normally, if it is a metallic mineral, only the lump can be used as a fuel while the ingot loses the "flammable" property, as if by smelting it, it gets consumed. If it is a gem, the gem can be used as fuel.
Yes, I am aware that it doesn't make sense, since, by combining nine ingots you get one block and you can use the block as fuel again, as if the ingots somehow regained their flammable property by simply being next to one another. We can discuss this on the forum, if you wish, and come up with something better.

---
### burntime, int
Default: `1` for flammable minerals, `0` otherwise.

The amount of time, in seconds, that the mineral lasts as a fuel source in furnaces.
Since the default for flammable materials is `1`, please change this if you want your mineral to be of any use in the furnace. One second is too little I would advise between 10 to 15 seconds for a normal fuel. More for a good fuel, less for a bad fuel.

Example: A fuel that lasts 30 seconds in the furnace is a good fuel. A fuel that lasts 120 seconds in the furnace is an insanely OP fuel source. Balance tends to be a good thing!

---
### block_burntime, int
Default: `9` times the `burntime` + `5`

The amount of time, in seconds, that the block made out of nine ingots or gems lasts as a fuel source in furnaces.
Typically nine times more than the item itself, plus a little bonus to make crafting this worthwhile.

Note: if your fuel has a very large duration, like for example `burntime = 30`, I would advise you gave this variable a value yourself, and not just leave it blank. If you leave it blank, the only beneffit for crafting the block and using it as a fuel source would be an extra 5 seconds of burntime. In this example, nine times the `burntime` of 30, that means using nine gems yields 270 seconds of time, while using the block yields 275 seconds. In this case the difference is tiny compared to just using the gems, so I would personally give it a value of like 300 or something, manually, to make crafting the block a little more appealing.

---
### ctime, int
Default: `6`

Time, in seconds, it takes for the metal lump to be cooked into the metal ingot.
Only works on metal type minerals.

---
### material_texture_index, int
Default: `2`

This is the number at the end of `material_texture` when not using custom textures.
In the current version can be either `1` or `2` for metals and `1`, `2`, or `3` for gems.
Each number corresponds to the three brightness variations of the `"...ingot_1"`, `"...ingot_2"`, `"...gem_1"`, `"...gem_2"`, and `"...gem_3"` textures found in the `\textures` folder.

As a result of this value, if the `material_texture_index` is `2`, and this is a metal type mineral with bright textures, it's `material_texture` will be `"bright_ingot_2"`.
This is, unless you provide your own material texture. If you do, this number will not have any effect at all. The `material_texture_index` is only used when you did not provide your own textures.

Let me explain this in a different way, just in case: if you browse the `\textures` folder, you'll find there are two different ingot textures, and three different gem textures. This number allows you to choose which one to use.

---
### lump_texture_index, int
Default: `1`

Same as the previous, only this one is the number at the end of `lump_texture`, it only gets used by metal type minerals, and can be either `1` or `2`.

---
### lump/material + _colorize, bool
Default: `true`

If enabled the lump, gem or ingot textures will be colorized.
It is enabled by default so leave this out if you want everything to be colorized.

---
### material_texture, string
Default: `texture_brightness` + `" _ingot_"`/`"_gem_"` + `material_texture_index`

The texture used for the ingot `craftitem` of metal type minerals, and the gem for gem type minerals.
This is called `material_texture` because it cannot be named `ingot_texture` or `gem_texture` as some minerals are metals while other are gems, so it has a name that is correct for both types of minerals.
If `material_texture_index` has no value, `material_texture` will have a `"_2"` at the end, because `2` is the default value of `material_texture_index`.

---
### lump_texture, string
Default: `texture_brightness` + `" _lump_1"`

Same as `material_texture` only this one is for lumps instead of ingots or gems, and is only used by metal type minerals.
Also comes in three brightness variants, and two different styles; `1` and `2`. Same as above, if no `material_texture_index` is given, `1` will be used.

---
### ore/block, node table

A `node` table that contains the `colorize`, `tiles`, and `groups` of the mineral's block or ore. If it's the ore it also has a `wherein_texture`, which is the texture of the node the ore appears in.
Each of these sub-members can be either left empty or filled by the user, and each is explained in their own sub sections below.

---
### colorize, bool
Default: `true`

This decides whether texture will be colorized or not. If this is an ore, only the overlayed texture is colorized, not the wherein texture.

---
### tiles, array of string

This is an array of string (which means it contains a number of string values separated by comas).
Example: `{ "my_ore_texture" }` this array only contains one texture, which means that one texture will be applied to all six sides/faces of the node
Example2: `{ "my_ore_texture", "my_other_ore_texture", "my_ore_texture", "my_other_ore_texture". "my_ore_texture", "my_other_ore_texture" }` in this case, since there are six values, half of the faces of the node will get one texture, and the other half will get the other texture.

---
### wherein_texture, string
Default: `"default_stone.png"`

This should be the full name of the texture of the node your ore generates in, with the `".png"` at the end.
Only ores use this. Leave this out of the definition if you want the ore to generate in stone and you don't want to do weird things with the textures.

Note: since this texture is drawn beneath the tiles of the ore, you can use this to your advantage to create interesting effects, even if the ore appears in stone. For example, you can make an ore with six different textures, one on each side, and then a special texture you created specifically to be seen from beneath the ore, in a way that, idk, combines in interesting ways with the ore's tiles. Just an idea.

---
### groups, groups table
Default: `{ cracky = 2 }`

A table containing pairs of node groups and integer values, separated by comas. See [node groups](https://github.com/minetest/minetest/blob/dafdb3edb4b65db144d72cd2274a657af671bdd1/doc/lua_api.txt#L1945) for more information.

Example: `{ cracky = 1, choppy = 2, snappy = 3 }`

---
### toolname, tool table
Default: Same values as all the default iron tools

`toolname` can be one of:
* `axe`
* `sword`
* `pick`
* `shovel`
* `hoe`

This table can contain a `colorize`, `texture_handle`, `texture_head`/`texture_blade`, `full_punch_interval`, `times`, `uses`, `maxlevel`, and a `damage` sub-member. If one or more of these is left out, they will take the default values (same values as the iron tools).

---
### colorize, bool
Default: `true`

Whether or not the head/blade texture of the tool will be colorized. I sugest you don't touch this unless you give your own, already colorized texture.

---
### texture_handle, string
Default: `texture_brightness` + `"_"` + tool type + `"handle"`

This is the texture used as the tool's handle.

---
### texture_head/texture_head, string
Default: `texture_brightness` + `"_"` + tool type + `"head"`/`"blade"`

This texture will be overlayed on top of the handle. It normally is just the head or blade of the tool.

---
### full_punch_interval, float
Default: Same as the equivalent iron tool

Time, in seconds, the player has to wait between clicks to deal full damage. If, instead, the player spam-clicks the tool faster than the `full_punch_interval`, the damage dealt will be reduced.

---
### times, times dictionary
Default: Same as the equivalent iron tool

Pairs of key-value, where the key must be in the form of `[some_integer]`, where `some_integer` is an `s16` integer, and value is a `float` number that represents the amount of time, in seconds, it takes for the tool to break the `[key]` tier of nodes.
Example: `{ [5] = 3.00, [6] = 4.00 }`
In this example, the tool can only break tier `5` and tier `6` nodes, taking `3` and `4` seconds respectively.

IMPORTANT: Pay attention, this dictionary has confusing ordering due to how Minetest registered their tiers of items.
Diamonds are Tier `1`, that is, TIER ONE.
While wooden tools are tier `3`, that is, TIER THREE.
Indeed, for some reason, tier `3` means weaker than tier `2` in minetest's code, and tier `2` is also weaker than tier `1` in minetest's code.

However if you want to add a new tier of minerals, stronger than diamond, I suggest that instead of following the illogical downward trend you add that stronger-than-diamond ore as a tier `4` ore, and then stronger minerals will be tier `5`, `6` and so on.
See [my Guide on how to understand tool and node parameters and how to add new tiers of ore](https://forum.minetest.net/viewtopic.php?t=27811) for a more detailed explanation.


---
### uses, int
Default: Same as the equivalent iron tool

Number of uses the tool has.
Remember that a tool loses exponentially less uses the greater the difference between the tool's `maxlevel` and the node's tier is. See [my Guide on how to understand tool and node parameters and how to add new tiers of ore](https://forum.minetest.net/viewtopic.php?t=27811) for a more detailed explanation.


---
### maxlevel, int
Default: Same as the equivalent iron tool

Maximum level of node the tool can break. It is also used to calculate how many uses the tool loses by breaking a node.

---
### damage, int
Default: Same as the equivalent iron tool

Amount of damage the tool deals.

---
### oregen, oregen table
Default: Same values as the ore generation parameters of iron in the default mod

Can contain a `wherein_node` member and a `scarcity`, `num_ores`, `size`, `max`, and `min` array.
If the arrays contain more than one value each, the API will register as many ores as the number of elements in the arrays.

Example:
If the arrays have two values each, like in the default values they are given, the API will register two ores using the first value in each array first, then the second value in each array.

---
### wherein_node, string
Default: `"defaul:stone"`

The node in which the ore will be generated.

---
### scarcity, array of int
Default: `{ 7, 24 }`

This is an array of integer numbers. The result of raising each number to the third power is equal to `1 / chance` where `chance` is the chance of the node being converted to the ore if it is the same as `wherein_node`.
That is to say, larger numbers means lower chance, smaller numbers means greater chance of the ore spawning.

---
### num_ores, array of int
Default: `{ 5, 27 }`

The number of ores that will spawn in a cluster of ores when it is generated.
Greater number means more ores per cluster. Cannot be larger than `size` raised to the third power.

---
### size, array of int
Default: `{ 3, 6 }`

The length of each edge of the cube that contains each cluster of ores.
A larger number means the ore will be more spread out, but it will also allow for greater values of `num_ores`.

---
### max, array of int
Default: `{ 0, -64 }`

An array with the highest Y-coordinates the ore can be generated in.

---
### min, array of int
Default: `{ -31000, -31000 }`

An array with the lowest Y-coordinates the ore can be generated in.

---

Lastly, there will be support for a new feature called `super_mining_layer` soon.
The super mining layer is a configurable layer in which all the minerals will be able to spawn with custom parameters for anyone who doesn't like their ores to be generated in exclusive layers.
More on this topic soon.
