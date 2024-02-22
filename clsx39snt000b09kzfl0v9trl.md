---
title: "Diving Back into Games-related Bugs! , especially, cards related games! 🕹️🎮"
datePublished: Thu Feb 22 2024 10:37:11 GMT+0000 (Coordinated Universal Time)
cuid: clsx39snt000b09kzfl0v9trl
slug: diving-back-into-games-related-bugs-especially-cards-related-games
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708598112364/4f8ac85c-e6b6-4548-aebf-571c52c0d9b1.webp
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1708598126981/f0a7b934-7171-4f5e-b85d-68ce81da25d0.webp
tags: bugbountytip-bugbounty-games-hacking

---

it's been a while since I tweeted about these kind of flaws, so here we are adding 3 more common bugs I see in games into the list ;)  
  
In the landscape of online games, particularly those involving cards or characters, players frequently encounter a trio of exploits that can significantly impact gameplay and fairness. This post aims to dissect these vulnerabilities, shedding light on their mechanisms and potential implications.

**1\. Playing More Cards Than Allowed** 🃏

Games typically enforce a limit on the number of cards or characters a player can use in a single round to maintain balance. However, some players manipulate client-server communication to bypass these limits. By altering the game state request sent to the server, they can include additional cards beyond the intended limit.

*Scenario Example:* 🎮  
A game allows a player to use four cards per round. The standard request might look like this:

```json
{
  "cards": [
    {"card1": "Spiderman"},
    {"card2": "Ant-Man"},
    {"card3": "Hulk"},
    {"card4": "Pikachu"}
  ]
}
```

Exploiting the game, a player modifies the request to include a fifth card:

```json
{
  "cards": [
    {"card1": "Spiderman"},
    {"card2": "Ant-Man"},
    {"card3": "Hulk"},
    {"card4": "Pikachu"},
    {"card5": "Super-Man"}
  ]
}
```

This exploit can lead to an unfair advantage by allowing the player to use more resources than opponents leading to instant win!

**2\. Using the Same Character or Card Multiple Times** 🔄️

Another common exploit involves duplicating a character or card within the same game, a scenario not typically allowed under normal gameplay rules. This is achieved by sending a manipulated request that includes multiple instances of the same card or character.

*Manipulation Example: 🔁*  
Instead of selecting diverse characters, the player repeats a powerful choice:

```json
{
  "cards": [
    {"card1": "Hulk"},
    {"card2": "Hulk"},
    {"card3": "Hulk"},
    {"card4": "Hulk"}
  ]
}
```

This approach can disrupt game balance by overwhelming opponents with repeated uses of powerful abilities or characters!

**3\. Using Abilities or Gear Before Unlocking Them** 🔓

Games often gate character abilities or gear behind progression milestones or purchases. Some players, however, find ways to circumvent these restrictions by crafting requests that include abilities or gear not yet unlocked.

*Exploit Illustration: 🗝️*  
A player without the necessary level or purchase sends a request to use a high-level ability:

```json
{
  "character": "Thor",
  "gear": ["Mjolnir"], // Assuming Mjolnir is not yet unlocked
  "ability": "Lightning Strike" // Also not unlocked
}
```

This type of exploit can significantly impact competitive play, allowing players to access powerful tools without the required investment or progression. ⚡