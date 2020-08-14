# PSION MACROS
This is a collection of macros to make it easier to play a Psion using Roll20's 5E OGL character sheet. No API scripts necessary. These rely heavily on Roll20's Roll Query feature, and will throw up prompts to select how you're using  your powers.

This Psion class is by [KibblesTasty](https://www.kthomebrew.com/) and is available [here](https://www.gmbinder.com/share/-LZSNMgmChWNGW979hrj).

# USAGE
1. Grab the ability you want and add it to your character sheet as a custom ability.
2. Set the following attributes on the character sheet (they'll all scale, so you can leave them alone unless you take a talent that modifies them):
* `psionic_power_save_desc`: This should be `No damage or effects.` If you have Potent Psionics, instead enter `Half damage and no effects.` You'll need this for any ability with a saving throw (though the math will still be fine - it's just for descriptions).
* `empowered_psionics_bonus`: This should be `0`. If you have Empowered Psionics, instead enter `@{intelligence_mod}`. You'll need this one for most abilities.
* `perfected_enhancement_bonus`: This should be `0`. If you have Perfected Enhancement, instead enter `floor(@{intelligence_mod}/2)`. You only need this one for Enhancing Surge.

# LIMITATIONS
* There's no way to edit attributes without API access, so these abilities won't directly modify any attributes on your character.
* There's no way to dynamically switch between attack rolls and saving throws, so for powers that do so, you'll need a separate macro for each mode.
* It really sucks that roll20 demands these be one-line snippets; if you edit them, I suggest adding line breaks every 3-4 items.
* I haven't found any way to include one's Psi Limit as part of the macros. So right now I've just got the roll queries for a 20th-level PC - ideally, on all the 1+ queries, we'd cap them at the psi limit. Ideally they'd be capped at the maximum allowable for any level.
* It's probably impossible to teach roll20 to actually enforce a psi limit within a turn, or even within a macro (it could be maybe done with API access, but I'm trying not to require that). This isn't that hard for people to track themselves, and teaching roll20 how to sum different values across the roll queries will be a giant pain, if it's possible at all.

# SHOW ME THE DANG MACROS ALREADY
These all use the OGL sheet's default roll templates, to try and keep as consistent an appearance as possible. Again, they rely on a few custom attributes you'll need to set, if you skipped 'USAGE' above.

## Telekinetic Force
```
@{wtype} &{template:atkdmg} {{charname=@{charname_output}}} {{rname=Telekinetic Force}} {{damage=1}} {{dmg1flag=1}} {{range=60ft}} {{dmg1=[[[[(1+?{Hammering (1+)|0|1|2|3|4|5|6|7|8|9|10})]]d10  + @{empowered_psionics_bonus}[INT]]]}} {{dmg1type=Bludgeoning}} {{save=1}} {{saveattr=Strength}} {{savedesc=@{psionic_power_save_desc}}} {{savedc=[[[[(@{spell_save_dc})]][SAVE]]]}} {{desc=?{Zone of (0-3)?|0,Target is|1, All creatures in a **5'** sphere are|2, All creatures in a **10'** sphere are|3, All creatures in a **20'** sphere are} shoved [[5+?{Hurling (1-3)|0|1|2|3}*10]] feet or knocked prone. ?{Crushing (2)|No,|Yes,&#x00A;**Crushing:** The target is restrained until the end of its next turn.}}} @{charname_output}
```

## Telekinetic Weapons
Features a built-in option if you've previously activated Psionic Weapons, if you're using them together; you can just delete those four lines if you're not. As with Eldritch Blast, if you're flinging multiple weapons, you're going to want to just hit this button multiple times. It'd be even harder than EB to script up multiple flings, since the damage dice can vary between attacks.
```
@{wtype} &{template:atkdmg} {{charname=@{charname_output}}} {{rname=Telekinetic Weapon}} {{damage=1}} {{attack=1}} {{always=1}} {{range=30ft}} {{mod=@{spell_attack_bonus}}} {{r1=[[1d20+@{spell_attack_bonus}]]}} {{r2=[[1d20+@{spell_attack_bonus}]]}} {{dmg1flag=1}} {{dmg1=[[?{Damage Dice} + @{empowered_psionics_bonus}[INT]]]}} {{dmg1type=?{Damage Type|Slashing|Piercing|Bludgeoning}}} {{crit1=1}} {{crit1=[[?{Damage Dice}[CRIT]]]}} ?{Psionic Weapon|No, |Yes, {{dmg2flag=1&#125;&#125; {{dmg2=[[[[(ceil((@{level} + 2) / 6))]]d6]]&#125;&#125; {{dmg2type=Psionic Weapon&#125;&#125; {{crit2=1&#125;&#125; {{crit2=[[[[(ceil((@{level} + 2) / 6))]]d6[CRIT]]]&#125;&#125; } @{charname_output}
```

