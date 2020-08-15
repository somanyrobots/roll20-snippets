# PSION MACROS
This is a collection of macros to make it easier to play a Psion using Roll20's 5E OGL character sheet. No API scripts necessary. These rely heavily on Roll20's Roll Query feature, and will throw up prompts to select how you're using  your powers.

This Psion class is by [KibblesTasty](https://www.kthomebrew.com/) and is available [here](https://www.gmbinder.com/share/-LZSNMgmChWNGW979hrj).

# USAGE
1. Grab the ability you want and add it to your character sheet as a custom ability.
2. Set the following attributes on the character sheet (they'll all scale, so you can leave them alone unless you take a talent that modifies them):
* `psionic_power_save_desc`: This should be `No damage or effects.` If you have the Potent Psionics talent, instead enter `Half damage and no effects.` You'll need this for any ability with a saving throw (though the math will still work fine - it's just for descriptions).
* `empowered_psionics_bonus`: This should be `0`. If you have the Empowered Psionics feature, instead enter `@{intelligence_mod}`. You'll need this one for most abilities.
* `perfected_enhancement_bonus`: This should be `0`. If you have the Perfected Enhancement feature, instead enter `floor(@{intelligence_mod}/2)`.
* `astral_construct_die`: This should be `d8`. If you have the Devastating Weapon feature (part of the Shaper subclass), instead enter `d12`.
* `empowered_construct_bonus`: This should be `0`. If you have the Empowered Construct feature, instead enter `@{intelligence_mod}`.

# LIMITATIONS
* There's no way to edit attributes without API access, so these abilities won't directly modify any attributes on your character.
* There's no way to dynamically switch between attack rolls and saving throws, so for powers that do so, you'll need a separate macro for each mode.
* It really sucks that roll20 demands these be one-line snippets; if you edit them, I suggest adding line breaks every 3-4 items.
* I haven't found any way to include one's Psi Limit as part of the macros. So right now I've just got the roll queries for a 20th-level PC - ideally, on all the 1+ queries, we'd cap them at the psi limit. Ideally they'd be capped at the maximum allowable for any level.
* It's probably impossible to teach roll20 to actually enforce a psi limit within a turn, or even within a macro (it could be maybe done with API access, but I'm trying not to require that). This isn't that hard for people to track themselves, and teaching roll20 how to sum different values across the roll queries will be a giant pain, if it's possible at all.

# SHOW ME THE DANG MACROS ALREADY
These all use the OGL sheet's default roll templates, to try and keep as consistent an appearance as possible. Again, they rely on a few custom attributes you'll need to set, if you skipped 'USAGE' above.

## Telekinetic Force
Make sure you've got `empowered_psionics_bonus` and `psionic_power_save_desc` attributes set - see instructions above.
```
@{wtype} &{template:atkdmg} {{rname=Telekinetic Force}} {{damage=1}} {{dmg1flag=1}} {{range=60ft}} {{dmg1=[[[[(1+?{Hammering (1+)|0|1|2|3|4|5|6|7|8|9|10})]]d10  + @{empowered_psionics_bonus}[INT]]]}} {{dmg1type=Bludgeoning}} {{save=1}} {{saveattr=Strength}} {{savedesc=@{psionic_power_save_desc}}} {{savedc=[[[[(@{spell_save_dc})]][SAVE]]]}} {{desc=?{Zone of (0-3)?|0,Target is|1, All creatures in a **5'** sphere are|2, All creatures in a **10'** sphere are|3, All creatures in a **20'** sphere are} shoved [[5+?{Hurling (1-3)|0|1|2|3}*10]] feet or knocked prone. ?{Crushing (2)|No,&nbsp;|Yes,&#x00A;**Crushing:** The target is restrained until the end of its next turn.}}} @{charname_output}
```

## Kinetic Slam
This is just Telekinetic Force, with the Kinetic Slam talent to turn it into an attack roll instead of a save.

Make sure you've got the `empowered_psionics_bonus` attribute set - see instructions above.
```
@{wtype} &{template:atkdmg} {{rname=Kinetic Slam}} {{damage=1}} {{attack=1}} {{always=1}} {{mod=@{spell_attack_bonus}}} {{dmg1flag=1}} {{range=60ft}} {{r1=[[1d20+@{spell_attack_bonus}]]}} {{r2=[[1d20+@{spell_attack_bonus}]]}} {{dmg1=[[[[(1+?{Hammering (1+)|0|1|2|3|4|5|6|7|8|9|10})]]d10  + @{empowered_psionics_bonus}[INT]]]}} {{dmg1type=Bludgeoning}} {{crit=1}} {{crit1=[[[[1+?{Hammering (1+)|0|1|2|3|4|5|6|7|8|9|10}]]d10[CRIT]]]}} {{desc=The target is shoved 5 feet or knocked prone.}} @{charname_output}
```

