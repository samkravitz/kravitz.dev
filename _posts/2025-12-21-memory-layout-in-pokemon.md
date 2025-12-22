---
layout: post
title: Memory Layout in Pokemon Generation I
date:   2025-12-21
categories: programming gaming
---
## Introduction
Pokemon was a staple of my childhood. I got lost in these games for hours, capturing and training up my friendly monsters. Apart from being core memories, these games also ignited my spark of curiosity for computing. How did games get made? How do they work? How is my pokemon team represented in code, and persisted in between rebooting the game? I wanted to answer these questions, for science, and definitely *not* as an excuse to revisit these games. Let's dig in.

## The Pokemon Data Structure (Pokemon Red/Blue/Yellow)
In Generation I, where the series began back in 1998, the 64 byte [data structure](https://bulbapedia.bulbagarden.net/wiki/Pok%C3%A9mon_data_structure_(Generation_I)) holds all the data that makes a Pokemon, well, a Pokemon.

The structure is layed out like this:
![Gen I Data Structure Layout](/assets/posts/memory_layout_in_pokemon/memory_layout.png)

Modelling this data structure in a language like C is trivial:

{% highlight c %}
typedef struct {
  uint8_t index_number;
  uint16_t current_hp;
  uint8_t level;
  uint8_t status_condition;
  // other fields
} Pokemon;
{% endhighlight %}


Of note, there are two fields for level, one at offset `0x03` and the other at offset `0x21`. Bulbapedia's article mentions that the first field, at offset `0x03`, is unused. We'll get to that later.

### Analyzing the layout
Knowing how the data structure is laid out, let's see how this works in practice. Here is a Vaporeon that I have in my game:
![Vaporeon](/assets/posts/memory_layout_in_pokemon/vaporeon.png)

Take note of its attributes:
- No.: 134
- Level: 25
- Status: OK
- HP: 102
- Type 1: Water

Imagine we can pause the game state and inspect the memory of the game at this very moment. [Sameboy](https://sameboy.github.io), the emulator I am using for this project, provides this feature! According to the article, the first Pokemon in our party's data structure begins at address `0xD163` + `0x08`. Let's go there and see what we see.

I've highlighted the 64 bytes (size of our data structure) at address `0xD16B` below. It's a bit hard to parse, but let's break it down:
![Vaporeon's data dump in hex](/assets/posts/memory_layout_in_pokemon/vaporeon_memory.png)


### Offset `0x00`: Index Number (1 byte)
The first byte in the data structure, `0x69`, is our Pokemon's index number. `Ox69` is `105` in decimal, which is different than the number we see in game, which is `134`. What gives? Well, the index number the game uses to keep track of what Pokemon is what is different than the Pokedex number which is displayed in game. [This page](https://bulbapedia.bulbagarden.net/wiki/List_of_Pok%C3%A9mon_by_index_number_in_Generation_I) shows the mapping between index numbers and Pokedex numbers (The first Pokemon [ever designed](https://nintendoeverything.com/rhydon-was-the-first-pokemon-ever-created/), Rhydon, is index number `1`). We can see that Vaporeon does indeed correspond to index number `105`, so we know we're looking at the right data.

### Offset `0x01`: Current HP (2 bytes)
Here, we see the value `0x0066`, which is `102` in decimal, the current HP of the Pokemon.

### Offset `0x03`: Level (1 byte)
This is the unusual one because there are two fields for level, one at `0x03`, and one at `0x21`. At both offsets we can see the value `0x19` (`25` decimal), so both seem to be accurate. Bulbapedia claims only the value at `0x21` is relevant, so I'm not sure why both work here.

### Offset `0x04`: Status Condition (1 byte)
The status condition byte is interpreted with the following bitfield:

| Bit | Value | Status condition |
|-----|-------|------------------|
| 3   | 0x04  | Asleep           |
| 4   | 0x08  | Poisoned         |
| 5   | 0x10  | Burned           |
| 6   | 0x20  | Frozen           |
| 7   | 0x40  | Paralyzed        |

My Pokemon, has status OK, so none of these bits are set, so seeing `0x00` at this offset makes sense.

### Offset `0x05`: Type (1 byte)
The final field we'll look at for now is the type byte, which follows this mapping:

| #   | Hex  | Type          |
|-----|------|---------------|
| 00  | 0x00 | Normal        |
| 01  | 0x01 | Fighting      |
| 02  | 0x02 | Flying        |
| 03  | 0x03 | Poison        |
| 04  | 0x04 | Ground        |
| 05  | 0x05 | Rock          |
| 06  | 0x06 | Bird (unused) |
| 07  | 0x07 | Bug           |
| 08  | 0x08 | Ghost         |
| 20  | 0x14 | Fire          |
| 21  | 0x15 | Water         |
| 22  | 0x16 | Grass         |
| 23  | 0x17 | Electric      |
| 24  | 0x18 | Psychic       |
| 25  | 0x19 | Ice           |
| 26  | 0x1A | Dragon        |

Our water type Pokemon has `0x15`, which is the correct mapping.

## Writing to our Data Structure
Our Vaporeon is cool and all, but I want my favorite Pokemon, Snorlax. If we could write values to this data structure, could I turn my Vaporeon into a Snorlax without having to go catch one? Sameboy provides this ability as well. Let's do it!

We'll craft a Snorlax with these attributes:
- No.: 143
- Level: 50
- Status: Asleep (as he often is)
- HP: 255
- Type 1: Normal

For fun, we'll also make all of his stats 150. I didn't describe where to find the offset for the stats for brevity, but it's the same process as the other attributes.

| Offset | Value (hex) | Value (Decimal) | Description             |
|--------|-------------|-----------------|-------------------------|
| 0x00   | 84          | 132             | Snorlax's index number  |
| 0x01   | FF          | 255             | Maximum HP              |
| 0x03   | 4           | 4               | Asleep status condition |
| 0x05   | 0           | 0               | Normal type             |

After writing these values, the memory layout of our data structure now looks like this:
![Snorlax's data dump in hex](/assets/posts/memory_layout_in_pokemon/snorlax_memory.png)

And the game reinterprets that structure to our desired creature. Neat!
![Snorlax](/assets/posts/memory_layout_in_pokemon/snorlax.png)

## Conclusion
By now, we've learned how the orginal Pokemon games interpret memory to display Pokemon and their attributes, as well as how to change them. Being able to write to any memory address on the console is insanely powerful, and basically lets you do anything you want in the game. There are many implications of this in gaming, such as cheating. I hope you learned something new in this post!
