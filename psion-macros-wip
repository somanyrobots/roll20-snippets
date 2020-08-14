###
### WIP
###

## Rampage (with die management)
Requires ChatSetAttr for automatic die management

&{template:dmg} {{charname=@{charname_output}}}
{{rname=Rampage}} {{damage=1}} {{dmg1flag=1}} {{dmg2flag=1}}
{{dmg1=[[1d@{rampage_die_size}]]}} {{dmg1type=Damage}}
{{dmg2=1d[[{@{rampage_die_size}+2,12}kl1]]}} {{dmg2type=Next Rampage}}
!setattr --silent --charid @{character_id} --rampage_die_size|[[{@{rampage_die_size}+2,12}kl1]]|12 !!!
@{charname_output}


## Elemental Blast
@{Zariyah|wtype}&{template:atkdmg} {{charname=Zariyah}} {{rname=Cryokinetic Blast}}
{{always=1}} {{damage=1}} {{attack=1}}
{{r1=[[1d20cs>1+@{Zariyah|spell_attack_bonus}-?{Overcharged|No,0|Yes,@{Zariyah|pb}}]]}}
{{r2=[[1d20+@{Zariyah|spell_attack_bonus}-?{Overcharged|No,0|Yes,@{Zariyah|pb}}]]}}
{{dmg1flag=1}} {{dmg1=[[(1+?{Amplified|0})d8 + 5[INT]]]}} {{dmg1type=Cold}} {{crit1=1}} {{crit1=[[(1+?{Amplified|0})d8[CRIT]]]}}
{{dmg2flag=1}} {{dmg2=[[2*?{Overcharged|No,0|Yes,@{Zariyah|pb}}]]}} {{dmg2type=Overcharge}}
{{desc=The target is slowed [[5+?{Amplified|0}*5]] feet, and must make a Constitution save or be restrained until the end of their next turn. ?{Lasting (1)|No, |Yes,&#x00A;**Lasting (1):** Your blast leaves a 5 foot sphere of devastation at the target point. Any creatures that enter this zone or end their turn there suffer the blast's secondary effects.}}}
@{Zariyah|charname_output}

## Elemental Blast Massive
@{Zariyah|wtype}&{template:atkdmg} {{charname=Zariyah}} {{rname=Cryokinetic Blast (Massive)}} {{attack=1}} {{damage=1}} {{dmg1flag=1}} {{dmg1=[[(1+?{Amplified|0})d8 + 5[INT]]]}} {{dmg1type=Cold}} {{crit1=[[?{Amplified|0}d8[CRIT]]]}} {{dmg2flag=1}} {{dmg2=[[2*?{Overcharged|No,0|Yes,@{Zariyah|pb}}]]}} {{dmg2type=Overcharge}} {{save=1}} {{saveattr=Dexterity}} {{savedesc=No damage or effects}} {{savedc=[[[[(@{Zariyah|spell_save_dc}-?{Overcharged|No,0|Yes,@{Zariyah|pb}})]][SAVE]]]}} {{desc=The target is slowed [[5+?{Amplified|0}*5]] feet, and must make a Constitution save or be restrained until the end of their next turn. ?{Lasting (1)|No, |Yes,&#x00A;**Lasting (1):** Your blast leaves a 5 foot sphere of devastation at the target point. Any creatures that enter this zone or end their turn there suffer the blast's secondary effects.}}} {{desc=All targets in a 30 ft cone take damage and are slowed [[5+?{Amplified|0}*5]] feet, and must make a Constitution save or be restrained until the end of their next turn. ?{Lasting (1)|No, |Yes,&#x00A;**Lasting (1):** Your blast leaves a 5 foot sphere of devastation at the target point. Any creatures that enter this zone or end their turn there suffer the blast's secondary effects.}}}

## Enhancing Surge
@{wtype} &{template:atkdmg} {{charname=@{charname_output}}}
{{rname=Enhancing Surge}} {{damage=1}} {{dmg1flag=1}} {{dmg2flag=1}} {{range=60ft}}
{{dmg1=[[[(1d4+[?{Fortifying (1+)|0|1|2|3|4})]]d6 + @{intelligence_mod}[INT]]]}} {{dmg1type=Psychic}}
{{dmg1=[[[[(1+?{Rending (1+)|0|1|2|3|4})]]d8 + @{intelligence_mod}[INT]]]}} {{dmg1type=Psychic}}
{{desc=The target has disadvantage on attacks against you until the start of your next turn. ?{Terrifying (1)|No, |Yes,&#x00A;**Terrifying (1):** The target is frightened until the end of its next turn&period;} ?{Overwhelming (2)|No, |Yes,&#x00A;**Overwhelming (2):** The target is *staggered* until the end of its next turn&period; A staggered creature's movement speed is halved&comma; it cannot take reactions&comma; and it has disadvantage on Dexterity saving throws and Constitution saving throws to maintain concentration on a spell.} ?{Reading (2)|No, |Yes,&#x00A;**Reading (2):** The target's next attack against you before the start of your next turn has disadvantage.}}}
@{charname_output}
