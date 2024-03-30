####AKA The Bingo Game####
This program is used in conjunction with bingosync.com to make a real-life bingo board speedrun game!
This is intended for play in a walkable populated settlement, and all players must have Internet-connected mobile devices. 
Full rules can be found here: https://docs.google.com/document/d/1uKTJbRp8W9Tf3kbdvQgDlT9pDRzDaCNnuTFc2Y-V8R8/edit?usp=drivesdk
The rest is up to you! Feel free to make your own goals. Detail on their formatting is found below
If you spot any bugs, you can contact me on Tumblr! @thepixelpenguin

===FORMATTING===
Goals must be written on a single line. Parentheses and brackets are reserved characters. 
Use brackets to indicate a range of numbers or letters to pick from at random. This is not padded with spaces. 
If the random number picked is 1, it will remove the letter s at the end of the goal
If picking a random letter, make sure the limits are both uppercase!
Use parentheses to indicate which bans outlaw this goal. This is used to make sure no lines are self-contradictory.  The parentheses must be at the end and contain a comma-separated list (no spaces) of the relevant tags.
If you're making a new ban, it must start with "Don't", and have a single-character tag in parentheses
Any goal that contains "at the end" should not be tagged. They will be rearranged so no two are in the same line.
Goals with asterisks have more clarification attached to them in the rules that won't fit in a square.
If you reorder the current goals, be prepared for the import command to break!

Rules are split into modifiers and win conditions by an ellipsis. Multiple modes can be chosen, but only one win.
Modes are written on a single line and end in a full stop; wins end in an exclamation mark instead. That's for clarity's sake.
Modes and wins have descriptors associated with them, in quotes. These are used to create a uniquely defined (if clunky) name so a particular ruleset can be quickly identified.
Modes have three tags: identity (=), exclusion (/), and requirement (-). They may have one of each.
Identity tags work like the tag on a ban: a single character used to identify a line. 
Exclusion tags mark a group of modes that are all mutually incompatible.
Requirement tags can apply to modes and wins. They specify a line with an identity tag which is a prerequisite.
Exclusion applies before requirement.
Requirement doesn't add the descriptor of the required rule, since it'd just be redundant.
If you make self-contradictory tags, the code will probably break...
