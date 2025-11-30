# Survivalist Trait and Gene

A RimWorld 1.6 mod that adds a rare Survivalist trait and gene, making pawns completely indifferent to food quality and living conditions. Perfect for underground bases, harsh environments, or pawns who just don't care about creature comforts.

## Overview

This mod adds three ways to make pawns true survivalists:
- **Survivalist Trait** (rare, 0.2 commonality) - appears naturally in character generation
- **Survivalist Gene** - can be added via gene editing, zero complexity and metabolic cost
- **SurvivalistFood Ideology Precept** - can be added to any ideology to make all followers indifferent to food quality

The Survivalist gene automatically grants the Survivalist trait to pawns who have it, ensuring identical effects including proper food selection. If both trait and gene are present, both are active (same effects, no stacking issues). The ideology precept works alongside them, nullifying food-related thoughts for all ideology followers.

## Features

### Food Indifference
Pawns with Survivalist trait/gene are completely indifferent to food quality:
- **No positive mood** from lavish meals (+12 normally)
- **No positive mood** from fine meals (+5 normally)
- **No negative mood** from raw food (-7 normally)
- **No negative mood** from kibble (-12 normally)
- **No negative mood** from rotten food (-10 normally)
- **No negative mood** from insect meat (-6/-3 normally)
- **No negative mood** from cannibalism (-20/-15 normally)
- **No negative mood** from eating without a table (-3 normally)
- **No negative mood** from being soaking wet (-3 normally) - rain doesn't bother survivalists

### Living Conditions
- **Outdoor need completely disabled** - pawns never need to go outside
- **No "cooped up" debuff** - can stay indoors indefinitely
- **No "cabin fever" debuff** - perfect for underground bases
- **No penalties for being indoors** - unlike Undergrounder trait which penalizes being outdoors

### Recreation
- **Recreation need decays 50% slower** - JoyFallRateFactor set to 0.5
- Less time needed for recreation activities
- More time for productive work

## Technical Details

### Trait Definition
- **defName**: `Survivalist`
- **Commonality**: 0.2 (rare)
- **Conflicts**: Undergrounder trait (mutual exclusion)
- **Effects**:
  - Disables `Outdoors` need
  - Blocks all food-related thoughts via `disallowedThoughtsFromIngestion`
  - Recreation fall rate: 0.5x (50% slower)

### Gene Definition
- **defName**: `Survivalist`
- **Complexity**: 0 (zero)
- **Metabolic Effect**: 0 (zero)
- **Display Category**: Miscellaneous
- **Effects**:
  - Automatically grants the Survivalist trait (via `forcedTraits`)
  - Suppresses Undergrounder trait if present
  - All other effects come from the trait (ensures food selection works properly)

### Ideology Precept
- **defName**: `SurvivalistFood_Indifferent`
- **Issue**: `SurvivalistFood`
- **Label**: "indifferent"
- **Description**: "We are true survivalists. Food is fuel, nothing more. We don't care about quality, taste, or source - paste, raw meat, insect flesh, even human meat. As long as it sustains us, it's acceptable."
- **No meme requirements** - can be added to any ideology
- **Nullifies all food-related thoughts** including those from other precepts (e.g., "nutrient paste disgusting", "meat abhorrent", "cannibalism abhorrent")

### Thought Patches
The mod patches the following thought definitions to add `nullifyingTraits`, `nullifyingGenes`, and `nullifyingPrecepts`:
- `AteLavishMeal`
- `AteFineMeal`
- `AteRawFood`
- `AteKibble`
- `AteCorpse`
- `AteRottenFood`
- `AteInsectMeatDirect`
- `AteInsectMeatAsIngredient`
- `AteHumanlikeMeatDirect`
- `AteHumanlikeMeatAsIngredient`
- `AteWithoutTable`
- `AteNutrientPasteMeal` (ideology precept thought)
- `SoakingWet` (weather thought from rain)

### Ideology Precept Thought Patches
The mod also patches ideology precept thoughts to be nullified by the SurvivalistFood_Indifferent precept:
- `AteNutrientPasteMeal` (from "nutrient paste disgusting" precept)
- `AteMeat_Abhorrent`, `AteMeat_Horrible`, `AteMeat_Disapproved` (from vegetarian ideologies)
- `AteHumanMeat_Abhorrent`, `AteHumanMeat_Horrible`, `AteHumanMeat_Disapproved` (from cannibalism precepts)

## Compatibility

### Conflicts
- **Undergrounder trait**: Mutual exclusion (both disable outdoor need, so they conflict)
- **CaveDweller gene** (indoor dweller): Core game automatically handles conflicts when both genes disable the same need

### How It Works
- **Gene grants trait**: The Survivalist gene uses `forcedTraits` to automatically grant the Survivalist trait
- **Single source of truth**: All effects (food thoughts, outdoor need, recreation) are defined in the trait
- **Both active**: If both trait and gene are present, both are active (same effects, no stacking issues)
- **Undergrounder suppression**: If Undergrounder trait is present with Survivalist gene → gene suppresses Undergrounder trait
- **CaveDweller conflicts**: Core game handles CaveDweller gene conflicts automatically

### Safe to Use
- ✅ Safe to add/remove mid-game
- ✅ XML-only implementation (no C# code)
- ✅ Zero performance impact
- ✅ No save game issues

## Use Cases

### Underground Bases
Perfect for pawns living in mountain bases or underground complexes. No need to worry about outdoor time or cabin fever.

### Harsh Environments
In extreme biomes where food quality is hard to maintain, Survivalist pawns don't care if they're eating raw meat or paste.

### Prisoners
Easier to manage prisoners who don't care about food quality or eating without tables.

### Caravans
Less recreation time needed means more time for travel and work.

### Low-Maintenance Pawns
Any pawn you want to be self-sufficient and require minimal attention to keep happy.

### Ideology-Based Survivalism
Add the "Survivalist food: Indifferent" precept to your ideology to make all followers practical about food without requiring specific memes or traits.

## Ideology Support

### Adding the Precept to Your Ideology

1. Open the ideology editor (in-game or during setup)
2. Navigate to precepts
3. Find "Survivalist food" issue
4. Select "Indifferent" precept
5. All ideology followers will now be indifferent to food quality

### Benefits

- **No meme requirements** - works with any ideology setup
- **Nullifies other food precepts** - if your ideology has "nutrient paste disgusting" or "meat abhorrent", adding this precept will nullify those negative thoughts
- **Perfect for practical ideologies** - allows you to be practical about food without requiring specific memes like Transhumanist
- **Works alongside trait/gene** - if a pawn has the Survivalist trait or gene AND the ideology has this precept, all food thoughts are nullified

## Requirements

- RimWorld 1.6
- Biotech DLC (required for gene functionality, though trait works without it)
- Ideology DLC (required for precept functionality, though trait/gene work without it)

## Installation

1. Download the mod
2. Place in `RimWorld/Mods/` folder
3. Enable in RimWorld mod list
4. Load order: Should load after Biotech (automatically handled)

## Performance

- **Zero performance impact** - pure XML implementation
- **No runtime overhead** - all effects handled by core game systems
- **No memory allocation** - static definitions only

## License

This mod is provided as-is for use with RimWorld.

## Credits

- Created by celphcs30
- GitHub: https://github.com/celphcs30/Survivalist-Trait-and-Gene

