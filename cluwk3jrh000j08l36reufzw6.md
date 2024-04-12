---
title: "More of Games-related Bugs!"
datePublished: Fri Apr 12 2024 10:59:51 GMT+0000 (Coordinated Universal Time)
cuid: cluwk3jrh000j08l36reufzw6
slug: more-of-games-related-bugs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1712919383438/e7d9faeb-15e3-4ce1-b6dd-e113837e5940.png
tags: hacking, games, pentesting, bugbounty

---

**Exploring Chests or Boxes: Unraveling the Secrets 🎲**

In the digital realm of gaming, chests and boxes are akin to Pandora's Box, each unveiling unique rewards and surprises. For instance, you're generally allowed to open a 'Golden Box', but what if, with a simple modification in the request, you could access the coveted 'Diamond Box'? This minor alteration not only permits players to access content they're not supposed to yet but might also allow them to do so at a significantly reduced cost, disrupting the game's intended economic balance.

A standard request to open a "Golden Box" might look like this:

```json
{
  "action": "openChest",
  "chestType": "Golden"
}
```

However, by changing the `chestType` to "Diamond," players can manipulate the outcome:

```json
{
  "action": "openChest",
  "chestType": "Diamond"
}
```

This adjustment could enable players to bypass intended gameplay progression and resource allocation, offering an unfair advantage or altering their gaming experience.

**Debug Endpoints: Hidden Passageways Within the Code 🛠️**

Debug endpoints are like hidden corridors within a game's architecture, intended for developers to troubleshoot and test. However, when these endpoints remain active in a live environment, they become secret tools for players to exploit. These can range from altering player stats to revealing hidden game mechanics, potentially giving some players an unfair edge or breaking the game's immersive experience.

Imagine stumbling upon a debug endpoint that allows you to modify your character's attributes:

```http
POST /debug/modifyAttributes HTTP/1.1

{
  "userId": "player123",
  "attributes": {
    "strength": 100,
    "speed": 100
  }
}
```

Access to such an endpoint can drastically alter the game's competitive landscape, enabling players to modify their avatars beyond the game's standard limitations.

**IDORs: Unintended Consequences of Poor Validation 🔄**

One of the funny examples me and my co workers in [Cyrex](https://cyrex.tech/) usually find is that games are often riddled with IDOR vulnerabilities, allowing for unexpected and often exploitative interactions. An example is when a game allows you to make a change or upgrade that should only affect your account but ends up impacting another player's account if the user ID in the request is altered.

Consider a scenario where you're supposed to upgrade a building:

```http
POST /api/upgradeBuilding HTTP/1.1

{
  "userId": "12345",
  "buildingId": "67890",
  "currency": 1000
}
```

By merely changing the `userId` to that of another player, you could potentially charge the cost to them, leading to unauthorized resource transfers:

```http
POST /api/upgradeBuilding HTTP/1.1

{
  "userId": "67890",
  "buildingId": "67890",
  "currency": 1000
}
```

**DOS Attacks: Exploiting System Limits for Disruption 🚫**

**Long Usernames/Names:** When games do not enforce backend validation for the length of usernames, players can create excessively long names, causing stress on the server and affecting the gameplay experience for others. This can result in server slowdowns or even crashes.

```http
POST /api/createUser HTTP/1.1
Host: examplegame.com
Content-Type: application/json
Content-Length: [length]

{
  "username": "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa...",
  "password": "SecurePassword123!",
  "email": "user@example.com"
}
```

**Long Messages:** Without limits on message lengths, players can flood chat systems with extensive messages, overwhelming the chat interface and potentially causing backend issues due to the excessive load.

```http
POST /api/sendMessage HTTP/1.1
Host: examplegame.com
Content-Type: application/json
Content-Length: [length]

{
  "message": "Lorem ipsum dolor sit amet, consectetur adipiscing elit... (extremely long text)",
  "userId": "123456",
  "roomId": "654321"
}
```

**Manipulating Transactions for Unforeseen Gains 💰**

A peculiar yet exploitative aspect we've encountered involves transactions where the game fails to validate whether the `currencyPaid` value is negative. When a player is supposed to pay a certain amount to receive or upgrade an in-game item, flipping the amount to a negative number could, ironically, increase their in-game currency instead of deducting it.

A typical upgrade request might look like this:

```json
POST /API/upgrade_house
{
  "houseId": "98765",
  "currencyPaid": 50000
}
```

But altering the `currencyPaid` to a negative value could unexpectedly credit the player's account:

```json
POST /API/upgrade_house
{
  "houseId": "98765",
  "currencyPaid": -50000
}
```

This action could result in the player receiving additional funds, turning what should be an expenditure into a gain, due to a logic flaw in the game's transaction processing system.