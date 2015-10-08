#3. Standard classes
## Connotations
There are specific classes used to reflect the general connotation of a dictionary entry.

### Classes
|Class Name|Meaning|Example|
|:--------:|-------|-------|
|`cn3`|Objectively negative. Almost always offensive if used.|shitty|
|`cn2`|Generally regarded as negative, but not objectively negative.|vulgar|
|`cn1`|Only appropriate in very specific contexts.|moist|
|`c0`|Neither perceived as good nor bad. A regular word.|cylindrical|
|`cp1`|Generally regarded as positive, cheerful, or constructive.|beautiful|

## Preposition agreement
Many dative verbs only work with specific prepositions. These classes indicate which prepositions are compatible with an entry.

The classes are named using a "preposition grid" based on the standard US numeric keypad. See the diagram below:

```
To create a preposition class, use the grid below to find the preposition you want.
Then, put a "p" before it.

Example: "around" = "p94"

                   78: onto    98: above
              ╔═════════╦═════════╦═════════╗
              ║    7    ║    8    ║    9    ║
              ║   TO*   ║  over   ║  FROM*  ║
              ╠═════════╬═════════╬═════════╣
74: beside -> ║    4    ║    5    ║    6    ║ <- 76: at
94: around <- ║ across  ║ inside  ║ outside ║ -> 96: away
              ╠═════════╬═════════╬═════════╣
              ║    1    ║    2    ║    3    ║
              ║ behind  ║  under  ║  front  ║
              ╚═════════╩═════════╩═════════╝
                 72: up to      92: below

* Modifier digit.
```
