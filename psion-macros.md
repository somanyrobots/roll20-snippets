# PSION MACROS
This is a collection of macros to make it easier to play a Psion using Roll20's 5E OGL character sheet. No API scripts necessary. These rely heavily on Roll20's Roll Query feature, and will throw up prompts to select how you're using  your powers.

This Psion class is by [KibblesTasty](https://www.kthomebrew.com/).

# USAGE
1. Grab the ability you want and add it to your character sheet as a custom ability.
2. Set the following attributes on the character sheet:
* `psionic_power_save_desc`: This should be `No damage or effects.` If you have Potent Psionics, instead enter `Half damage and no effects.` You'll need this for any ability with a saving throw (though the math will still be fine - it's just for descriptions).
* `empowered_psionics_bonus`: This should be `0`. If you have Empowered Psionics, instead enter `@{intelligence_mod}`. You'll need this one for most abilities.
* `perfected_enhancement_bonus`: This should be `0`. If you have Perfected Enhancement, instead enter `floor(@{intelligence_mod}/2)`. You only need this one for Enhancing Surge.

# LIMITATIONS
There's no way to edit attributes without API access, so these abilities won't directly modify any attributes on your character. Also, there's no way to dynamically switch between attack rolls and saving throws, so for powers that do so, you'll need a separate macro for each mode. Also, it really sucks that roll20 demands these be one-line snippets; if you edit them, I suggest adding line breaks every 3-4 items.

# SHOW ME THE DANG MACROS ALREADY
## Telekinetic Force
```
@{wtype} &{template:atkdmg} {{charname=@{charname_output}}} {{rname=Telekinetic Force}} {{damage=1}} {{dmg1flag=1}} {{range=60ft}} {{dmg1=[[[[(1+?{Hammering (1+)|0|1|2|3|4})]]d10  + @{empowered_psionics_bonus}[INT]]]}} {{dmg1type=Bludgeoning}} {{save=1}} {{saveattr=Strength}} {{savedesc=@{psionic_power_save_desc}}} {{savedc=[[[[(@{spell_save_dc})]][SAVE]]]}} {{desc=?{Zone of (0-3)?|0,Target is|1, All creatures in a **5'** sphere are|2, All creatures in a **10'** sphere are|3, All creatures in a **20'** sphere are} shoved [[5+?{Hurling (1-3)|0|1|2|3}*10]] feet or knocked prone. ?{Crushing (2)|No,|Yes,&#x00A;**Crushing:** The target is restrained until the end of its next turn.}}} @{charname_output}
```

## Telekinetic Weapons
Features a built-in option if you've previously activated Psionic Weapons, if you're using them together; you can just delete those four lines if you're not. As with Eldritch Blast, if you're flinging multiple weapons, you're going to want to just hit this button multiple times. It'd be even harder than EB to script up multiple flings, since the damage dice can vary between attacks.
```
@{wtype} &{template:atkdmg} {{charname=@{charname_output}}} {{rname=Telekinetic Weapon}} {{damage=1}} {{attack=1}} {{always=1}} {{range=30ft}} {{mod=@{spell_attack_bonus}}} {{r1=[[1d20+@{spell_attack_bonus}]]}} {{r2=[[1d20+@{spell_attack_bonus}]]}} {{dmg1flag=1}} {{dmg1=[[?{Damage Dice} + @{empowered_psionics_bonus}[INT]]]}} {{dmg1type=?{Damage Type|Slashing|Piercing|Bludgeoning}}} {{crit1=1}} {{crit1=[[?{Damage Dice}[CRIT]]]}} ?{Psionic Weapon|No, |Yes, {{dmg2flag=1&#125;&#125; {{dmg2=[[[[(ceil((@{level} + 2) / 6))]]d6]]&#125;&#125; {{dmg2type=Psionic Weapon&#125;&#125; {{crit2=1&#125;&#125; {{crit2=[[[[(ceil((@{level} + 2) / 6))]]d6[CRIT]]]&#125;&#125; } @{charname_output}
```

## Telepathic Intrusion
```
@{wtype} &{template:atkdmg} {{charname=@{charname_output}}} {{rname=Telepathic Intrusion}} {{damage=1}} {{dmg1flag=1}} {{range=60ft}} {{dmg1=[[[[(1+?{Rending (1+)|0|1|2|3|4})]]d8 + @{empowered_psionics_bonus}[INT]]]}} {{dmg1type=Psychic}} {{save=1}} {{saveattr=Wisdom}} {{savedesc=@{psionic_power_save_desc}}} {{savedc=[[[[(@{spell_save_dc})]][SAVE]]]}} {{desc=The target has disadvantage on attacks against you until the start of your next turn. ?{Terrifying (1)|No, |Yes,&#x00A;**Terrifying (1):** The target is frightened until the end of its next turn&period;} ?{Overwhelming (2)|No, |Yes,&#x00A;**Overwhelming (2):** The target is *staggered* until the end of its next turn&period; A staggered creature's movement speed is halved&comma; it cannot take reactions&comma; and it has disadvantage on Dexterity saving throws and Constitution saving throws to maintain concentration on a spell.} ?{Reading (2)|No, |Yes,&#x00A;**Reading (2):** The target's next attack against you before the start of your next turn has disadvantage.}}} @{charname_output}
```

## Psionic Weapons
I mostly use it through the built-in option on TK Weapons, but this will quickly roll damage for you as needed.
```
&{template:dmg} {{charname=@{charname_output}}} {{rname=Psionic Weapon}} {{damage=1}} {{dmg1flag=1}} {{range=60ft}} {{dmg1=[[[[(ceil((@{level} + 2) / 6))]]d6]]}} {{dmg1type=Psychic}} @{charname_output}
```

## Rampage (with no die management)
Just rolls your rampage damage, it's up to you to track the die.
```
&{template:dmg} {{charname=@{charname_output}}} {{damage=1}} {{dmg1flag=1}} {{rname=Rampage (?{Rampage Die|d4|d6|d8|d10|d12})}} {{dmg1=[[1?{Rampage Die|d4|d6|d8|d10|d12}]]}} @{charname_output}
```