## Telekinetic Weapons
Features a built-in option if you've previously activated Psionic Weapons, if you're using them together; you can just delete those four lines if you're not. As with Eldritch Blast, if you're flinging multiple weapons, you're going to want to just hit this button multiple times. It'd be even harder than EB to script up multiple flings, since the damage dice can vary between attacks.

Make sure you've got the `empowered_psionics_bonus` attribute set - see instructions above.
```
@{wtype} &{template:atkdmg} {{rname=Telekinetic Weapon}} {{damage=1}} {{attack=1}} {{always=1}} {{range=30ft}} {{mod=@{spell_attack_bonus}}} {{r1=[[1d20+@{spell_attack_bonus}]]}} {{r2=[[1d20+@{spell_attack_bonus}]]}} {{dmg1flag=1}} {{dmg1=[[?{Weapon Damage} + @{empowered_psionics_bonus}[INT]]]}} {{dmg1type=?{Damage Type|Slashing|Piercing|Bludgeoning}}} {{crit=1}} {{crit1=[[?{Weapon Damage}[CRIT]]]}} ?{Psionic Weapon|No, |Yes, {{dmg2flag=1&#125;&#125; {{dmg2=[[[[(ceil((@{level} + 2) / 6))]]d6]]&#125;&#125; {{dmg2type=Psionic Weapon&#125;&#125; {{crit2=1&#125;&#125; {{crit2=[[[[(ceil((@{level} + 2) / 6))]]d6[CRIT]]]&#125;&#125; } @{charname_output}
```

## Telepathic Intrusion
Make sure you've got `empowered_psionics_bonus` and `psionic_power_save_desc` attributes set - see instructions above.
```
@{wtype} &{template:atkdmg} {{rname=Telepathic Intrusion}} {{damage=1}} {{dmg1flag=1}} {{range=60ft}} {{dmg1=[[[[(1+?{Rending (1+)|0|1|2|3|4|5|6|7|8|9|10})]]d8 + @{empowered_psionics_bonus}[INT]]]}} {{dmg1type=Psychic}} {{save=1}} {{saveattr=Wisdom}} {{savedesc=@{psionic_power_save_desc}}} {{savedc=[[[[(@{spell_save_dc})]][SAVE]]]}} {{desc=The target has disadvantage on attacks against you until the start of your next turn. ?{Terrifying (1)|No, |Yes,&#x00A;**Terrifying (1):** The target is frightened until the end of its next turn&period;} ?{Overwhelming (2)|No, |Yes,&#x00A;**Overwhelming (2):** The target is *staggered* until the end of its next turn&period; A staggered creature's movement speed is halved&comma; it cannot take reactions&comma; and it has disadvantage on Dexterity saving throws and Constitution saving throws to maintain concentration on a spell.} ?{Reading (2)|No, |Yes,&#x00A;**Reading (2):** The target's next attack against you before the start of your next turn has disadvantage.}}} @{charname_output}
```

## Mind Thrust
This is just Telepathic Intrusion, with the Mind Thrust talent to turn it into an attack roll instead of a save.

Make sure you've got the `empowered_psionics_bonus` attribute set - see instructions above.
```
@{wtype} &{template:atkdmg} {{rname=Mind Thrust}} {{damage=1}} {{attack=1}} {{always=1}} {{mod=@{spell_attack_bonus}}} {{dmg1flag=1}} {{range=60ft}} {{r1=[[1d20+@{spell_attack_bonus}]]}} {{r2=[[1d20+@{spell_attack_bonus}]]}} {{dmg1=[[[[(1+?{Rending (1+)|0|1|2|3|4|5|6|7|8|9|10})]]d10  + @{empowered_psionics_bonus}[INT]]]}} {{dmg1type=Psychic}} {{crit=1}} {{crit1=[[[[1+?{Rending (1+)|0|1|2|3|4|5|6|7|8|9|10}]]d10[CRIT]]]}} {{desc=The target has disadvantage on attacks against you until the start of your next turn.}} @{charname_output}
```

