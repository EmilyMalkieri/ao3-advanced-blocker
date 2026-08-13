# AO3 Advanced Blocker

Userscript for Tampermonkey

Fork of Wolfbatcat's/Blackbatcat's version, available at https://github.com/Wolfbatcat/ao3-userscripts and https://greasyfork.org/en/scripts/549942-ao3-advanced-blocker.

This is just my own version designed to my own preferences and usecase. Feel free to use it--I consider it a large improvement--but keep in mind the features labeled ⚠️ below.

## Migrating to this version

Delete the regular version from your tampermonkey dashboard, then click the boxed-in "+" button and paste in this script.

That's it, that's all you need to do. This will reuse any settings you made for the original. If you're using AO3 Script Sync, that will also just keep working.

Remember that if you're not using Tampermonkey Sync, you'll have to uninstall the original and install this script on each of your devices.

This script doesn't auto-update.

## Changes from upstream

The as of this writing current version of the original is supplied in the `upstream` branch (though with my autoformat settings applied). If you want to see what's actually changed in the code, [just compare the two branches](https://github.com/EmilyMalkieri/ao3-advanced-blocker/compare/upstream...main).

### Easier matching

The biggest limitation of this script is that it matches against tags as they are displayed on the page, not as they exist in AO3's database. That means we have to anticipate how authors spell their characters, relationships, fandoms, and other tags. And if there's one thing authors are, it's creative with name spellings. Several changes have been made to try to alleviate this issue as much as possible:

- Fandoms are now automatically expanded into regular expressions, matching any text before and after. So for example, if you're a big My Hero Academia fan, you don't have to copy-paste Kanji or worry about how exactly authors spell your fandom. Just write `with:{Hero Aca}` and you're good. If you need wildcards in the middle of your fandom, write `with:{Avatar.*Airbender}`. (That's a real regex `.*`, not a plain `*` wildcard like is accepted elsewhere.)
    - Note that this is on top of the recently added `||` ("or") combinator. If you need to, you can combine both.
- Character names are similarly expanded into regular expressions from both ends, both for character filters and for relationships. So you could write just `Kara` and it'd match `Kara Danvers`, `Kara Zor-El`, `Kara Danvers (Supergirl 2015)`, `Supergirl | Kara Danvers`, `Evil kara from another universe` and any other new, exciting, and entirely unpredictable spelling variants. But we're making sure you're actually matching her, not another name like `Skara` that happens to include the same letters.
    - This means that for relationships, if you want `Kara/Lena`, you can actually just write that.
- ⚠️ Relationships are now accepted in any order. That is, if you write `A/B`, we're also matching for `B/A`.

### Poly-inclusive matching

Matching for poly ships has been expanded and simplified based on two assumptions:

- If you like a poly ship, we assume you also like subgroups of that ship. (That is, `A/B/C` also matches `A/B`, `A/C`, and `B/C`.)
- ⚠️ If you like a ship, we assume you'll still like it with another character added in. (That is, `A/B` also matches `A/B/D`.)
    - We're aware that this will occasionally let through an absolutely cursed tag that you hate. We encourage vigorous use of the block functionality and believe the benefits of finding more fun ships outweigh the negatives here.

In plain terms, for any relationship you specify, we're matching not just that exact ship, but any ship that includes two or more of these characters.

### Miscellaneous changes

- We make an attempt at filtering out false positive matches for relationships, such as `past Your Favourite/Ship` and `Your Favourite/Ship (mentioned)`.
- We still match relationships if none of your character filters apply to this fandom, and vice versa.
- ~~Fixed an issue where blacklisted tags weren't being preserved.~~ (Since fixed in upstream.)

