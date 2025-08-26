## Custom dynamic stormgate triggers

> Written by a user named thanks. If you want to include these triggers in your own custom map, I would appreciate if you credited me when you publish it for sharing.

Works for:
- Up to 5 stormgates inclusive (stormgates will not start if you have >5)
- Up to 8 teams (the current maximum allowed in Stormgate as of the Necrolyte patch)
- Arbitrary numbers of players

Stormgate reward mechanics work by assigning 1 reward to a player on the winning team that has done the most damage, as per the standard stormgate capture point logic in the Necrolyte patch.

Handicap modifiers for vision radius and damage bonuses are handled relative to the winning team.
Previously it was just a comparison between team 1 and team 2, but now I keep track of the game's max claimed stormgate count and calculate all vision radius and damage buffs relative to this.

### Usage Guide

To use less than 5 stormgates it may be fine to simply not include the objects in preplaced_named_objects.
However, I have typically removed all references, i.e.:
- Deleting StormgateCP# and StormgateSubmerged# in preplaced_named_objects.json
- Deleting any references to the StormgateCP# and StormgateSubmerged# in managed_archetypes.json (likely optional)

#### Steps
- Assign placed names of StormgateCP# (where # is an integer 1-5) to tug of war objects
  - Tug Of War IDs are typically labelled 0-4 but StormgateCP# placed names must be 1-5.
  - This is handled inside managed_archetypes.json
- Assign placed names of StormgateSubmerged# (where # is an integer 1-5) to stormgate objects
  - Stormgate IDs are typically labelled 0-4 but StormgateSubmerged# placed names must be 1-5.
  - This is handled inside managed_archetypes.json
    - An example managed_archetypes.json with nothing but 5 stormgates has been included.
- Place up to 5 instances of StormgateSubmerged# and StormgateCP# on the map
  - This is in preplaced_named_objects.json and they should be owned by Player 31 (Neutral Hostile).
  - Ensure the position arrays (x, y) match for pairs of capture points and stormgate submerged structures.
  - An example preplaced_named_objects.json with 5 pairs of objects placed has been included. Feel free to move these around the map and/or change GUIDs.
- Copy this folder of Stormgates/ into catalog/archetypes/ so that we have catalog/archetypes/Stormgates/
- Ensure the "Init_StormgateObjectives" trigger is run as a "Trigger_Run" call in catalog/archetypes/MeleeInitialization.json
- Ensure the correct time of day is specified in catalog/archetypes/InstanceMapSettings.json
  - The time of day should also be edited so that night time triggers at the same time as the first stormgate spawn at ~194.0seconds and then is in sync with subsequent stormgates from then on.

#### Tips

If you want to debug a little easier, here are a few things you can do:
- Set "is_enabled": true for UI_SendChatMessage in Init_StormgateObjectives.json.
    - There's a few function calls here that print out the number of stormgates the triggers find on your map. - They are set to is_enabled: false by default, but you can set them to true if you want to print out the number of stormagates found. This may be useful if you cannot work out why things are running.
- Edit the initial "194.0" second spawn timer in "Init_StormgateObjectives.json".
    - I usually set it to about "34.0" seconds so that I can play Vanguard, spawn 3 servos, overcharge rush the middle to make a barracks and sentry post.
    - This lets me get a fast iteration time on checking if a stormgate spawns, tracks damage properly, reveals the right vision, and assign the reward and spend less time waiting around.

### Disclaimer

- I make no guarantees that any of this works, especially not when patches are prone to introduce breaking changes.
- Regarding bugs: its best to look out for things like:
  - Are all the announcers playing correctly?
  - Does every player have a revealer of each stormgate?
  - Is the vision radius of the revealer for each player scaling relative to the winning team?
  - Is the damage buff modifier for each player scaling relative to the winning team?