## Telepathic Intrusion
```
@{wtype} &{template:atkdmg} {{charname=@{charname_output}}} {{rname=Telepathic Intrusion}} {{damage=1}} {{dmg1flag=1}} {{range=60ft}} {{dmg1=[[[[(1+?{Rending (1+)|0|1|2|3|4|5|6|7|8|9|10})]]d8 + @{empowered_psionics_bonus}[INT]]]}} {{dmg1type=Psychic}} {{save=1}} {{saveattr=Wisdom}} {{savedesc=@{psionic_power_save_desc}}} {{savedc=[[[[(@{spell_save_dc})]][SAVE]]]}} {{desc=The target has disadvantage on attacks against you until the start of your next turn. ?{Terrifying (1)|No, |Yes,&#x00A;**Terrifying (1):** The target is frightened until the end of its next turn&period;} ?{Overwhelming (2)|No, |Yes,&#x00A;**Overwhelming (2):** The target is *staggered* until the end of its next turn&period; A staggered creature's movement speed is halved&comma; it cannot take reactions&comma; and it has disadvantage on Dexterity saving throws and Constitution saving throws to maintain concentration on a spell.} ?{Reading (2)|No, |Yes,&#x00A;**Reading (2):** The target's next attack against you before the start of your next turn has disadvantage.}}} @{charname_output}
```

## Enhancing Surge
```
@{wtype} &{template:dmg} {{charname=@{charname_output}}} {{rname=Enhancing Surge}} {{damage=1}} {{dmg1flag=1}} {{dmg2flag=1}} {{range=60ft}} {{dmg1=[[1d4+?{Fortifying (1+)|0|1|2|3|4|5|6||7|8|9|10}d6 + [[@{perfected_enhancement_bonus}]][INT/2]]]}} {{dmg1type=Temp HP}} {{dmg2=1d4+?{Savage (1+)|0|1|2|3|4|5|6|7|8|9|10}d6}} {{dmg2type=Bonus Damage}} {{desc=The target gains temp HP and deals bonus damage to one application of their next damage roll.?{Swift (2)|No, |Yes,&#x00A;**Swift (2):** The target gains 30ft of movement speed&period;} ?{Resilient (3)|No, |Yes,&#x00A;**Resilient (3):** The target gains resistance to all damage until the start of your next turn.}}} @{charname_output}
```

## Enhancing Surge
For now, I've got this just outputting what the bonus damage is (game-wise, you probably don't want to preroll it). It's easy to preroll it; I think I know how to set it to be rollable on a click, and may yet do so.
```
@{wtype} &{template:dmg} {{charname=@{charname_output}}} {{rname=Enhancing Surge}} {{damage=1}} {{dmg1flag=1}} {{dmg2flag=1}} {{range=60ft}} {{dmg1=[[1d4+?{Fortifying (1+)|0|1|2|3|4}d6 + [[@{perfected_enhancement_bonus}]][INT/2]]]}} {{dmg1type=Temp HP}} {{dmg2=1d4+?{Savage (1+)|0|1|2|3|4}d6}} {{dmg2type=Bonus Damage}} {{desc=The target gains temp HP and deals bonus damage to one application of their next damage roll.?{Swift (2)|No, |Yes,&#x00A;**Swift (2):** The target gains 30ft of movement speed&period;} ?{Resilient (3)|No, |Yes,&#x00A;**Resilient (3):** The target gains resistance to all damage until the start of your next turn.}}} @{charname_output}
```

## Psionic Weapons
I mostly use it through the built-in option on TK Weapons, but this will quickly roll damage for you as needed.
```
@{wtype} &{template:dmg} {{charname=@{charname_output}}} {{rname=Psionic Weapon}} {{damage=1}} {{dmg1flag=1}} {{range=60ft}} {{dmg1=[[[[(ceil((@{level} + 2) / 6))]]d6]]}} {{dmg1type=Psychic}} @{charname_output}
```

## Rampage (with no die management)
Just rolls your rampage damage, it's up to you to track the die.
```
@{wtype} &{template:dmg} {{charname=@{charname_output}}} {{damage=1}} {{dmg1flag=1}} {{rname=Rampage (?{Rampage Die|d4|d6|d8|d10|d12})}} {{dmg1=[[1?{Rampage Die|d4|d6|d8|d10|d12}]]}} @{charname_output}
```
