# HoI4Mod Loader: Equipment Conversion

A Hearts of Iron IV mod for [HoI4Mod-Loader](https://github.com/Builder-HD/HoI4Mod-Loader)'s
equipment conversion tools. The loader does nothing on its own: this mod switches its tools on.

With it, a production line's **Convert from Stockpile** also takes equipment of **earlier
generations** of the same equipment: Infantry Equipment 0 and I into II, older chassis, airframes
and hulls into newer ones. It works for every equipment type of every mod that defines `parent`,
with no `can_convert_from` entries to write. Optionally, **Shift+click** on a line's convert button
limits it to stockpiled designs of the same role (fighters only from fighters, CAS only from CAS),
and the tooltip lists, greyed, the designs it leaves out.

The base game's rule still applies: a country's own equipment converts only once it is
**decommissioned** (the production view's "Decommission"); captured equipment always.

## Game rules

An "Equipment Conversion" group on the game rules screen:

| Rule | Options (default first) |
|---|---|
| Convert Earlier Generations | allowed, blocked |
| Cost per Extra Generation | 75%, 100%, 90%, 50%, 25% |
| Conversions the Game Already Allows | game cost, factor applies |
| Same Role Only (Shift+Click) | allowed, blocked |
| Earlier Ship Hulls | blocked, allowed |

**Cost:** one generation back costs what the game charges; each further generation back
multiplies that by the factor (with 75%: two back 75%, three back 56%).

## How it works

At game start (`on_startup`) the scripted effect `hoi4_loader_conversion_apply_rules` sets the
loader's global flags from the rules: `hoi4_loader_conversion_generations`,
`hoi4_loader_conversion_role_mode`, `hoi4_loader_conversion_factor_<percent>`,
`hoi4_loader_conversion_existing_loader_cost` and `hoi4_loader_conversion_naval` (the loader's
MODDING.md, "Converting earlier generations"). The flags are saved with the game.

**Existing saves** (started without this mod), or switching during a game, from the console:

```
event hoi4_loader_equipment_conversion.1
```

opts in with the flags as the game rules say;

```
event hoi4_loader_equipment_conversion.2
```

opts out (every flag cleared; the loader removes its conversions again). Both events are hidden.

Without the loader the rules show and do nothing; saves load normally either way.

## Install

1. Install the loader (its README); it needs a loader build with `equipment_conversion`.
2. Copy `hoi4_loader_equipment_conversion.mod` into `Documents\Paradox Interactive\Hearts of Iron IV\mod\`,
   and copy this folder's `common\`, `events\`, `localisation\` and `descriptor.mod` into
   `...\mod\hoi4_loader_equipment_conversion\`.
3. Enable **HoI4Mod Loader: Equipment Conversion** in the launcher playset.

Checked with Hearts of Iron IV 1.19.4.2, single player. Not tested in multiplayer; there every
player needs the loader.
