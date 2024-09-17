---
title: "Anki for Chinese"
about: "A quick primer on how to use Anki (a spaced repetition software) in general and how to set it up for learning Chinese in particular."
---
## What is Anki?
Anki is a [<q>free and open-source flashcard program using spaced repetition, a technique from cognitive science for fast and long-lasting memorization</q>](https://en.wikipedia.org/wiki/Anki_(software)).
Anki's philosophy follows an object-oriented programming paradigm. **Notes** are Anki’s equivalent of objects, storing the data using **fields** to store single pieces of information. To review you need **cards**, which are the visual representation of (selected subset of) a note. Do not store information in separate **decks** (such as `english-chinese`, `chinese-english`) so that in case you need to edit something you only need to do it once.

## "Special effects" for cards
Find proper explanations and examples in the [Anki manual's Card Generation article](https://docs.ankiweb.net/templates/generation.html). 
* typing the answer: `{%raw%}{{type:FIELDNAME}}{%endraw%}`
* conditional statements: `{%raw%}{{#FIELDNAME}}…{{/FIELDNAME}}{%endraw%}`
* hints: `{%raw%}{{hint:FIELDNAME}}{%endraw%}`
* the odd one: `{%raw%}{{cloze:FIELDNAME}}{%endraw%}`

## Reviewing
To test e.g. only english-hanzi, do a logical search like `(card:3 or card:5) deck:中文` if you have the same cards (in particular in the same order - that’s what the number refers to) as mentioned below.

## Setting up Anki for Chinese
[Chinese Support (Addon)](https://ankiweb.net/shared/info/3448800906) introduces (amongst others) the following fields:

* Hanzi
* Meaning
* Reading
* Color
* Sound
* Traditional
* Simplified

I would recommend adding the following fields as well:

* Stroke Order Diagram
* Sentence (example sentences)
* Similar (Hanzi that look similar)

As for the cards, I found the following to be helpful:

1. Hanzi → Pinyin
2. Hanzi → Meaning
3. Meaning → Hanzi
4. Meaning → Pinyin
5. Listening → Hanzi
6. Listening → Meaning