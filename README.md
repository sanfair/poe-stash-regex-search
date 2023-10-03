# Regex Patterns for PoE Stash Tab Searches

GGG has [added regex search to stash tabs](https://www.reddit.com/r/pathofexile/comments/n3pxxc/when_did_we_get_regex_search_in_tabs/gwtxiqb/?context=10) in patch 3.14.

This is a collection of patterns I find useful.

Tip: Use [Awakened PoE Trade](https://github.com/SnosMe/awakened-poe-trade)'s feature to group and save your patterns. It also pastes one of the saved patterns with a click of a button. You can also use this to search a vendor's inventory.

Contents:
- [How does it work?](#how-does-it-work)
- [Patterns](#patterns)

## How does it work?
With regex search you can search each line of the item text (CTRL+C) case-insensitvely. This is equivalent to the `m` and `i` modifiers.

Hint: Play around with regexes on https://regex101.com.

### Example
Let's say you have two items (only relevant information below):

Item 1
```
Quality: +9%
--------
Requirements:
Level: 70
Str: 62
Dex: 62
--------
+15 to Strength
```

Item 2
```
Quality: +12%
--------
Requirements:
Level: 70
Dex: 62
--------
+23 to Strength
```

#### Basic search
If you want to search for the item with the strength requirement you can use:

```
str:
```
or
```
^str
```

However, this won't work:
```
str
```
since it also matches the mod.

#### At least 10% Quality
To do this, you have to define that you want the number after the text `Quality: +` to be 2 digits long and the first digit should be 1 or higher.
```
"quality: \+([1-9][0-9])"
```

## Patterns

### Generic

- 6-links
  ```
  "sockets: ([rgbw]-?){6}"
  ```
- 6 number of sockets  
  ```
  "sockets: ([rgbw].?){6}"
  ```
- +10% and higher Quality
  ```
  "quality: \+([1-9][0-9])"
  ```
- 70+ life
  ```
  "([1-9]\d{2,}|[7-9]\d).+life"
- 75+ life
  ```
  "([1-9]\d{2,}|[8-9]\d|[7][5-9]).+life"
  ```
- 80+ item level
  ```
  "item level: ([1-9]\d{2,}|[8-9]\d)"
  ```

### Item classes

#### Requirements of class x
- All of class x
  ```
  "class: x"
  ```

- Strength
  ```
  "class: x" "^str" "!(^dex)"
  ```

- Dexterity
  ```
  "class: x" "^dex" "!(^(str|int))"
  ```

- Intelligence
  ```
  "class: x" "^int" "!(^(str|dex))"
  ```

- Strength / Dexterity
  ```
  "class: x" str: dex:
  ```

- Strength / Intelligence
  ```
  "class: x" dex: str:
  ```

- Dexterity / Intelligence
  ```
  "class: x" dex: int:
  ```

### Other

- Chaos Recipe
  ```
  "rare"|"item level: ([6][0-9]|[7][0-4])"
  ```
  
- Regal Recipe
  ```
  "rare"|"item level: (100|[8-9][0-9]|[7][5-9])"
  ```
  
## Sources
I didn't come up with all of them. Some sources:

https://www.reddit.com/r/pathofexile/comments/n3pxxc/when_did_we_get_regex_search_in_tabs/
https://www.reddit.com/r/pathofexile/comments/nb8j00/rolling_maps_with_regex/
https://www.reddit.com/r/pathofexile/comments/og135v/i_learned_about_the_poe_stash_and_vendor_regex/
