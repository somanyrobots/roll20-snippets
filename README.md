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
* It really sucks that roll20 demands these be one-line snippets; when editing, I suggest adding line breaks every 3-4 items.
* I haven't found any way to include one's Psi Limit as part of the macros. So right now I've just got the roll queries for a 20th-level PC - ideally, on all the 1+ queries, we'd cap them at the psi limit for every level.
* It's probably impossible to teach roll20 to actually enforce a psi limit within a turn, or even within a macro (it could be maybe done with API access, but I'm trying not to require that). This isn't that hard for people to track themselves, and teaching roll20 how to sum different values across the roll queries will be a giant pain, if it's possible at all.