## Enhancing Surge
For now, I've got this just outputting what the bonus damage is (game-wise, you probably don't want to preroll it). It's easy to preroll it; I think I know how to set it to be rollable on a click, and may yet do so.

Make sure you've got the `perfected_enhancement_bonus` attribute set - see instructions above.
```
@{wtype} &{template:dmg} {{rname=Enhancing Surge}} {{damage=1}} {{dmg1flag=1}} {{dmg2flag=1}} {{range=60ft}} {{dmg1=[[1d4+?{Fortifying (1+)|0|1|2|3|4|5|6||7|8|9|10}d6 + [[@{perfected_enhancement_bonus}]][INT/2]]]}} {{dmg1type=Temp HP}} {{dmg2=1d4+?{Savage (1+)|0|1|2|3|4|5|6|7|8|9|10}d6}} {{dmg2type=Bonus Damage}} {{desc=The target gains temp HP and deals bonus damage to one application of their next damage roll.?{Swift (2)|No, |Yes,&#x00A;**Swift (2):** The target gains 30ft of movement speed&period;} ?{Resilient (3)|No, |Yes,&#x00A;**Resilient (3):** The target gains resistance to all damage until the start of your next turn.}}} @{charname_output}
```

## Phase Rift
If you use Echoing, you should just roll this macro twice.
```
@{wtype} &{template:atkdmg} {{rname=Phase Rift}} {{damage=1}} {{dmg1flag=1}} {{range=?{Long (1-3)|0, 10ft|1, **20ft**|2, **30ft**|3, **40ft**}}} {{dmg1=[[[[(1+?{Disruptive (1+)|0|1|2|3|4|5|6|7|8|9|10})]]d8 + @{empowered_psionics_bonus}[INT]]]}} {{dmg1type=Force}} {{save=1}} {{saveattr=Dexterity}} {{savedesc=@{psionic_power_save_desc}}} {{savedc=[[[[(@{spell_save_dc})]][SAVE]]]}} {{desc=You step through space, making a rift and inflicting damage on any creature in your path. ?{Blurring (1)|No, |Yes,&#x00A;**Blurring (1):** You are heavily obscured until the start of your next turn&period;} ?{Ethereal (2)|No, |Yes,&#x00A;**Ethereal (2):** You can pass through solid objects&comma; buildings&comma; and terrain&period; If you would end inside a space you cannot occupy&comma; the power fails.} }} @{charname_output}
```

## Astral Construct Attack
This is just a quick macro for making an attack with your astral construct - it's actually one of the simplest,
being basically just a spiritual weapon macro. If you had ChatSetAttr installed, you could create a pretty fancy macro to put on an astral construct token to do things like auto-Solidify.

Make sure you've got `astral_construct_die` and `empowered_construct_bonus` attributes set - see instructions above.
```
@{wtype} &{template:atkdmg} {{rname=Astral Construct Attack}} {{damage=1}} {{attack=1}} {{always=1}} {{range=60ft}} {{mod=@{spell_attack_bonus}}} {{r1=[[1d20+@{spell_attack_bonus}]]}} {{r2=[[1d20+@{spell_attack_bonus}]]}} {{dmg1flag=1}} {{dmg1=[[?{Grow (1)|No, 1|Yes, 2}@{astral_construct_die} +  @{empowered_construct_bonus}[INT]]] }} {{dmg1type=Force}} {{crit=1}} {{crit1=[[?{Grow (1)|No, 1|Yes, 2}@{astral_construct_die}[CRIT]]]}}  } @{charname_output}
```

## Psionic Weapons
I mostly use it through the built-in option on TK Weapons, but this will quickly roll damage for you as needed.
```
@{wtype} &{template:dmg} {{rname=Psionic Weapon}} {{damage=1}} {{dmg1flag=1}} {{range=60ft}} {{dmg1=[[[[(ceil((@{level} + 2) / 6))]]d6]]}} {{dmg1type=Psychic}} @{charname_output}
```

## Rampage (with no die management)
Just rolls your rampage damage, it's up to you to track the die.
```
@{wtype} &{template:dmg} {{damage=1}} {{dmg1flag=1}} {{rname=Rampage (?{Rampage Die|d4|d6|d8|d10|d12})}} {{dmg1=[[1?{Rampage Die|d4|d6|d8|d10|d12}]]}} @{charname_output}
```
