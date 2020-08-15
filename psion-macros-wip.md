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
