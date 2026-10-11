# Lesson 5 - Composition

*Twelve numbers, and which one is which.*

```
dotnet run
```

It prints three character sheets and the start of a fight.  Read all three
sheets before you open a file.  Then ask the question this lesson is built on:

> **Bree is the party's wizard.  Does her sheet look like a wizard's?**

| File | What it is |
|---|---|
| `Player.cs` | **Today's file.**  A player character, one property per line, with a Lesson 2 factory guarding the door. |
| `Creature.cs` | Anything the players fight.  It looks familiar on purpose.  It's your homework. |
| `Display/CharacterSheet.cs` | Prints a sheet.  Lesson 3's move:  display beside the data. |
| `Rules/Combat.cs` | Armor class and attack bonus.  Rules beside the data. |
| `Program.cs` | Builds the party and a goblin.  Keep it short. |

## A name you've heard before

**Primitive obsession** is a design that says everything in `int` and `string`
because those were the types at hand.

You've met it once already:  one number with a rule, stored as a bare `int`,
so nothing stopped an illegal value.  The fix was a type that owns the rule.

Today it's back in a different shape.  Count the `int`s in `Player`.  Every one
of them is legal.  The trouble is that six of them only mean anything
*together*, and nothing in the code says so.

## The one new idea

**A type can be made of other types.**

When a group of values always travels together, it's a type nobody has named
yet.  Every character sheet ever printed draws the six ability scores in one
box.  The code should too.

You've done this before without the word for it.  A `Card` is made of a `Suit`
and a `Value`.  A `Deck` is made of a list of cards.  Building a type out of
other types is called **composition**, and a `Player` is about to get the same
treatment.

## Things to notice before you change anything

Everything here works exactly the way it was written.  Your job for now is to
explain it.

1. **Find the wizard.**  Read Bree's sheet, then read the line in `Program.cs`
   that builds her.  What went wrong, and why did the program let it?
2. **Try to write the guard.**  `Player.Create` checks every number.  Write one
   more check that would have caught Bree.  What would it have to know?
3. **Swap a different pair.**  In Tamsin's line, swap current and max health.
   Run it.  Why does *that* swap get caught, when Bree's didn't?
4. **Count the rule.**  How many times is "3 to 18" written in this project?
   How many places compute a modifier from a score?

## Today's steps

1. **Read `Player` and count.**  How many members?  How many are `int`?  Which
   ones belong together?
2. **Named arguments.**  Rewrite Bree's line with names:
   `intelligence: 15, wisdom: 12`.  Does it fix the bug?  Does it stop the next
   person from making it?
3. **Extract `AbilityScores`.**  One record holding the six scores, with its own
   factory.  The 3-to-18 rule moves in with them.  Lesson 2's door follows the
   data.
4. **Give `Player` an `AbilityScores`.**  `Player.Create` takes one now in place
   of six `int`s.  Count `Player`'s members again.
5. **Fix the callers.**  `CharacterSheet` and `Combat` reach for
   `player.Strength`.  What do they reach for now?
6. **Count the cost honestly.**  Is `AbilityScores.Create(8, 14, 13, 15, 12, 10)`
   any safer than what we started with?  What did the extraction actually buy?

## Went beyond?

- **Make every caller say the names.**  Look up `required` and `init`.  Build
  `AbilityScores` so `new AbilityScores { Strength = 8, ... }` is the only way
  in, and leaving one out won't compile.  What does it cost you?
- **The next pair.**  Two more numbers in `Player` always travel together.
  Find them.  Don't extract them yet -- say what type they want to be, and what
  rule it would keep.
- **Bonus level:  `Level` and `Experience`.**  No new tools needed, just
  thinking.  Is `Level` part of a character's stats, or something separate?
  And are `Level` and `Experience` really two independent numbers?  Could a
  level-3 character have 0 XP?  Should the code allow it?

## Done for today?

> Name the six numbers you moved, the type they moved into, and one mistake that
> no longer compiles because of it.
